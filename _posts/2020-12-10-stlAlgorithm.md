---
title: "[STL/C++] STL 알고리즘 컨테이너"
excerpt: "내가 보려고 만든 C++ STL 알고리즘 컨테이너 목록"
last_modified_at: 2020-12-10T21:51:00+09:00
categories: STL
toc: true
toc_sticky: true
---

# STL algorithm container

stl의 알고리즘 컨테이너는 크게 7가지로 나뉜다.
각 알고리즘들에 대한 설명은 [여기](https://github.com/GyeongHoKim/algorithm) 
알고리즘 관련된 것들을 정리하는 내 깃허브 리포지토리에 있다.

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

# 제거 알고리즘

| 알고리즘 | 설명(p는 구간 [b,e)의 반복자) |
| :---: | :---: |
| `p=remove(b,e,x)` | 구간의 순차열을 x원소가 남지 않도록 덮어쓰기로 이동한다. 알고리즘 수행 후 순차열은 [b,p)가 된다 |
| `p=remove(b,e,f)` | 구간의 순차열을 `f(*p)`가 참인 원소가 남지 않도록 덮어쓰기로 이동한다. 알고리즘 수행 후 순차열은 [b,p)가 된다 |
| `p=remove_copy(b,e,t,x)`| 구간의 순차열에서 *p==x가 아닌 원소만 순차열 [t,p)에 복사한다 |
| `p=remove_copy_if(b,e,t,f)` | 구간의 순차열에서 `f(*p)`가 참이 아닌 원소만 순차열에 복사한다 |
| `p=unique(b,e)` | 구간의 순차열을 인저반 중복원소가 남지 안게 덮어쓰기로 이동한다. 알고리즘 수행 후 p는 마지막 원소를 가리킨다 |
| `p=unique(b,e,f)` | 구간의 순차열을 `f(*p)`가 참인 인접한 중복 원소가 남지 않게 덮어쓰기로 이동한다. 알고리즘 수행 후 순차열은 [b,p)가 된다 |
| `p=unique_copy(b,e,t)` | 구간의 순차열에서 인접한 중복 원소가 아닌 원소를 두 번째 순차열에 복사한다 |
| `p=unique_copy(b,e,t,f)` | 구간의 순차열에서 `f(*p)`가 참인 인접한 중복 원소가 아닌 원소를 두 번째 순차열에 복사한다 |

# 변경 알고리즘

| 알고리즘 | 설명(p는 구간 [b,e)의 반복자) |
| :---: | :---: |
| `bool = next_permutation(b,e)` | 구간의 순차열을 사전순 다음 순열이 되게 한다. 마지막 순열이라면 bool은 false |
| `bool = next_permutation(b,e,f)` | 구간의 순차열을 사전순 다음 순열이 되게 한다. 비교에 f를 사용, 마지막 순열일 경우 bool은 false |
| `bool = prev_permutation(b,e)` | 구간의 순차열을 사전순 이전 순열이 되게 한다. 첫 순열일 경우 bool은 false |
| `bool = prev_permutation(b,e,f)` | r구간의 순차열을 사전순 이전 순열이 되게 한다. 비교에 f를 사용, 첫 순열일 경우 bool은 false |
| `p = partition(b,e,f)` | 구간의 순차열 중 `f(*p)`가 참인 원소는 [b,p)의 순차열에, 거짓인 원소는 [p,e)의 순차열로 분류한다 |
| `random_shuffle(b,e)` | 구간의 순차열을 랜덤으로 뒤섞는다 |
| `random_shuffle(b,e,f)` | 구간의 순차열을 f를 랜덤기로 하여 뒤섞는다 |
| `reverse(b,e)` | 구간의 순차열을 뒤집는다 |
| `p = reverse_copy(b,e,f)` | 구간의 순차열을 뒤집어 목적지 순차열 [t,p)에 복사 |
| `rotate(b,m,e)` | 구간의 순차열을 왼쪽으로 회전한다 |
| `p = rotate_copy(b,m,e,t)` | 구간의 순차열을 왼쪽으로 회전시켜 목적지 순차열에 복사 |
| `stable_partition(b,e,f)` | partition알고리즘과 같고 원소의 상대적 순서를 유지 |

# 정렬 알고리즘

| 알고리즘 | 설명(p는 구간 [b,e)의 반복자) |
| :---: | :---: |
| `p = partition(b,e,f)` | 구간의 순차열 중 `f(*p)`가 참인 원소는, [b,p)의 순차열에 거짓인 원소는 그 뒤로 분류 |
| `stable_partition(b,e,f)` | partition알고리즘과 같고 원소의 상대적인 순서를 유지한다 |
| `make_heap(b,e)` | 힙을 생성하고 구간의 순차열을 힙 구조로 변경한다 |
| `make_heap(b,e,f)` | 힙을 생성하고 구간의 순차열을 힙 구조로 변경한다, f는 조건자로 비교에 사용한다 |
| `push_heap(b,e,f)` | 힙에 원소를 추가한다, f는 조건자로 비교에 사용한다 |
| `push_heap(b,e)` | 힙에 원소를 추가한다 |
| `pop_heap(b,e)` | 힙에서 원소를 제거한다 |
| `pop_heap(b,e,f)` | 힙에서 원소를 제거한다 f는 조건자로 비교에 사용한다 |
| `sort_heap(b,e)` | 힙을 정렬한다. 구간의 순차열을 힙 구조를 이용하여 정렬한다 |
| `sort_heap(b,e,f)` | 힙을 정렬한다 구간의 순차열을 힙 구조를 이용하여 정렬한다. f는 조건자로 비교에 사용한다 |
| `nth_element(b,m,e)` | 구간의 원소 중 m-b개 만큼 선별된 원소를 [b,m)순차열에 놓이게 한다 |
| `nth_element(b,m,e,f)` | 구간의 원소 중 m-b개 만큼 선별된 원소를 [b,m)순차열에 놓이게 한다. f는 조건자로 비교에 사용한다 |
| `sort(b,e)` | 퀵 정렬을 기반으로 정렬 |
| `sort(b,e,f)` | 퀵 정렬을 기반으로 정렬, f를 비교에 사용 |
| `partial_sort(b,m,e)` | 힙 정렬을 기반으로 정렬 구간의 원소 중 m-b개 만큼의 상위 원소를 정렬 |
| `partial_sort(b,m,e,f)` | 힙 정렬을 기반으로 정렬 구간의 원소 중 m-b개 만큼의 상위 원소를 정렬, f는 조건자로 비교에 사용한다 |
| `partial_sort_copy(b,e,b2,e2)` | 힙 정렬을 기반으로 정렬, 구간의 원소 중 e2-b2개의 원소만 정렬하여 복사한다 |
| `partial_sort_copy(b,e,b2,e2,f)` | 힙 정렬을 기반으로 정렬, 구간의 원소 중 e2-b2개의 원소만 정렬하여 복사한다, f는 조건자로 비교에 사용한다 |

# 정렬된 범위 알고리즘

| 알고리즘 | 설명(p는 구간 [b,e)의 반복자) |
| :---: | :---: |
| `binary_search(b,e,x)` | 구간의 순차열에 x와 같은 원소가 있는가 |
| `binary_search(b,e,x,f)` | 구간의 순차열에 x와 같은 원소가 있는가, f를 비교에 사용 |
| `includes(b,e,b2,e2)` | 두 번째 구간의 모든 원소가 첫 번째 구간에도 있는가 |
| `p = lower_bound(b,e,x)` | p는구간의 순차열에서 x와같은 첫 원소의 반복자 |
| `p = lower_bound(b,e,x,f)` | p는구간의 순차열에서 x와같은 첫 원소의 반복자, f를 비교에 사용 |
| `p = upper_bound(b,e,x)` | p는 구간의 순차열에서 x보다 큰 원소의 반복자 |
| `p = upper_bound(b,e,x,f)` | p는 구간의 순차열에서 x보다 큰 원소의 반복자, f를 비교에 사용 |
| `pair(p1,p2) = equal_range(b,e,x)` | 구간 [p1,p2)의 순차열은 구간의 순차열에서 x와같은 원소의 구간이다. [lower_bound(),upper_bound())의 순차열과 같다 |
| `pair(p1,p2) = equal_range(b,e,x)` | 구간 [p1,p2)의 순차열은 구간의 순차열에서 x와같은 원소의 구간이다. [lower_bound(),upper_bound())의 순차열과 같다, f를 비교에 사용 |
| `p = merge(b,e,b2,e2,t)` | 첫 번째 구간의 순차열과 두 번째 구간의 순차열을 합쳐 [t,p)에 저장 |
| `p = merge(b,e,b2,e2,t,f)` | 첫 번째 구간의 순차열과 두 번째 구간의 순차열을 합쳐 [t,p)에 저장, f는 비교에 사용 |
| `inplace_merge(b,m,e)` | 정렬된 [b,m)순차열과 [m,e)순차열을 [b,e)순차열로 합친다 |
| `inplace_merge(b,m,e,f)` | 정렬된 [b,m)순차열과 [m,e)순차열을 [b,e)순차열로 합친다, f는 비교에 사용 |
| `p = set_union(b,e,b2,e2,t)` | 두 순차열의 합집합, [t,p)에 저장 |
| `p = set_union(b,e,b2,e2,t,f)` | 두 순차열의 합집합, [t,p)에 저장, f는 비교에 사용 |
| `p = set_intersection(b,e,b2,e2,t)` | 두 순차열의 교집합, [t,p)에 저장 |
| `p = set_intersection(b,e,b2,e2,t,f)` | 두 순차열의 교집합, [t,p)에 저장, f는 비교에 사용 |
| `p = set_difference(b,e,b2,e2,t)` | 두 순차열의 차집합, [t,p)에 저장 |
| `p = set_difference(b,e,b2,e2,t,f)` | 두 순차열의 차집합, [t,p)에 저장, f는 비교에 사용 |
| `p = set_symmetric_difference(b,e,b2,e2,t)` | 두 구간의 순차열을 정렬된 대칭 차집합으로 [t,p)에 저장 |
| `p = set_symmetric_difference(b,e,b2,e2,t,f)` | 두 구간의 순차열을 정렬된 대칭 차집합으로 [t,p)에 저장, f는 비교에 사용 |

# 수치 알고리즘

| 알고리즘 | 설명(p는 구간 [b,e)의 반복자) |
| :---: | :---: |
| `x2 = accumulate(b,e,x)` | x2는 x를 초깃값으로 시작한 구간[b,e) 순차열 원소의 합 |
| `x2 = accumulate(b,e,x,f)` | x2는 x를 초깃값으로 시작한 구간[b,e) 순차열 원소의 누적, f를 누적에 사용 |
| `x2 = inner_product(b,e,b2,x)` | x2는 x를 초깃값으로 시작한 구간 [b,e)와 구간 [b2,,b2+e-b)의 내적이다 |
| `x2 = inner_product(b,e,b2,x,f1,f2)` | x2는 x를 초깃값으로 시작한 구간 [b,e)와 구간 [b2,,b2+e-b)의 모든 원소끼리 f2연산 후 f1연산으로 총 연산한 결과다 |
| `p = adjaent_differene(b,e,t)` | 구간의 인접 원소와의 차를 두 번째 구간에 저장한다 |
| `p = adjacent_difference(b,e,t,f)` | 구간의 인접 원소와의 차를 두 번째 구간에 저장한다, f를 연산에 사용 |
| `p = partial_sum(b,e,t)` | 구간의 현재 원소까지의 합을 두 번째 구간에 저장한다 |
| `p = partial_sum(b,e,t,f)` | 구간의 현재 원소까지의 연산을 두 번째 순차열에 저장한다. f를 연산에 사용한다 |

끝