---
title: "[임베디드/C] 유저 어플리케이션 작성하기"
excerpt: "디바이스 드라이버 만들기 세번째"
last_modified_at: 2020-10-06T20:55:00+09:00
categories: embedded
tag: ['라즈베리파이','arm']
toc: true
toc_sticky: true
author_profile: false
---
# 유저 어플리케이션

> 2020 군장병 SW 역량 강화 교육 "C언어로 배우는 IoT 프로그래밍(유명환)" 강좌의 내용입니다.

우리가 앞서 만들었던 디바이스 드라이버를 실제로 사용하게 만드는 유저 어플리케이션을 살펴보자.

# 소스코드 전문

``` c

/**
 **     SKELETON User Application Example for Raspberry Pi
 **
 **/

#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <fcntl.h>

static unsigned short flag;

int main(void)
{
	int fd,i;
	int key;
	char *buffer1 = "EFGH";
	char buffer2[256];

	fd = open("/dev/SKELETON", O_RDWR);

	if (fd < 0)
	{
    		printf("USERAPP: /dev/SKELETON isn't opened! \n");
		//exit(1);
		return 1;
	}
    	else
	    	printf("USERAPP: /dev/SKELETON is opened! \n");

    	while((key = main_menu()) != 0)
	{
		switch(key)
		{
	   		case '1':	
				printf("USERAPP: Keyboard [1] is clicked! \n");
				ioctl(fd, 1, flag);
				break;
           		case '2':
                		printf("USERAPP: Keyboard [2] is clicked! \n");
				ioctl(fd, 2, flag);
                		break;
           		case '3':
                		printf("USERAPP: Keyboard [3] is clicked! \n");
				ioctl(fd, 3, flag);
				break;
           		case '4':
                		printf("USERAPP: Keyboard [4] is clicked! \n");
				ioctl(fd, 4, flag);
                		break;
           		case 'w':
                		printf("USERAPP: write() is called! \n");
				write(fd, buffer1, 5);
                		break;
           		case 'r':
                		printf("USERAPP: read() is called! \n");
				read(fd, buffer2, 5);
				printf("USERAPP: Device read data = %s \n", buffer2);
                		break;
           		case 'q':
                		printf("USERAPP: User Application is closed! \n");
				close(fd);
				//exit(-1);
				return 0;
                		break;
                }
        }

        return 0;
} 


int main_menu(void)
{
        int key;
        char input[2];
        printf("\n\n");
        printf("********** [USERAPP] Menu ***********\n");
        printf("* 1.  USERAPP: Keyboard [1]         *\n");
        printf("* 2.  USERAPP: Keyboard [2]         *\n");
        printf("* 3.  USERAPP: Keyboard [3]         *\n");
        printf("* 4.  USERAPP: Keyboard [4]         *\n");
        printf("* w.  USERAPP: write() execute      *\n");
        printf("* r.  USERAPP: read() execute       *\n");
	printf("*                                   *\n");
        printf("* q.  USERAPP: Quit                 *\n");
        printf("*************************************\n");
        printf("\n\n");

	printf("USERAPP: Select the command number : ");
	key = getchar();
	getchar();
	printf("\n\n");

        return key;
}
```

## buffer1, buffer2

``` c
char *buffer1 = "EFGH";
char buffer2[256];
```
첫 번째 버퍼의 경우 디바이스에 전달하고자 하는 데이타, 두 번째의 경우 디바이스로부터 전달 받은 데이타를 담을 공간이다.

## open()

> `fd = open("/dev/SKELETON", O_RDWR);`

c 프로그래밍을 할 때 fopen()함수를 자주 사용했지만,  stdio.h파일을 사용할 수 없는 low level programming의 경우 시스템 콜을 더 자주 쓴다. 이 함수 이전에 이미 /dev/SKELETON파일이 생성되있어야 한다.

## 이외의 것들

이후부터는 메인메뉴를 호출하는 내용이다. 숫자를 누르는 경우에는 단순히 io control함수를, 읽거나 쓰는 경우, read나 write를 사용한다.

``` c
 while((key = main_menu()) != 0)
{
	switch(key)
	{
	  	case '1':	
			printf("USERAPP: Keyboard [1] is clicked! \n");
			ioctl(fd, 1, flag);
			break;
           	case '2':
                	printf("USERAPP: Keyboard [2] is clicked! \n");
			ioctl(fd, 2, flag);
                	break;
           	case '3':
                	printf("USERAPP: Keyboard [3] is clicked! \n");
			ioctl(fd, 3, flag);
			break;
           	case '4':
                	printf("USERAPP: Keyboard [4] is clicked! \n");
			ioctl(fd, 4, flag);
                	break;
           	case 'w':
                	printf("USERAPP: write() is called! \n");
			write(fd, buffer1, 5);
                	break;
           	case 'r':
                	printf("USERAPP: read() is called! \n");
			read(fd, buffer2, 5);
			printf("USERAPP: Device read data = %s \n", buffer2);
                	break;
           	case 'q':
                	printf("USERAPP: User Application is closed! \n");
			close(fd);
			//exit(-1);
			return 0;
                	break;
            }
    }

    return 0;
} 
```
이전에도 말했지만, 유저 어플리케이션의 함수들은 직접 디바이스 드라이버에 접근할 수 없으므로 모두 디바이스 드라이버를 통해 데이타를 전달받는다.

# 실행

> sudo insmod skeleton.ko
make로 만든 커널 모듈을 삽입시킨 후,

> sudo mknod /dev/SKELETON[sort of device] [major number] [minor number]

디바이스의 종류는 캐릭터 디바이스.
dmesg를 통해 메이저 넘버를 확인하고, 마이너 넘버는 이때까지 겹치는 디바이스가 없을 테니 0이다.
이를 통해 디바이스파일, 노드를 만들었다.
이후 `dir /dev/SKELETON`을 해보면 맨 왼쪽에 c가 적혀있는 것을 볼 수 있는데, 이는 inode값이다.
파일이면 f, Block Device면 b가 적힐 것이다. 쭉 읽어보면 메이저 번호와 마이너 번호도 확인할 수 있을 것이다.

> sudo ./userapp

접근권한은 디바이스를 만져야하니 루트권한이어야 한다. 정상적으로 실행되는 것을 확인할 수 있다.

# 실제 개발에는?

커널 소스 전부를 재컴파일하기가 매번 힘들기 때문에 리눅스의 커널모듈을 이용하여 디바이스 드라이버를 만들었다. 디버깅이 끝나면, 이 디바이스 드라이버는 커널 소스에 포함시켜서 포팅한다. 부팅할때, 메모리에 상주시키는 것이다.

끝