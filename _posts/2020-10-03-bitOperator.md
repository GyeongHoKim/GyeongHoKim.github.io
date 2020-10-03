---
title: "비트연산자, 매크로"
excerpt: "마이크로컨트롤러에서 자주 사용하는 비트연산자와 비트매크로 함수"
last_modified_at: 2020-10-03T20:03:00+09:00
categories: embedded
tag: ATmega128
toc: true
toc_sticky: true
author_profile: false
---
# 마이크로컨트롤러에서 자주 사용되는 비트연산자

입출력장치 등 다양한 장치가 레지스터를 통해 제어되고 이에 따라 비트연산자가 자주 사용된다.

* 비트 세트
* 비트 클리어
* 비트 반전
* 비트 검사/읽기

## 비트 세트

or 연산자를 사용하면 원하는 비트를 1로 만들고 그 이외에는 이전의 값을 유지시키도록 할 수 있다.

```  c
register |= 0b00000100; // 또는
register |= 0x04; //제일 자주 사용하는 코드는
#define registerBit 2
register |= (1 << registerBit);
```

여기서 0x04를 마스크라고 한다.

## 비트 클리어

& 연산자를 사용하면 원하는 비트를 0으로, 그 이외에는 이전의 값을 유지시키도록 할 수 있다.

``` c
register &= 0b00000100; // 또는
register &= 0x04; //제일 자주 사용하는 코드는
#define registerBit 2
register &= ~(1 << registerBit);
```

## 비트 반전

^(xor연산자)연산자를 사용하면 특정 비트를 반전시킬 수 있다.

``` c
register ^= 0b00000100; // 또는
register ^= 0x04; //제일 자주 사용하는 코드는
#define registerBit 2
register ^= (1 << registerBit);
```

## 비트 검사/읽기

&연산자를 사용하면 특정 비트를 읽을 수 있다.

``` c
BIT = register & 0b00000100; // 또는
BIT = register & 0x04; //제일 자주 사용하는 코드는
#define registerBit 2
BIT = register & (1 << registerBit);
```

# 비트 연산 매크로

sfr_defs.h파일에 비트연산 매크로가 몇가지 들어있다.

``` c
#define _BV(bit) (1 << (bit))
#define bit_is_set(sfr, bit) (_SFR_BYTE(sfr) & _BV(bit))
#define bit_is_clear(sfr, bit) (!(_SFR_BYTE(sfr) & _BV(bit)))
```

첫 번째는 원하는 위치에 세트,
두 번째는 원하는 위치의 비트가 세트되어있는지,
세 번째는 원하는 위치릐 비트가 클리어되어있는지 말해준다.  
이외에도 특정 비트가 세트 또는 클리어될 때까지 기다리는 매크로 함수도 있다.

끝