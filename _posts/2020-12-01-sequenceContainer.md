---
title: "[STL/C++]시퀀스 컨테이너"
excerpt: "내가 보려고 만든 C++ STL 시퀀스 컨테이너"
last_modified_at: 2020-12-01T23:07:00+09:00
categories: STL
toc: true
toc_sticky: true
---

# 내가 보려고 만든 C++ STL 정리 - Sequence Container

STL 컨테이너는 시퀀스 컨테이너와 연관 컨테이너로 나눌 수 있다. 표준 시퀀스 컨테이너는 vector, list, deque이다.

# vector

## vector의 주요 인터페이스

| 템플릿 형식 |
| :---: |
| `template<typename T, typename Allocator = allocator<T>> class vector` |

T는 vector 컨테이너 원소의 형식이고, 할당자는 디폴트가 `allocator<T>`이다. 디폴트 외의 할당자를 쓰는 법은 나도 모른다.

| 생성자 |
| :---: | :---: |
| `vector v` | v는 빈 컨테이너 |
| `vector v(n)` | v는 기본 값으로 초기화된 n개의 원소를 갖는다 |
| `vector v(n, x)` | v는 x값으로 초기화된 n개의 원소를 갖는다 |
| `vector v(v2)` | 복사 생성자 |
| `vector v(b, e)` | v는 반복자 구간 [b, e)로 초기화된 원소를 갖는다 |

| 멤버 함수 |
| :---: | :---: |
| `v.assign(n, x)` | v에 x값으로 n개의 원소를 할당 |
| `v.assign(b, e)` | v를 반복자 구간 [b, e)로 할당 |
| `v.at(i)` | v의 i번째 원소를 참조한다(const, non const version exist & include range check) |
| `v.back()` | v의 마지막 원소 참조(const, non const version exist) |
| `p = v.begin()` | p는 v의 첫 원소를 가리키는 반복자(const, non const version exist) |
| `x = v.capacity()` | x는 v에 할당된 공간의 크기 |
| `v.clear()` | v의 모든 원소를 제거한다 |
| `v.empty()` | v가 비었는지 조사 |
| `p = v.end()` | p는 v의 끝을 가리키는 반복자(const, non const version exist) |
| `q = v.erase(p)` | p가 가리키는 원소를 제거한다. q는 다음 원소를 가리킨다 |
| `q = v.erase(b,e)` | 반복자 구간 [b, e)의 모든 원소를 제거한다. q는 다음 원소를 가리킨다 |
| `v.front()` | v의 첫 번째 원소를 참조한다(const, non const version exist) |
| `q = v.insert(p, x)` | p가 가리키는 위치에 x를 삽입, q는 삽입한 원소를 가리키는 반복자 |
| `v.insert(p, n, x)` | p가 가리키는 위치에 n개의 x값을 삽입 |
| `v.insert(p, b, e)` | p가 가리키는 위치에 반복자 구간 [b, e)의 원소를 삽입한다.
| `x = v.max_size()` | x는 v가 담을 수 있는 최대 원소의 개수(메모리 크기) |
| `v.pop_back()` | v의 마지막 원소 제거 |
| `v.push_back(x)` | v의 끝에 x를 추가 |
| `p = v.rbegin()` | p는 v의 역 순차열의 첫 원소를 가리키는 반복자(const, non const version exist) |
| `p = v.rend()` | p는 v의 역 순차열의 끝을 표식하는 반복자(const, non const version exist) |
| `v.reserve(n)` | n개의 원소를 저장할 공간을 예약한다 |
| `v.resize(n)` | v의 크기를 n으로 변경하고 확장되는 공간의 값을 기본값으로 초기화 |
| `v.resize(n, x)` | v의 크기를 n으로 변경하고 확장되는 공간의 값을 x값으로 초기화 |
| `v.size()` | v 원소의 개수 |
| `v.swap(v2)` | v와 v2를 swap한다 |

