# 큐

선입선출 자료구조이다. C++ 덱 컨테이너의 하위호환으로 생각하면 되려나.

## 큐의 ADT

enqueue와 dequeue이 두 가지가 핵심. 자료구조를 처음 가르쳐주셨던 교수님의 성함을 이니셜로 따면 디큐였는데 디큐 디큐 할 때마다 그분이 떠오른다. 아무튼 c로 구현하자면

``` c
void QueueInit(Queue * pq);
int QIsEmpty(Queue * pq);
void Enqueue(Queue * pq, Data data);
Data Dequeue(Queue * pq);
Data QPeek(Queue * pq);
```

정도가 되겠다.

## 연결리스트 기반의 큐 구현

* 헤더 파일

