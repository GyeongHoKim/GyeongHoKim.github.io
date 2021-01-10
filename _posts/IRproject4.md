# 적외선 송신을 어떻게 할까요

ATmega128의 핀넘버는 다음과 같다.(출처: https://m.blog.naver.com/ga1267/220079623919)
![pinNum](https://mblogthumb-phinf.pstatic.net/20140802_19/ga1267_1406963819868rGijk_PNG/%C4%B8%C3%B3.PNG?type=w2)

ATmega128의 1번 타이머는 16비트 타이머이고 이 타이머/카운터를 이용해서 신호를 송신한다고 가정할 때,  
CTC 모드로 설정한 타이머/카운터에 비교일치 인터럽트를 두 개(하나는 560us마다 인터럽트를 발생하도록, 다른 하나는 38kHz로 인터럽트가 발생하도록)만들어 놓으면 된다.
참고로 분주비를 256으로 하면 1클럭당 16us가 지나는데 35클럭돌면 560us로 562us와 비슷하게 된다. 또, 분주를 안한 상태에서 210번 클럭이 돌았을때 인터럽트가 발생하도록하면 대략 38kHz가 된다.  
출력 핀인 OC1A, OC1B, OC1C는 각각 OCR1A, OCR1B, OCR1C에 따른 인터럽트를 출력하게 된다. 문제는, 우리는 하나의 핀이 두 개의 인터럽트를 동시에 적용되도록 해야 한다는 점이다.  


가령 `printIR()`이라는 함수를 구현한다고 가정해보자. 그 함수의 매개변수로는 내가 미리 디코딩한 NET format 코드가 배열로 들어갈 것이다.  
함수 내부에서 논리 0, 논리 1, 리드 코드의 경우를 따진다. 평소에는 OCR1A를 0로 두어 파형이 생성되지 않게 하고 출력이 시작 될 때, 34로 고치면 된다.  
volatile int형 cnt 변수를 하나 선언하고 파형이 두 번 반복되었을 때 한 번 더 cnt를 1로 둬서 HIGH가 나오는 시기를 한 타임 늦추면 될 것 같다. 

## 1번 타이머/카운터 설정

``` c
// TCCR1A = _BV (WGM12); OCR1A  = 34; TIMSK1 = _BV (OCIE1A); TCCR1B =  _BV (CS21) | _BV (CS22); 해야 함
// TCCR1A |= _BV(COM1A0) || _BV(COM1A1); 해놓으면 처음 인터럽트가 발생 할 때 HIGH가 출력된다.
volatile int cnt = 0;

ISR(TIMER1_COMPA_vect)
{
  if(cnt == 0)
    TCCR1A |= _BV(COM1A1) && ~_BV(COM1A0);    
  else
    TCCR1A |= _BV(COM1A0) || _BV(COM1A1);
	++cnt;
	cnt %= 2;
}
```
여기까지 해두면 560us마다 인터럽트가 걸리게 하는 건 끝났다.

두 번째로, 38kHz 반송파에 싣는 것인데 아마 TCCR1B에 COM1Bn 비트들을 디폴트로 놔두면 OC1B에서 출력이 되지 않을 것이고  
인터럽트가 발생 할 때마다 OC1A핀의 출력을 toggle하면 될 것 같다.

``` c
//TCCR1A = 0;
//TCCR1B = _BV(WGM12) | _BV (CS10);
//OCR1A =  209;

ISR(TIMERS1_COMPB_vect)
{
  PORTB ^= (1 << 5);
}
```

oh, shit here we go again.