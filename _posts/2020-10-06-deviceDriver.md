---
title: "[임베디드/C] 디바이스 드라이버 작성하기"
excerpt: "디바이스 드라이버 만들기 두번째"
last_modified_at: 2020-10-06T18:30:00+09:00
categories: embedded
tag: ['라즈베리파이','arm']
toc: true
toc_sticky: true
author_profile: false
---
# 디바이스 드라이버

> 2020 군장병 SW 역량 강화 교육 "C언어로 배우는 IoT 프로그래밍(유명환)" 강좌의 내용입니다.

이전에 커널모듈 문법을 알아보았다. 커널모듈 중, 실제 디바이스를 제어하는 것들이 디바이스 드라이버다. 디바이스 드라이버는 혼자서 실행되지 않는다. 그래서 디바이스 드라이버를 불러오게끔하는 유저 어플리케이션을 쌍으로 둬야 우리가 만든 디바이스가 효능을 발휘할 것이다.

# 리눅스 디바이스의 구분

다음 목차 디바이스 드라이버 소스 전문 맨 끝 줄부근을 보면 `module_init(skeleton_init)`이라고 되어있다. 커널 삽입이 이루어졌을 때 처음 실행되는 함수가 skeleton\_init이다. 

``` c
int skeleton_init(void)
{
	int result;

	//printk 함수는 우선순위를 포함할 수 있다(KERN_INFO)
	printk(KERN_INFO "SKELETON: skeleton_init() is called.. \n");

	result = register_chrdev(SKELETON_MAJOR, "SKELETON", &skeleton_fops);

	if (result < 0) {
        	printk(KERN_WARNING "SKELETON: \t Can't get major number! \n");
        	return result;
    	}

    	if(SKELETON_MAJOR == 0)
		SKELETON_MAJOR = result;

	printk("SKELETON: SKELETON_MAJOR = %d \n", SKELETON_MAJOR);

	return 0;
}
```

디바이스를 정의하는 데 필요한 항목이 세 가지가 있다.
> 그룹, 메이저 번호(다른 종류 디바이스 구분), 마이너 번호(같은 종류 디바이스  구분)

## 그룹

* character
* block
* network
리눅스 디바이스는 이 세가지 그룹으로 나뉜다. 일반적으로 디바이스라 하면 Character나 Block Device를 말하고  
> Character 디바이스는 다만 버퍼를 거치지 않는다.
이는, 데이타를 미리 받아놓고 순서를 바꿀 수가 없으며 무조건 순서대로 제어되는 디바이스임을 뜻한다. 보통 우리 주변에서 제일 많이 찾 수 있는 디바이스가 이 Character Device이다. 가령 키보드라던가.

## register\_*dev();

어플리케이션이 운영체제에 특정 디바이스를 쓰겠다고 요청한다고 해보자.
하지만 어플리케이션이 커널 내부에 직접 들어갈 수는 없다. 그러므로
해당 디바이스와 매핑되어있는 디바이스 파일이라는 특정 노드를 오픈하는 행위를 통해
운영체제에 이 디바이스를 쓰겠다고 알리게 된다.
그러면 운영체제는 자신에게 등록된 디바이스 드라이버를 검색한다.

등록되지 않은 드라이버는 운영체제에게 검색되지 않는다. 즉, 디바이스 드라이버가 실행되기 위해서는 **디바이스 드라이버가 자신을 운영체제에 등록**하는 과정이 필요하다.
이 역할을 `register_chardev` `register_blkdev` `regist_netdev`함수가 맡는다.

---

이 함수들의 반환값은 0, 양수, 음수이다.
* 0인 경우: 인자로 전달된 메이저넘버를 사용해도 문제가 없음이 확인되었을 때
* 양수인 경우: 메이저넘버를 인자로 주지않았을 때, os가 알아서 메이저 넘버를 하나 반환해 준다
* 음수인 경우: 에러

첫 번째 인자는 내가 제어할 디바이스를 구분하기 위한 메이저 번호

``` c
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/init.h>
#include <linux/errno.h>

#include <linux/fs.h>	/** ... for MAJOR(), MINOR() macro **/

#include <asm/uaccess.h>  /** ... for copy_to,from_user() **/
#include <asm/io.h>	/** ... for in/b/w/l(), out/b/w/l/() **/

#include <linux/slab.h>		/** ... for kmalloc() ... **/

int SKELETON_MAJOR = 0;
```
/proc/devices에 디바이스 메이저 번호들이 적혀있는데, 여기 적혀있지 않은 번호를 찾아서 기입한다. 그리고 그 번호가 유효하다면, register\_*dev()의 반환값이 0이 나올 것이다. 위의 코드에는 메이저번호를 0으로 줬는데, 이는 운영체제에서 유효한 메이저 번호를 반환해 달라는 뜻이다.

