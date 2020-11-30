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

컴파일러마다 조금씩 다르지만 대부분 (이전 capacity) + (이전 capacity / 2)를 만족한다.
가령, 원소가 6개 들어있는 vector<int> 컨테이너에 push_back(n)을 하면 capacity를 9로 재할당하여 원소들을 복사한다.
참고로 capacity()는 vector 컨테이너만 제공한다.
