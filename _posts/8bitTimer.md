# 타이머/카운터

입력되는 펄스를 세는 장치이다. 주기가 일정한 펄스가 입력된다며 펄스의 개수를 통해 시간을 측정할 수 있으므로 타이머의 역할도 가능하다. 입력 펄스의 경우 시스템 클록이나 외부에서 주어지는 클록 중에서 선택할 수 있다.

# 8비트 타이머/카운터

0에서 255까지 셀 수 있다. ATmega128의 내부 클록은 16MHz속도로 동작하므로 0에서 시작하여 다시 0으로 돌아오기까지의 시간은 $256/16M = 0.016ms$이다. 0.016ms 이상의 시간을 측정하기 위해 분주기를 사용하여 주기가 긴 클록을 생성한다.

# TCNTn(n = 0, 2) 레지스터

펄스의 수는 TCNTn에 기록된다. 펄스의  개수가 셀 수 있는 최댓값을 넘어서면 0으로 초기화되며 오버플로 인터럽트가 발생한다. 0에서 부터 최댓값까지 도달하는 시간은 클록과 분주비에 의해 결정된다. 클록은 16MHz이고 분주비는 몇 가지 값 중 하나만 쓸 수 있어서 다양한 시간을 측정하기가 어렵다. 그래서 비교 일치 인터럽트를 사용하여 보다 다양한 시간을 측정한다. 비교 일치 인터럽트는 TCNTn 레지스터에 저장된 현재까지의 펄스 개수가 미리 설정된 값과 동일할 경우 발생한다. 미리 설정된 값은 OCRn(n = 0, 2)레지스터에서 읽어온다. 타이머/카운터의 세부 동작을은 TCCRn레지스터를 통해 제어한다.

## LED점멸 예제

``` c
#include <avr/io.h>
#include <avr/interrpt.h>

int count = 0;
int state = 0;

ISR(TIMER0_OVF_vect)
{
  count++;
  if(count == 32) { // overflow 32 happened -> 0.5 second
    count = 0;
    state = !state;
    if(state) PORTB = 0x01; // LED on
    else PORTB = 0x00; // LED off
  }
}

int main(void)
{
  DDRB = 0x01;
  PORTB = 0x00;
  
  // pre-scaler set to 1,024
  TCCR0 |= (1 << CS02) | (1 << CS01) | (1 << CS00);
  
  TIMSK |= (1 << TOIE0); // overflow interrupt allowed
  
  sei(); // global interrupt allowed
  
  while(1){}
  return 0;
}
```