# PWM으로 38kHz 반송파에 신호 보내기

이건 구글링하다가 찾은 아두이노로 38kHz만드는 코드인데 

``` c
// 메인함수에 해당하는 setup와 loop 함수부터 보는게 국룰


const byte LED = 9;  // Timer 1 "A" output: OC1A 아마 이게 OC1A 핀인가벼

// 2번 타이머 비교일치 인터럽트 발생할때 벡터테이블에서 일로 점프, 즉 1ms지날 때 이리로 콤
ISR (TIMER2_COMPA_vect)
{
  // COM1A0,1은 OC1A핀의 출력을 제어한다. 디폴트는 둘 다 0이고 처음에 0일테니 COM1A0이 세트되어 핀의 출력이 반전이 됨.
  // 두번째 부터는 조금 다른데 COM1A0가 1인 상태에서 XOR하면 0으로 돌아감. 즉, 켜져있으면 끄고 꺼져있으면 킴.
  TCCR1A ^= _BV (COM1A0) ;  // Toggle OC1A on Compare Match
  
  //COM1A0비트가 0이라면
  if ((TCCR1A & _BV (COM1A0)) == 0)
    LED를 끈다
    digitalWrite (LED, LOW);  // ensure off LED의 초기상태에 따라 달라질 수 있으니 0일 때 끄도록 확실히함
   
}  // end of TIMER2_COMPA_vect

void setup() {
//아래의 핀모드 함수는 선택된 핀을 출력으로 설정한다.
 pinMode (LED, OUTPUT);
 // TCCR1A,B 1번 3번 타이머는 16비트 타이머인데 TCCR1A에 0을 넣는건 어차피 디폴트가 0이라 넘겨도 되고 TCCR1B의 값이 중요한데
 // WGM12에 1을 넣겠다는 건 CTC모드에 TOP은 OCR1A안에 있는 값으로 하겠다는 거고 CS10에 셋하겠다는 건 분주비를 1로 잡겠다는 것임.
 // set up Timer 1 - gives us 38.095 MHz (correction: 38.095 KHz)
 TCCR1A = 0;
 TCCR1B = _BV(WGM12) | _BV (CS10);   // CTC, No prescaler
 // 비교일치값인데 0부터 세니깐 총 210클럭이 돌면 비교일치 인터럽트를 내겠다는 거고(그니까 이 경우엔 펄스를 떨어뜨린다는 뜻)
 // 클럭이 16Mhz(아두이노 이거 뭔모델인지 모르겠는데 16Mhz라고 되어있음. ATmega128도 16메가, 개꿀)니까 1클럭당 1/16000000초
 // 210/(16*10^6) = 13.125ns 당 비교일치가 발생한다. 펄스 떨어뜨리는데 13.125니까 펄스 하나 온전하게는 즉, 주기가 26.25ns
 // 주기가 26.25ns면 주파수는 1/26.25n = 38.095k니까 38kHz에 근접하다. 여기까지가 1번 타이머 이용한 것.
 OCR1A =  209;          // compare A register value (210 * clock speed)
                        //  = 13.125 nS , so frequency is 1 / (2 * 13.125) = 38095
 
 // Timer 2 - gives us our 1 mS counting interval
 // 16 MHz clock (62.5 nS per tick) - prescaled by 128
 //  counter increments every 8 uS.
 // So we count 125 of them, giving exactly 1000 uS (1 mS)
 // 위에 주석읽어보니까 대충 1ms당 인터럽트 거는것 가틈. 카운터가 8us당 증가하고 125를 카운트하면 1ms가 된데.
 TCCR2A = _BV (WGM12) ;   // CTC mode 이건 1번 카운터 설정한거 보면 됨, CTC모드에 TOP은 OCR2A값...인데 TCCR2A인거 보니까 아두이노는 카운터가 전부 16비트인가베 엄머나 세상에 코드 좀 고쳐야쓰겄네
 OCR2A  = 124;            // count up to 125  (zero relative!!!!) 0부터 세니까 125클럭 돌면 비교일치 발생
 // 두 줄 밑에 보면 분주비를 128로 잡으니깐 실질적인 클럭 주파수는 16M/128hz고 1클럭당 128/16 ns = 8ns 지난다. 125클럭돌면 1ms지남
 TIMSK2 = _BV (OCIE2A);   // enable Timer2 Interrupt 타이머 인터럽트 허용시키는 거
 TCCR2B =  _BV (CS20) | _BV (CS22) ;  // prescaler of 128 분주비 128로 설정. 여기까지가 2번 타이머 이용(아두이노에서는 16비트 타이머인듯)

}  // end of setup

void loop()
 {
 // all done by interrupts
 }
 ```
 
 2번 타이머에 대해서 ISR을 설정했으므로 1ms당 깜빡이는 건데, 1번 타이머에 대한 ISR을 만들어 놓으면 38kHz 주파수로 깜빡이게 될 것이고 volatile로 플래그 변수를 선언해서 2번 타이머에 의해 켜져있는 상태에서만 1번 타이머의 ISR에서 LED를 제어할 수 있도록 1번과 2번 타이머의 ISR내부에 if문을 걸어두거나 플래그 변수를 수정하도록 하면 우리가 원하는 NET code를 출력하게 만들 수 있을 것이다.
 
 일단 내가 원하는 건 아두이노가 아니라 ATmega128이니깐 16비트 타이머인 1번, 3번 타이머를 쓰도록 수정해야 한다. 또, 1ms당 깜빡이는게 아니라 논리 0의 경우 562us당, 논리 1의 경우에는 562us는 켰다가 이후 1675us(약 562us 두 번)동안은 끄고 있어야 한다. 데이터전송부 말고도 리드코드를 전송할 때는 또 다르다. 일반 데이터의 경우 9ms동안 켰다가 4.5ms동안 꺼야 하고 연속 데이터의 경우 9ms 켰다가 2.25ms동안 꺼야 한다. 플래그 변수가 좀 많이 필요하겠군.