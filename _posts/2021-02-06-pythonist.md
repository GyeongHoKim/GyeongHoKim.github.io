---
title: "[Python/Pythonic Code] 파이썬 스타일 코드"
excerpt: "리스트 컴프리헨션, 람다함수, 멥리듀스함수"
last_modified_at: 2021-02-06T00:23:00+09:00
categories: python
tag: python
toc: true
toc_sticky: true
author_profile: false
---

# Pythonic Code

대학교 1학년 시절, 파이썬을 처음 배우고 프로그래밍 별거 없다고 생각했다.  
그러다 딥러닝 스터디에서 텐서플로우를 배웠고 그때부터 슬슬 다른사람이 짠 코드들을 리뷰해주는 기회가 생기게 되었는데,  
후우... for문이 리스트 안에 들어가있고 그 뒤로는 if문에, 처음보는 `__main__`. 장난아니었다. 파이썬으로 코딩을 오래 해 본사람들의 코드는
뭐가뭔지 알아볼 수가 없었다.

나중에, 그러니까 내가 군대에 들어오고 상병이 될 때쯤. 부서에서 정보체계간부님이 읽던 파이썬 책을 습득하게 되었고 그 책에서 Pythonic Code라는 걸 알게 되었다. 리스트 컴프리헨션 예제를 보자마자 딥러닝 스터디에서 느꼈던 그 당혹스러움이 먼저 떠올랐고 그 챕터를 공부하고 나니 희열감이 장난아니었다.
그 희열감을 나누고자 한다.

## List Comprehention

> `[i for i in range(5)]` 가장 기초적인 리스트 컴프리헨션이다.

1. 필터링
    * if문을 for문 뒤에
    * if ~ else문을 쓰고 싶다면 for문 앞에
2. 중첩 반복문
    * for문 연달아서 두 개 작성
    * 필터링 적용가능
3. 이차원 리스트
    * 단순히 곱셈으로 초기화를 하면 같은 객체를 가리키는 참조리스트가 여러개 생길 뿐. 2차원 리스트 초기화를 하고 싶다면 반드시 리스트 컴프리헨션을 사용해야 한다.
    * 대괄호 두 개 사용
    * for문 연달아서 두 개 작성(주의: 대괄호의 위치에 따라 for문의 실행이 달라짐)
        - `[[i + j for i in range(5)] for j in range(5)]`일때 안쪽 대괄호 안의 for문이 그 바깥의 for문 아래로 들어간다고 생각하면 쉽다.

## Lambda function

`(lambda 매개변수 : 연산)(매개변수 값)`하면 연산결과를 그 위치에 반환한다.
`f = lambda x: x ** 2` 이런 식으로 사용하면 f에 그 람다함수의 위치가 담긴다.

## Map Reduce

1. map() function
    * 시퀀스 자료형에서 요소마다 같은 기능을 적용할 때 사용
    * 제너레이터라는 오브젝트를 리턴(리스트가 아니라는 점에 유의할 것! 단, python2버전에서는 리스트를 반환)
    * 리스트 컴프리헨션으로 나타낼 수 있기 때문에 굳이 쓰지는 않으나 제너레이터를 이용하여 속도를 빠르게 하고 싶을 때 사용
    * 필터링 가능, 반드시 else문을 넣어야 동작함
2. reduce() function
    * 시퀀스 자료형에 차례대로 함수 적용하여 **통합**
    
가령
``` python3
#example1 and example2 is same
ex = [1, 2, 3, 4, 5]
f = lambda x, y : x + y
example1 = list(map(f, ex, ex))
example2 = [x + y for x, y in zip(ex, ex)]

#if you want filter,
list(map(lambda x : x ** 2 if x % 2 == 0 else x, ex))
```
로 표현할 수 있다.

끝