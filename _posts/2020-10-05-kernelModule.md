---
title: "[임베디드/C]커널 모듈 프로그래밍의 기초"
excerpt: "디바이스 드라이버 만들기 첫번째"
last_modified_at: 2020-10-05T20:45:00+09:00
categories: embedded
tag: ['라즈베리파이','arm']
toc: true
toc_sticky: true
author_profile: false
---
# 커널 모듈 프로그래밍의 기초

> 2020 군장병 SW 역량 강화 교육 "C언어로 배우는 IoT 프로그래밍(유명환)" 강좌의 내용입니다.

프로세스, 파일, 네트워크,  장치 등을 관리하는 사용자에게 편리한 인터페이스를 제공해주는 시스템 소프트웨어이다.

리눅스는 모든 기능이 커널에서 동작한다. 기능을 추가하려면 커널을 수정하여 전체 를 다시 컴파일해야 하는 단점이 있으나, 실시간으로 모듈을 추가할 수 있는 기능을 제공한다(모듈).

## 커널 모듈 예제

``` c
#include <linux/modul.h>
#include <linux/kernel.h>

int init_module(void)
{
	//linux kernel module이 삽입될 때는 항상 init module이라는 함수가 호출된다.
	//아무 모듈이나 삽입하게되면 중요한 영역이 훼손될 수 있기 때문
	//커널 모듈은 메모리에서 실행되기 때문에 커널 메모리에서 찍는다.
	printk("HELLO MODULE loaded \n");
	
	return 0;
}

void cleanup_module(void)
{
	printk("HELLO MODULE unloaded \n");
}
```

이걸 컴파일 하려면

## MAKEFILE

``` c
obj-m := hello_mod.o //means kernel module

KDIR = /usr/src/linux //kernel source directory, you have to download
PWD := $(shell pwd) //쉘명령어로 현제 디렉터리 가져와서 넣음

default:
	$(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules
	/*커널소스 디렉터리를 SUBDIRS로 마운트 시킨 후
	해당 디렉터리로 경로를 바꾼 뒤에 MAKE modules를 한다.*/

//$make clean하면 아래의 명령어가 실행되서  컴파일된  파일이 다 지워진다 
clean:
	rm -rf *.ko
	rm -rf *.mod.*
	rm -rf .*.cmd
	rm -rf .tmp*
	rm -rf *.o
	rm -rf *.order
	rm -rf Module.*
	rm -rf Modules.*
```

하고나면 현재 디렉터리에 hello_mod.ko파일이 있을 것이다.
아직 이 파일은 하드디스크에 있고, 이 모듈을 실행중인 리눅스 커널의 메모리영역으로 삽입하기 위해서는 insmod명령어를 사용해야 한다.

## 모듈 삽입

``` shell
sudo insmod hello_mod.ko
```
실행하면...아무런 변화가  없다.
커널 메모리에 메시지를 던졌기 때문이다.

리눅스 커널 내부의 디버깅 메시지를 보려면 dmesg명령어를  써야 하는데, tail부분만 확인하면 된다.

``` shell
dmesg | tail
```
살펴보면 HELLO MODULE loaded가 보일 것이다. init_module이 호출되었기 때문이다. 그리고 실시간으로 커널 모듈을 확인하는 명령어가 lsmod인데,

``` shell
lsmod
```
하면 used_by가 0인 모듈로 확인될 것이다. 아무도 이  커널 모듈을 사용하고 있지 않을 때, 이 모듈을 삭제할 수 있다. 명령어는 rmmode [module]이다.

``` shell
sudo rmmode hello_module
```
lsmod 해보면 모듈이 삭제되었다는 것을 알  수 있다.
rmmod에 대해 조금만 더 설명하자면,
파일이  메모리에 올라갈 때, 심볼형태로 가는데 rmmod는 그 파일과 관련된 모든 심볼들을 지워주는 역할이다. 실제로, insmod를 하여 파일을 심볼형태로 메모리에 올리면 /proc/kallsyms라는 현재 존재하는 모든 심볼을 기록하는  파일에 기록되는데, rmmod이후에 `cat /proc/kallsyms | [module]`을 해보면 아무것도 안뜨는 것을 볼 수 있을 것이다.

## 심볼의 중첩성 문제

init_module과 cleanup_module은 리눅스에서 제공되는 함수이름이다. 실행중인 커널이 엄청나게 많다면 저 두 함수가 엄청나게 많을 거고, 하나의 프로세스 내에 똑같은 심벌이 있으면(쓰레드로 공유되어있으면) 문제가 생겨 실행이 안되지 않을까.

그래서 리눅스 커널이나 디바이스 드라이버에서 사용되는 함수나 전역변수의 이름 앞에는 해당 디바이스나 커널 '[이름]_'을 붙여야 된다.

``` c
#include <linux/modul.h>
#include <linux/kernel.h>

int hello_init(void)
{
	printk("HELLO MODULE loaded \n");
	
	return 0;
}

void hello_exit(void)
{
	printk("HELLO MODULE unloaded \n");
}
```

이런 식으로.
근데, 위에서 말했듯이 커널 모듈은 중요하기 때문에 명시한 함수이름인 init_module, cleanup_module이 아니면 커널에 올릴 수가 없다. 그래서 위의 두 함수이름을 다시 커널이 요구하는데로 바꿔줘야 한다.

``` c
#include <linux/init.h>
module_init( hello_init );
module_exit( hello_exit );
```
module_init, module_exit 함수 매크로를 사용하여 커널이 삽입되고 퇴출될 때만 커널이 요구하는 문법대로 함수이름을 바꾸어준다.

# 정리

``` c
#include <linux/modul.h>
#include <linux/kernel.h>
#include <linux/init.h>

int hello_init(void)
{
	printk("HELLO MODULE loaded \n");
	
	return 0;
}

void hello_exit(void)
{
	printk("HELLO MODULE unloaded \n");
}

module_init(hello_init);
module_exit(hello_exit);
```

이런 커널모듈의 문법을 따르는 것이 리눅스 디바이스 드라이버이다.

끝