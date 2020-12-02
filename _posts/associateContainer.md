---
title: "[STL/C++] 연관 컨테이너"
excerpt: "내가 보려고 만든 C++ STL 연관 컨테이너"
last_modified_at: 2020-12-02T20:55:00+09:00
categories: STL
toc: true
toc_sticky: true
---

# 내가 보려고 만든 C++ STL 정리 - associate Container

연관 컨테이너는 set, multiset, map, multimap이 있고 모두 같은 인터페이스를 제공한다.

## set의 주요 인터페이스

| 템플릿 형식 |
| :---: | :---: |
| `template<typename Key, typename Pred=less<Key>, typename Allocator=allocator<Key>> class set` | Key는 set 컨테이너 원소의 형식이며, Pred는 set의 정렬 기준인 조건자이다. 기본 조건자는 less이다 |

| 생성자 |
| :---: | :---: |
| `set s;` | s는 빈 컨테이너다 |
| `set s(pred)` | s는 빈 컨테이너 |
| `set s(s2)` | s는 s2 컨테이너의 복사본이다(복사 생성자 호출) |
| `set s(b, e)` | s는 반복자 구간 [b, e)로 초기화된 원소를 갖는다 |
| `set s(b, e, pred)` | s는 반복자 구간 [b, e)로 초기화된 원소를 갖는다. 정렬 기준은 pred조건자를 사용한다 |

| 멤버 함수 |
| :---: | :---: |
| `p = s.begin()` | p는 s의 첫 원소를 가리키는 반복자다(const, 비 const 버전이 있음) |
| `s.clear()` | s의 모든 원소를 제거한다 |
| `n = s.count(k)` | 원소 k의 개수를 반환한다 |
| `s.empty()` | s가 비었는지 조사한다 |
| `p = s.end()` | p는 s의 끝을 표식하는 반복자다(const, 비 const 버전이 있음) |
| `pr = s.equal_range(k)` | pr은 k 원소의 반복자 구간인 pair객체다(const, 비 const 버전이 있음) |
| `q = s.erase(p)` | p가 가리키는 원소를 제거한다. q는 다음 원소를 가리킨다 |
| `n = s.erase(k)` | k 원소를 모두 제거한다. n은 제거한 개수이다 |
| `p = s.find(k)` | p는 k 원소의 위치를 가리키는 반복자다(const, 비 const 버전이 있음) |
| `pr = s.insert(k)` | s 컨테이너에 k를 삽입한다. pr은 삽입한 원소를 가리키는 반복자와 성공 여부의 bool값인 pair객체다 |
| `q = s.insert(p, k)` | p가 가리키는 위치부터 빠르게 k를 삽입한다. q는삽입한 원소를 가리키는 반복자이다 |
| `s.insert(b, e)` | 반복자 구간 [b, e)의 원소를 삽입한다 |
| `pred = s.key_comp()` | pred는 s의 key정렬 기준인 조건자다(key_compare 타입) |
| `p = s.lower_bound(k)` | p는 k의 시작 구간을 가리키는 반복자다(const, 비 const 버전이 있음) |
| `n = s.max_size()` | n는 s가 담을 수 있는 최대 원소의 개수다(메모리의 크기) |
| `p = s.rbegin()` | p는 s의 역 순차열의 첫 원소를 가리키는 반복자다(const, 비 const 버전이 있음) |
| `p = s.rend()` | p는 s의 역 순차열의 끝 원소를 표식하는 반복자다(const, 비 const 버전이 있음) |
| `s.size()` | s 원소의 개수다 |
| `v.swap(v2)` | v와 v2를 swap한다 |
| `p = s.upper_bound(k)` | p는 k의  끝 구간을 가리키는 반복자다(const, 비 const 버전이 있음) |
| `pred = s.value_comp()` | pred는 s의 value 정렬 기준인 조건자다(const, 비 const 버전이 있음) |

| 연산자 |
| :---: | :---: |
| `s1 == s2` | s1과 s2의 모든 원소가 같은가(bool 형식) |
| `s1 != s2` | s1과 s2의 모든 원소 중 하나라도 다른 원소가 있는가?(bool 형식) |
| `s1 < s2` | 생략 |
| `s1 <= s2` | 생략 |
| `s1 > s2` | 생략 |
| `s1 >= s2` | 생략 |

멤버 형식

* allocator_type
* const_iterator
* const_pointer
* const_reference
* const_reverse_iterator
* difference_type
* iterator
* key_compare
* key_type
* pointer
* reference
* reverse_iterator
* size_type
* value_compare
* value_type
