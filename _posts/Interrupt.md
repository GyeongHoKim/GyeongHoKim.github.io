---
title: "[임베디드/C]AVR에서 인터럽트(Interrupt)방식 사용하기"
excerpt: "ATmega128에서 구현하는 인터럽트 방식 프로그래밍"
last_modified_at: 2020-10-09T18:39:00+09:00
categories: embedded
tag: ['ATmega128','avr']
toc: true
toc_sticky: true
author_profile: false
---
# 인터럽트방식과 폴링방식

앞선 포스트에서 마이크로컨트롤러를 프로그래밍할 때, main함수에 무한루프를 넣어 이벤트가 발생하는지를 계속해서 소프트웨어적으로 감시하고, 특정 이벤트가 발생하면 동작을 수행하는 방식을 많이 사용했다. 이와 같은 방식을 폴링(Polling)방식이라 한다. 그런데 다음 코드를 살펴보자

``` c
int main(void)
{
    while(1)
    {
        // read button status
        // judge button status
        // print LED1 for button status
        
        // UART character recieved
        // compare recieved characteer
        // print LED2 for button status
    }
    return 0;
}
```
