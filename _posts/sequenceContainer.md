# 내가 보려고 만든 C++ STL 정리 - Sequence Container

STL 컨테이너는 시퀀스 컨테이너와 연관 컨테이너로 나눌 수 있다. 표준 시퀀스 컨테이너는 vector, list, deque이다.

# vector

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
| `p = v.rend()` | p는 v의 역 순차열의 끝을 표식하는 반복자(const, non const version exist)