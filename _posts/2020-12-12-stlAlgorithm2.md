---
title: "[STL/C++] STL 컨테이너 어댑터"
excerpt: "stack, queue, priority_queue"
last_modified_at: 2020-12-12T17:10:00+09:00
categories: STL
toc: true
toc_sticky: true
---

# 컨테이너 어댑터

C++의 STL을 공부하면서 stack, queue같은 자료구조를 만들기가 굉장히 수월하겠다고 느꼈는데, 이미 몇 가지의 자료구조는 컨테이너 어댑터를 통해 바로 쓸 수가 있다.
컨테이너 어댑터는 다른 컨테이너의 인터페이스를 변경한 컨테이너이고, stack, queue, priotrity_queue 세 가지가 있다.

## stack 예제

``` c++
#include <iostream>
#include <stack>
using namespace std;

int main()
{
    stack<int> st;
  
    st.push(10);
    st.push(20);
    st.push(30);
  
    while(!st.empty())
    {
        cout << st.top() << endl;
        st.pop();
    }
  
    return 0;
}
```
결과는 30 20 10이 될 것이다.

## queue 예제

``` c++
#include <iostream>
#include <queue>
#include <list>
using namespace std;

int main()
{
    queue<int, list<int>> q;
    
    q.push(10);
    q.push(20);
    q.push(30);
    
    while(!q.empty())
    {
        cout << q.front() << endl;
        q.pop();
    }
    
    return 0;
}
```
결과는 10 20 30이 될 것이다

## priority_queue 예제

``` c++
#include <iostream>
#include <queue>
#include <deque>
using namespace std;

int main()
{
    priority_queue<int> pq1;
    pq1.push(40);
    pq1.push(20);
    pq1.push(30);
    pq1.push(50);
    pq1.push(10);
    
    cout << "priority_queue[less]:" << endl;
    while(!pq1.empty())
    {
        cout << pq1.top() << endl;
        pq1.pop();
    }
    cout << "========================" << endl;
    
    priority_queue<int, deque<int>, greater<int>> pq2;
    pq2.push(40);
    pq2.push(20);
    pq2.push(30);
    pq2.push(50);
    pq2.push(10);
    
    cout << "priority_queue[greater]:" << endl;
    while(!pq2.empty())
    {
        cout << pq2.top() << endl;
        pq2.pop();
    }
    
    return 0;
}
```
내부적으로 힙 알고리즘이 사용되어 있으므로 조건자에 따라 쌓이는 순서가 달라진다.

끝