두 번째 인자는 어플리케이션과 연동하기 위한 인터페이스 즉, 디바이스파일의 이름이다(디바이스파일을 오픈하기 위해). 절대경로도 가능하다.

세 번째 마지막 인자는

``` c
struct file_operations skeleton_fops = {
	.open		= skeleton_open,
	.release	= skeleton_release,
	.read		= skeleton_read,
	.write		= skeleton_write,
	.unlocked_ioctl		= skeleton_ioctl,	
	/** In newer kernels, use .unlocked_ioctl in the place of .ioctl. **/
};
```
이렇게 시스템콜과 함수의 일대일 매핑관계를 지정한 구조체를 전달한다.

정리하자면, 디바이스를 구분하기 위한 메이저넘버, 어플리케이션 연동을 위한 디바이스 파일의 이름, 연동 이후 사용할 함수들의 이름을 전달하는 것이다.

## unregister\_*dev();

커널모듈의 실행 이후, module\_exit()부분에서 등록된 디바이스를 지워야한다. 그 역할을 하는 것이 unregister\_*dev()함수이다.

# skeleton device를 제어하는 가상의 드라이버 만들기

일단 드라이버 전문을 보여주고, 하나하나 설명하겠다.

``` c
/**
 **	SKELETON Device Driver Example for Raspberry Pi
 ** 
 **/
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/init.h>
#include <linux/errno.h>

#include <linux/fs.h>	/** ... for MAJOR(), MINOR() macro **/

#include <asm/uaccess.h>  /** ... for copy_to,from_user() **/
#include <asm/io.h>	/** ... for in/b/w/l(), out/b/w/l/() **/

#include <linux/slab.h>		/** ... for kmalloc() ... **/

int SKELETON_MAJOR = 0;

int skeleton_open (struct inode *inode, struct file *filp)
{
	printk(KERN_INFO "SKELETON: skeleton_open() is called.. \n");

	printk(KERN_INFO "SKELETON: \t Major number = %d \n", MAJOR(inode->i_rdev));
	printk(KERN_INFO "SKELETON: \t Minor number = %d \n", MINOR(inode->i_rdev));
	
	return 0; 
}

int skeleton_release (struct inode *inode, struct file *filp)
{
	printk("SKELETON: skeleton_release() is called.. \n");

	return 0;
}

ssize_t skeleton_read (struct file *filp, char *buf, size_t count, loff_t *f_pos)
{
	char *dev_data = "ABCD";
	int err;

	// dev_data = kmalloc(count, GFP_KERNEL);
	// because 'dev_data' has initial data("ABCD"), we don't have to kmalloc

	if( (err = copy_to_user(buf, dev_data, count)) < 0 )
		return err;
	/**
	 **	copy_to_user(to, from, n)
	 **/
	
	printk(KERN_INFO "SKELETON: skeleton_read() is called.. \n");

	// kfree(dev_data);

	return count;
}

ssize_t skeleton_write (struct file *filp, const char *buf, size_t count, loff_t *f_pos)
{
	char *dev_data;
	int err;

	dev_data = kmalloc(count, GFP_KERNEL);

	if( (err = copy_from_user(dev_data, buf, count)) < 0 )
		return err;
	/**
	 **	copy_from_user(to, from, n)
	 **/
	
	printk(KERN_INFO "SKELETON: skeleton_write() is called.. \n");
	printk(KERN_INFO "SKELETON: \t User write data = %s \n", dev_data);

	kfree(dev_data);

	return count;
}

int skeleton_ioctl(struct inode *inode, struct file *filp, unsigned int cmd, unsigned long arg)
{
	printk(KERN_INFO "SKELETON: skeleton_ioctl() is called.. \n");

        switch(cmd)
        {
 		case 1:  
                {
			printk("\n");
			printk("SKELETON: Keyboard = [1] \n");
                      break;
                }
                case 2:
                {
			printk("\n");
			printk("SKELETON: Keyboard = [2] \n");
                       break;
                }
                case 3:
                {
			printk("\n");
			printk("SKELETON: Keyboard = [3] \n");
                       break;
                }
                case 4:
                {
			printk("\n");
			printk("SKELETON: Keyboard = [4] \n");
 		       break;
                }
                                          
                default:
                        return 0;
        }
        return 0;
}

		
struct file_operations skeleton_fops = {
	.open		= skeleton_open,
	.release	= skeleton_release,
	.read		= skeleton_read,
	.write		= skeleton_write,
	.unlocked_ioctl		= skeleton_ioctl,	
	/** In newer kernels, use .unlocked_ioctl in the place of .ioctl. **/
};


int skeleton_init(void)
{
	int result;

	printk(KERN_INFO "SKELETON: skeleton_init() is called.. \n");

	result = register_chrdev(SKELETON_MAJOR, "SKELETON", &skeleton_fops);

	if (result < 0) {
        	printk(KERN_WARNING "SKELETON: \t Can't get major number! \n");
        	return result;
    	}

    	if(SKELETON_MAJOR == 0)
		SKELETON_MAJOR = result;

	printk("SKELETON: SKELETON_MAJOR = %d \n", SKELETON_MAJOR);

	return 0;
}

void skeleton_exit(void)
{
	unregister_chrdev(SKELETON_MAJOR, "SKELETON");
}

module_init(skeleton_init);
module_exit(skeleton_exit);

MODULE_LICENSE("Dual BSD/GPL");
```