| 연산자 |
| :---: | :---: |
| `v1 == v2` | v1과 v2의 모든 원소가 같은가(bool) |
| `v1 != v2` | v1과 v2의 모든 원소 중 하나라도 다른 원소가 있는가(bool) |
| `v1 < v2` | 문자열 비교처럼 v2가 v1보다 큰가(bool) |
| `v1 <= v2`| 생략 |
| `v1 > v2` | 생략 |
| `v1 >= v2` | 생략 |
| `v[i]` | v의 i번째 원소를 참조(const, 비 const 버전이 있고 범위점검이 없음 |

멤버 형식의 종류

* alloator_type
* const_iterator
* const_pointer
* const_reference
* const_reverse_iterator
* difference_type
* iterator
* pointer
* reference
* reverse_iterator
* size_type
* value_type

## 메모리 확장 정책

vector는 원소가 하나의 메모리 블록에 연속적으로 저장된다.
컴파일러마다 조금씩 다르지만 대부분 (이전 capacity) + (이전 capacity / 2)를 만족한다.
가령, 원소가 6개 들어있는 vector<int> 컨테이너에 push_back(n)을 하면 capacity를 9로 재할당하여 원소들을 복사한다.
참고로 capacity()는 vector 컨테이너만 제공한다.

# deque

## deque 컨테이너의 주요 인터페이스

| 템플릿 형식 |
| :---: |
| `template<typename T, typename Allocator = allocator<T>> class deque` |

| 생성자 |
| :---: | :---: |
| `deque dq` | dq는 빈 컨테이너 |
| `deque dq(n)` | dq는 기본 값으로 초기화된 n개의 원소를 갖는다 |
| `deque dq(n, x)` | dq는 x값으로 초기화된 n개의 원소를 갖는다 |
| `deque dq(dq2)` | 복사 생성자 |
| `deque dq(b, e)` | dq는 반복자 구간 [b, e)로 초기화된 원소를 갖는다 |

| 멤버 함수 |
| :---: | :---: |
| `dq.assign(n, x)` | dq에 x값으로 n개의 원소를 할당 |
| `dq.assign(b, e)` | dq를 반복자 구간 [b, e)로 할당 |
| `dq.at(i)` | dq의 i번째 원소를 참조한다(const, non const version exist & include range check) |
| `dq.back()` | dq의 마지막 원소 참조(const, non const version exist) |
| `p = dq.begin()` | p는 dq의 첫 원소를 가리키는 반복자(const, non const version exist) |
| `dq.clear()` | dq의 모든 원소를 제거한다 |
| `dq.empty()` | dq가 비었는지 조사 |
| `p = dq.end()` | p는 dq의 끝을 가리키는 반복자(const, non const version exist) |
| `q = dq.erase(p)` | p가 가리키는 원소를 제거한다. q는 다음 원소를 가리킨다 |
| `q = dq.erase(b,e)` | 반복자 구간 [b, e)의 모든 원소를 제거한다. q는 다음 원소를 가리킨다 |
| `dq.front()` | v의 첫 번째 원소를 참조한다(const, non const version exist) |
| `q = dq.insert(p, x)` | p가 가리키는 위치에 x를 삽입, q는 삽입한 원소를 가리키는 반복자 |
| `dq.insert(p, n, x)` | p가 가리키는 위치에 n개의 x값을 삽입 |
| `dq.insert(p, b, e)` | p가 가리키는 위치에 반복자 구간 [b, e)의 원소를 삽입한다.
| `x = dq.max_size()` | x는 dq가 담을 수 있는 최대 원소의 개수(메모리 크기) |
| `dq.pop_back()` | dq의 마지막 원소 제거 |
| `dq.pop_front()` | dq의 첫 원소 제거 |
| `dq.push_back(x)` | dq의 끝에 x를 추가 |
| `dq.push_front(x)` | dq의 앞쪽에 x를 가가
| `p = dq.rbegin()` | p는 dq의 역 순차열의 첫 원소를 가리키는 반복자(const, non const version exist) |
| `p = dq.rend()` | p는 dq의 역 순차열의 끝을 표식하는 반복자(const, non const version exist) |
| `dq.resize(n)` | dq의 크기를 n으로 변경하고 확장되는 공간의 값을 기본값으로 초기화 |
| `dq.resize(n, x)` | dq의 크기를 n으로 변경하고 확장되는 공간의 값을 x값으로 초기화 |
| `dq.size()` | dq 원소의 개수 |
| `dq.swap(dq2)` | dq와 dq2를 swap한다 |

| 연산자 |
| :---: | :---: |
| `dq1 == dq2` | dq1과 dq2의 모든 원소가 같은가(bool) |
| `dq1 != dq2` | dq1과 dq2의 모든 원소 중 하나라도 다른 원소가 있는가(bool) |
| `dq1 < dq2` | 문자열 비교처럼 dq2가 dq1보다 큰가(bool) |
| `dq1 <= dq2`| 생략 |
| `dq1 > dq2` | 생략 |
| `dq1 >= dq2` | 생략 |
| `dq[i]` | dq의 i번째 원소를 참조(const, 비 const 버전이 있고 범위점검이 없음 |

멤버 형식의 종류

* alloator_type
* const_iterator
* const_pointer
* const_reference
* const_reverse_iterator
* difference_type
* iterator
* pointer
* reference
* reverse_iterator
* size_type
* value_type

## 메모리 확장 정책

vector는 앞이 막힌구조인 반면, deque은 양 쪽 다 뚫려있다.
vector는 capacity보다 더 크게 들어올 경우 메모리를 재할당하고 원소를 전부 옮기는 반면, deque은 메모리 블록을 하나 할당해서 최종적으로 데이터를 여러 메모리 블록에 나뉘어서 저장한다. 굳이 원래 원소들을 전부 삭제할 필요가 없다.
둘 다,  insert나 erease시에 그 위치에서부터의 모든 원소들을 밀어내야 한다는 비효율적인 면이 있으나 deque은 앞으로도 밀어낼 수 있기 때문에 vector보다는 덜 비효율적이라고 할 수 있다.

# list 컨테이너

## list 컨테이너 주요 인터페이스

| 템플릿 형식 |
| :---: |
| `template<typename T, typename Allocator = allocator<T>> class list` |

| 생성자 |
| :---: | :---: |
| `list lt` | lt는 빈 컨테이너 |
| `list lt(n)` | lt는 기본 값으로 초기화된 n개의 원소를 갖는다 |
| `list lt(n, x)` | lt는 x값으로 초기화된 n개의 원소를 갖는다 |
| `list lt(lt2)` | 복사 생성자 |
| `list lt(b, e)` | lt는 반복자 구간 [b, e)로 초기화된 원소를 갖는다 |

| 멤버 함수 |
| :---: | :---: |
| `lt.assign(n, x)` | lt에 x값으로 n개의 원소를 할당 |
| `lt.assign(b, e)` | lt를 반복자 구간 [b, e)로 할당 |
| `lt.back()` | lt의 마지막 원소 참조(const, non const version exist) |
| `p = lt.begin()` | p는 lt의 첫 원소를 가리키는 반복자(const, non const version exist) |
| `lt.clear()` | lt의 모든 원소를 제거한다 |
| `lt.empty()` | lt가 비었는지 조사 |
| `p = lt.end()` | p는 lt의 끝을 가리키는 반복자(const, non const version exist) |
| `q = lt.erase(p)` | p가 가리키는 원소를 제거한다. q는 다음 원소를 가리킨다 |
| `q = lt.erase(b,e)` | 반복자 구간 [b, e)의 모든 원소를 제거한다. q는 다음 원소를 가리킨다 |
| `lt.front()` | lt의 첫 번째 원소를 참조한다(const, non const version exist) |
| `q = lt.insert(p, x)` | p가 가리키는 위치에 x를 삽입, q는 삽입한 원소를 가리키는 반복자 |
| `lt.insert(p, n, x)` | p가 가리키는 위치에 n개의 x값을 삽입 |
| `lt.insert(p, b, e)` | p가 가리키는 위치에 반복자 구간 [b, e)의 원소를 삽입한다. |
| `x = lt.max_size()` | x는 lt가 담을 수 있는 최대 원소의 개수(메모리 크기) |
| `lt.pop_back()` | lt의 마지막 원소 제거 |
| `lt.pop_front()` | lt의 첫 원소 제거 |
| `lt.push_back(x)` | lt의 끝에 x를 추가 |
| `lt.push_front(x)` | lt의 앞쪽에 x를 추가 |
| `p = lt.rbegin()` | p는 lt의 역 순차열의 첫 원소를 가리키는 반복자(const, non const version exist) |
| `p = lt.rend()` | p는 lt의 역 순차열의 끝을 표식하는 반복자(const, non const version exist) |
| `lt.resize(n)` | lt의 크기를 n으로 변경하고 확장되는 공간의 값을 기본값으로 초기화 |
| `lt.resize(n, x)` | lt의 크기를 n으로 변경하고 확장되는 공간의 값을 x값으로 초기화 |
| `lt.size()` | lt 원소의 개수 |
| `lt.sort()` | lt의 모든 원소를 오름차 순으로 정렬 |
| `lt.sort(pred)` | lt의 모든 원소를 pred(조건자)를 기준으로 정렬, pred(조건자)는 이항 조건자 |
| `lt.splice(p, lt2)` | p가 가리키는 위치에 lt2의 모든 원소를 잘라 붙인다. |
| `lt.splice(p, lt2, q` | p가 가리키는 위치에 lt2의 q가 가리키는 원소를 잘라 붙인다. |
| `lt.splice(p, lt2, b, e)` | p가 가리키는 위치에 lt2의 순차열 [b, e)을 잘라 붙인다. |
| `lt.swap(lt2)` | lt와 lt2를 swap한다 |
| `lt.unique()` | 인접한 원소의 값이 같다면 유일한 원소의 순차열로 만든다. |
| `lt.unique(pred)` | 인접한 원소가 pred(이항 조건자)의 기준에 맞다면 유일한 원소의 순차열로 만든다. |

| 연산자 |
| :---: | :---: |
| `lt1 == lt2` | lt1과 lt2의 모든 원소가 같은가(bool) |
| `lt1 != lt2` | dq1과 lt2의 모든 원소 중 하나라도 다른 원소가 있는가(bool) |
| `lt1 < lt2` | 문자열 비교처럼 lt2가 lt1보다 큰가(bool) |
| `lt1 <= lt2`| 생략 |
| `lt1 > lt2` | 생략 |
| `lt1 >= lt2` | 생략 |

멤버 형식의 종류

* alloator_type
* const_iterator
* const_pointer
* const_reference
* const_reverse_iterator
* difference_type
* iterator
* pointer
* reference
* reverse_iterator
* size_type
* value_type