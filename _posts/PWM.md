# PWM

예전에 동아리에서 아두이노만질 때, PWM 핀으로 LED 밝기를 조정한 적이 있다.

![pwm](https://ko.wikipedia.org/wiki/%ED%8E%84%EC%8A%A4_%ED%8F%AD_%EB%B3%80%EC%A1%B0#/media/%ED%8C%8C%EC%9D%BC:PWM_duty_cycle_with_label.gif)

위키피디아에 PWM치면 나오는 사진인데, 듀티사이클을 조정해서 50%로 하면 LED의 밝기가 절반이 된것 같은 착각을 줌으로써 아날로그 데이터를 표현할 수 있다.
PAM, PCM같은 펄스 변조방식이 있긴한데, PWM은 펄스의 폭으로 아날로그 데이터를 디지털로 변환한다고 보면 된다.
아날로그 데이터를 샘플링하고 그 값들 각각을 듀티사이클로 표현하는 것인데, 100이라는 샘플을 그냥 이진수 배열로 내보내는 다른 방식과 다르게 진짜 비트를 100번 찍어내는 방식때문에 저장공간이 많이 필요하다. 그래서 데이터 저장이나 전송으로는 잘 사용하지 않고 LED 밝기 제어나 모터 속도 제어같은 곳에 많이 쓰인다.

## LED 밝기 조정 예제

``` c
#define F_CPU 16000000L
#include <avr/io.h>
#include <util/delay.h>

#define LED_TIME 20

void turn_on_LED_in_PWM_manner(int dim)
{
	int i;
	
	PORTB = 0x01;
	
	for(i = 0; i < 256; ++i) {
		if(i>dim) PORTB = 0x00;
		_delay_us(LED_TIME);
	}
}

int main()
{
	DDRB = 0x01;
	
	int dim = 0;
	int direction = 1;
	
	while(1) {
		turn_on_LED_in_PWM_manner(dim);
		
		dim += direction;
		
		if(dim == 0) direction = 1;
		if(dim == 256) direction = -1;
	}
	
	return 0;
}
```

위 코드의 경우, turn_on_LED_in_PWM_manner함수가 cpu 클럭의 거의 대부분을 차지하게 되어 다른 작업을 할 수 있는 여지가 없어진다.
하드웨어에 PWM 신호출력을 맡길 수가 있는데, ATmega128에도 아두이노처럼 PWM 신호출력을 파형 생성기에 맡길 수가 있다. 타이머/카운터에 비교일치 인터럽트가 생길 경우, 파형생성기를 통해 신호를 출력할 수가 있는데 이를 이용하면 된다.

## 카운터를 이용한 LED 점멸 예제

``` c
#define F_CPU 16000000L
#include <avr/io.h>
#include <util/delay.h>

int main()
{
	int dim = 0;
	int direction = 1;
	
	DDRB |= (1 << PB4);
	
	TCCR0 |= (1 << WGM01) | (1 << WGM00);
	
	TCCR0 |= (1 << COM01);
	
	TCCR0 |= (1 << CS02) | (1 << CS01) | (1 << CS00);
	
	while(1) {
		OCR0 = dim;
		_delay_ms(10);
		
		dim += direction;
		
		if(dim == 0) direction = 1;
		if(dim == 255) direction = -1;
	}
	
	return 0;
}
```