## int skeleton\_open();

디바이스 파일을 오픈했을 때 이 함수가 호출될 것이다.

``` c
int skeleton_open( struct inode *inode, strct file *filp)
{
	printk(KERN_INFOR "SKELETON: skeleton_open() is called.. \n");
	
	printk(KERN_INFO "SKELETON: \t Major number = %d \n", MAJOR(inode->dev));
	printk(KERN_INFO "SKELETON: \T Minor numbber = %d \n", MINOR(inode->i_rdev));
	//MAJOR()과 MINOR()는 각각 메이저 넘버와 마이너 넘버를 보여주는 매크로
	
	return 0;
}
```

## int skeleton\_release();

어플리케이션 종료 시 호출된다.

## ssize\_t skeleton\_read();

유저 어플리케이션은 디바이스에 직접 접근할 수 없으므로 디바이스 드라이버가 직접 디바이스 메모리 버퍼에서 데이터를  읽어온다. 이후, 그 내용을 카피하여 유저 어플리케이션에게 전달해준다(copy\_to\_user();).

``` c
ssize_t skeleton_read (struct file *filp, char *buf, size_t count, loff_t *f_pos)
{
	char *dev_data = "ABCD";
	int err;

	// dev_data = kmalloc(count, GFP_KERNEL);
	// because 'dev_data' has initial data("ABCD"), we don't have to kmalloc

	if( (err = copy_to_user(buf, dev_data, count)) < 0 )
		return err;
	/**
	 **	copy_to_user(to, from, n)
	 **/
	
	printk(KERN_INFO "SKELETON: skeleton_read() is called.. \n");

	// kfree(dev_data);

	return count;
}
```

kmalloc()함수로 메모리를 할당한 다음, inb() 등의 함수로 실제 디바이스의 데이터를 읽어와서 저장해야 한다(여기서는 가상 디바이스이므로 임의의 값을 이미 가지고 있다고 생각하고 시작).
이후, copy\_to\_user 함수를 이용하여 데이타를 복사하여 유저 스페이스에 던진다.

## ssize\_t skeleton\_write();

유저 어플리케이션은 디바이스를 직접 조작할 수 없으므로 디바이스 드라이버에 데이타를 전달해주고, 드라이버는 그 내용을 복사하여 kmalloc으로 할당한 메모리에 받아온다. 이후 outb()등의 함수로 실제 디바이스에 데이타를 쓴다. 그래서 여기는 copy\_from\_user함수를 사용한다.

``` c
ssize_t skeleton_write (struct file *filp, const char *buf, size_t count, loff_t *f_pos)
{
	char *dev_data;
	int err;

	dev_data = kmalloc(count, GFP_KERNEL);

	if( (err = copy_from_user(dev_data, buf, count)) < 0 )
		return err;
	/**
	 **	copy_from_user(to, from, n)
	 **/
	
	printk(KERN_INFO "SKELETON: skeleton_write() is called.. \n");
	printk(KERN_INFO "SKELETON: \t User write data = %s \n", dev_data);

	kfree(dev_data);

	return count;
}
```

## int skeleton\_ioctl();

io control함수는 디바이스에 메모리가 없을 때 사용한다. 그래서 switch case문을 이용하여 각각의 경우에 대한 함수를 적어놓는다.

``` c
int skeleton_ioctl(struct inode *inode, struct file *filp, unsigned int cmd, unsigned long arg)
{
	printk(KERN_INFO "SKELETON: skeleton_ioctl() is called.. \n");

        switch(cmd)
        {
 		case 1:  
                {
			printk("\n");
			printk("SKELETON: Keyboard = [1] \n");
                      break;
                }
                case 2:
                {
			printk("\n");
			printk("SKELETON: Keyboard = [2] \n");
                       break;
                }
                case 3:
                {
			printk("\n");
			printk("SKELETON: Keyboard = [3] \n");
                       break;
                }
                case 4:
                {
			printk("\n");
			printk("SKELETON: Keyboard = [4] \n");
 		       break;
                }
                                          
                default:
                        return 0;
        }
        return 0;
}
```

끝