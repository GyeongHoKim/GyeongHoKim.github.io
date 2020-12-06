---
title: "[STL/C++] 원소를 수정하지 않는 알고리즘"
excerpt: "내가 보려고 만든 C++ STL 알고리즘 컨테이너 1편"
last_modified_at: 2020-12-03T22:55:00+09:00
categories: STL
toc: true
toc_sticky: true
---

# 원소를 수정하지 않는 알고리즘

적으면서 외우자!

| 알고리즘 | 설명(p는 구간 [b,e)의 반복자) |
| :---: | :---: |
| `p = adjacent_find(b,e)` | p는 구간의 원소 중 `*p == *(p+1)`인 첫 원소를 가리키는 반복자 |
| `p = adjacent_find(b,e,f)` | p는 구간의 원소 중 `f(*p, *(p+1))`이 참인 첫 원소를 가리키는 반복자 |
| `n = count(b,e,x)` | n은 구간의 원소 중 x 원소의 개수 |
| `n = count_if(b,e,f)` | n은 구간의 원소 중 `f(*p)`가 참인 원소의 개수 |
| `equal(b,e,b2)` | 두 구간의 원소가 모두 같은가 |
| `equal(b,e,b2,f)` | 두 구간의 모든 원소가 `f(*p, *q)`를 만족하는가 |
| `p = find(b,e,x)` | p는 구간에서 x와 같은 첫 원소의 반복자 |
| `p = find_end(b,e,b2,e2)` | p는 첫 번째 구간의 순차열 중 두 번째 구간의 순차열과 일치하는 순차열 첫 원소의 반복자 |
| `p = find_end(b,e,b2,e2,f)` | p는 첫 번째 구간의 순차열 중 두 번째 구간의 순차열과 f를 만족하는 순차열 첫 원소의 반복자 |
| `p = find_first_of(b,e,b2,e2)` | p는 첫 번재 구간에서 두 번째 구간의 원소 중 같은 원소가 발견된 첫 원소의 반복자 |
| `p = find_first_of(b,e,b2,e2,f)` | p는 첫 번째 구간의 원소 중 두 번째 구간의 원소와 f를 만족하는 첫 원소의 반복자 |
| `p = find_if(b,e,f)` | p는 구간에서 `f(*p)`를 만족하는 첫 원소를 가리키는 반복자 |
| `f = for_each(b,e,f)` | 구간의 모든 원소에 f를 적용한다. f를 다시 되돌려 준다 |
| `lexicographical_compare(b,e,b2,e2)` | 첫 번째 구간의 순차열이 두 번째 구간의 순차열보다 작다면 true |
| `lexicographical_compare(b,e,b2,e2,f)` | 첫 번째 구간의 순차열이 두 번째 구간의 순차열보다 작다면 true, 해당 순차열은 f를 만족 |
| `k = max(a,b)` | k는 둘 중 더 큰 것 |
| `k = max(a,b,f)` | k는 둘 중 더 큰 것. 이 때 큰 것은 f(a,b)를 적용 |
| `p = max_element(b,e)` | p는 구간에서 가장 큰 원소의 반복자 |
| `p = max_element(b,e,f)` | p는 구간에서 가장 큰 원소의 반복자. 이때 비교는 f를 사용 |
| `k = min(a,b)` | k는 둘 중 더 작은 것 |
| `k = min(a,b,f)` | k는 둘 중 더 작은 것. 이 때 작은 것은 f(a,b)를 사용 |
| `p = min_element(b,e)` | p는 구간에서 가장 작은 원소의 반복자 |
| `p = min_element(b,e,f)` | p는 구간에서 가장 작은 원소의 반복자. 이때 비교는 f를 사용 |
| `pair(p,q) = mismatch(b,e,b2)` | (p,q)는 두 구간에서 `!(*p==*q)`인 첫 원소를 가리키는 반복자의 쌍 |
| `pair(p,q) = mismatch(b,e,b2,f)` | (p,q)는 두 구간에서 `!f(*p,*q)`가 참인 첫 원소를 가리키는 반복자의 쌍 |
| `p = search(b,e,b2,e2)` | p는 첫 번째 구간의 순차열 중 두 번째 구간의 순차열과 일치하는 순차열 첫 원소의 반복자 |
| `p = search(b,e,b2,e2,f)` | p는 첫 번째 구간의 순차열 중 두 번째 구간의 순차열과 f가 적용되는 순차열 첫 원소의 반복자 |
| `p = search_n(b,e,n,x)` | p는 구간의 원소 중 x값이 n개 연속한 첫 원소의 반복자 |
| `p = search_n(b,e,n,x,f)` | p는 구간의 원소 중 `f(*p,x)`가 참인 값이 n개 연속한 첫 원소의 반복자 |

각 알고리즘들에 대한 설명은 [여기](https://github.com/GyeongHoKim/algorithm) 
알고리즘 관련된 것들을 정리하는 내 깃허브 리포지토리에 있다.

# 원소를 수정하는 알고리즘

| 알고리즘 | 설명(p는 구간 [b,e)의 반복자) |
| :---: | :---: |
| `p = copy(b,e,t)` | 구간의 원소를 [t,p)로 모두 복사 |
| `p = copy_backward(b,e,t)` | 구간의 원소를 마지막 원소로부터 [p,t)로 모두 복사 |
| `fill(b,e,x)` | 구간의 모든 원소를 x로 채운다 |
| `fill_n(b,n,x)` | 구간 [b, b+n)의 모든 원소를 x로 채운다 |
| `f = for_each(b,e,f)` | 구간의 모든 원소에 `f(*p)`동작을 적용, f를 되돌려준다 |
| `generate(b,e,f)` | 구간의 모든 원소를 f()로 채운다 |
| `generate_n(b,n,f)` | 구간의 모든 원소를 f()로 채운다 |
| `iter_swap(p,q)` | 반복자가 가리키는 `*p`와 `*q`의 원소를 교환한다 |
| `p = merge(b,e,b2,e2,t)` | 정렬된 순차열 둘을 [t, p)로 합병 정렬한다 |
| `p = merge(b,e,b2,e2,t,f)` | 정렬된 순차열 둘을 [t, p)로 합병 정렬한다. f로 비교 |
| `replace(b,e,x,x2)` | 구간의 x인 원소를 x2로 수정한다 |
| `replace(b,e,f,x2)` | 구간에서 f가 참인 원소를 x2로 수정 |
| `p = replace_copy(b,e,t,x,x2)` | 구간의 x인 원소를 x2로 수정하여 [t,p)로 복사 |
| `p = replace_copy(b,e,t,f,x2)` | 구간에서 f가 참인 원소를 x2로 수정하여 [t,p)로 복사 |
| `swap(a, b)` | a와 b를 교환한다 |
| `swap_ranges(b,e,b2)` | 구간의 원소와 구간 [b2, b2+(e-b))의 원소를 교환한다 |
| `p = transform(b,e,t,f)` | 구간의 모든 원소를 `f(*p)`하여 두 번째 구간에 저장한다. p는 저장된 마지막 원소의 반복자이다 |
| `p = transform(b,e,b2,t,f)` | 두 구간의 모든 원소를 f하여 t로 시작하는 구간에 저장한다. p는 저장된 마지막 원소의 반복자이다 |