---
title: "[algorithm] 재귀호출을 통한 완전탐색"
excerpt: "재귀, 그리고 완전 탐색, bruteforce"
last_modified_at: 2021-01-26T17:02:00+09:00
categories: algorithm
toc: true
toc_sticky: true
author_profile: false
---

# 완전 탐색

> 모든 경우의 수를 탐색하는 방법

# 재귀 함수

> 자기 자신을 호출하는 함수

# 패턴

재귀호출을 통한 완전탐색 문제를 풀며 일종의 패턴을 찾아냈는데,
처음에는 기저사례를 if문으로 구현하고 재귀호출이 들어가는 for문에서 재귀호출 부분의 코드 앞뒤로 넣고 빼는 코드가 있다는 것이다. $nCm$을 예로 설명하자면 다음과 같다.

![recursive](/assets/images/algorithm/exampleOfRecursive.png)

끝