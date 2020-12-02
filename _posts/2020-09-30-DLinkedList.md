---
title: "[자료구조론/C]자료구조론 1편 더미연결리스트"
excerpt: "더미연결리스트의 C 구현"
last_modified_at: 2020-09-30T18:50:00+09:00
categories: dataStructure
toc: true
toc_sticky: true
---

# 연결리스트

가독성 너무 떨어진다... css좀 손보자(끼에엑)

## 연결리스트 ADT

![LinkedList](/assets/images/DLinkedList/LinkedList.jpeg)

	자료와 주소값을 저장할 수 있는 노드를 단위로 하여 만든 연속적인 배열

**연결리스트 ADT의 Operations**  

- 초기화(연결리스트 초기화)  
	- void ListInit();  

- 삽입(연결리스트 노드 일반/정렬 삽입)  
	- int FInset();  
	- int SInsert();  

- 삭제(연결리스트 노드 삭제)  
	- LData LRemove();  
  
## 더미 노드 연결리스트의 c 구현

* DLinkedList.h  


``` cpp
#ifndef __D_LINKED_LIST_H__
#define __D_LINKED_LIST_H__

#define TRUE 1
#define FALSE 0

typedef int LData // 나중에 리스트에 담길 데이타의 형을 변환하기 편함

typedef struct _node
{
	LData data;
	struct _node* next;
} Node;

typedef struct _linkedList
{
	Node* head;
	Node* cur;
	Node* before;
	int numOfData;
	int (*comp)(LData d1, LData d2);
} LinkedList;

typedef LinkedList List;

void ListInit(List* plist);
void LInsert(List* plist, LData data);

int LFirst(List* plist, LData* pdata);
int LNext(List* plist, LData* pdata);

LData LRemove(List* plist);
int LCount(List* plist);

void SetSortRule(List* plist, int (*comp)(LData d1, LData d2));

#endif
```

---

* void ListInit(List* plist);

``` c
void ListInit(List* plist)
{
    plist->head = (List*)malloc(sizeof(Node));
    plist->head->next = NULL;
    plist->comp = NULL;
    plist->numOfData = 0;
}
```

![DListInit](/assets/images/DLinkedList/Init.jpeg)  
    1. plist라는 변수는 앞서 정의한 List형의 변수 주소를 가지고 있다. List 구조체에서 head멤버는 첫번째 노드를 가리키는 포인터 변수이다. malloc함수로 Node만큼의 크기를 동적할당한 후 그 첫주소를 (List*)형으로 타입캐스팅한 값을 대입하게 된다.
	2. 이렇게 만든 더미 노드에 다음 노드 주소값을 NULL로 한다.
	3. Insert시 comp가 가리키는 함수를 사용하여 정렬삽입하게 되는데 일단 NULL하고 이후 SetSortRule함수로 정한다.
	4. 데이타의 수를 0으로 초기화한다.

---
* void LInsert(List* plist, LData data);

``` c
void LInsert(List* plist LData data)
{
    if(plist->comp == NULL) FInsert(plist, data);
    else SInsert(plist, data);
}
``` 
	1. 정렬기준 함수가 없다면 머리에,
	2. 있다면 정렬기준대로 삽입한다.

``` c
void FInsert(List* plist, LData data)
{
    List* newNode = (List*)malloc(sizeof(Node));
    newNode->data = data;

    newNode->next = plist->head->next;
    plist->head->next = newNode;

    (plist->numOfData)++;
}
```
	1. 새로운 노드를 만든다
	2. 새로운 노드에 데이타를 넣는다
	3. 새로운 노드의 다음노드 주소를 초기화시킨다
	4. 새로운 노드를 가장 앞에 연결한다

여기서 눈여겨 보아야 할 점은, newNode의 값들을 먼저 초기화 시키고 나중에 원래 연결리스트의 값들을 조정한다는 것이다.

만약 newNode가 아니라 plist->head->next = newNode 코드가 앞섰다면,  
![ifNotNewFirst](/assets/images/DLinkedList/ifNotNewFirst.jpeg)  
원래의 정보를 잃게 된다.  
꼭 연결리스트가 아니더라도 삽입을 진행할 때는 새로운 노드의 값을 먼저 초기화해야 한다.

``` c
void SetSortRule(List* plist, int (*comp)(LData d1, LData d2))
{
    plist->comp = comp;
}

void SInsert(List* plist, LData data)
{
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    Node* pred = plist->head;

    while(pred->next != NULL && plist->comp(data, pred->next->data) != 0)
        pred = pred->next;

    newNode->next = pred->next;
    pred->next = newNode;

    (plist->numOfData)++;
}
```
	1. 새로운 노드 생성
	2. 새로운 노드의 데이타 초기화
	3. 현재위치를 표현할 포인터변수 할당 및 초기화
	4. 조건에 맞는(정렬규칙) 위치가 될 때까지 현재위치 이동
	5. 현재위치 다음 노드의 주소값을 새로운 노드의 다음 노드 주소값으로 초기화
	6. 새로운 노드 삽입

정렬삽입도 크게 다르지 않다. 차이점을 말하자면 Finsert()함수는 현재위치에 해당하는 pred변수에 더미노드주소를 넣었고, SInsert()함수는 현재위치를 comp함수의 결과값으로 한 것 뿐이다.

* int LFirst(List* plist, LData* pdata)

``` c
int LFirst(List* plist, LData* pdata)
{
    if(plist->head->next == NULL) return FALSE;

    plist->before = plist->head;
    plist->cur = plist->head->next;

    *pdata = plist->cur->data;
    return TRUE;
}
```
	1. 비어있는지 체크
	2. 이전 노드는 더미노드
	3. 현재 노드는 더미노드의 바로 다음 노드
	4. 현재 노드의 데이터 저장
	5. 성공 반환

접근함수는 순회와 삭제에 자주 쓰인다. 현재노드를 삭제한다고 하면, cur이 가리키는 주소는 의미없는 주소가 되므로 before을 사용하여 cur의 위치를 한칸 앞으로 옮긴다던가 하는 추가조정을 한다.

* int LNext(List* plist, LData* pdata)

``` c
int LNext(List* plist, LData* pdata)
{
    if(plist->cur->next == NULL) return FALSE;
    
    plist->before = plist->cur;
    plist->cur = plist->cur->next;

    *pdata = plist->cur->data;
    return TRUE;
}
```
	1. 비어있는지 체크
	2. before 한 칸 뒤로
	3. cur 한 칸 뒤로
	4. 데이터 저장
	5. 성공 반환

---
* LData LRemove(List* plist);

``` c
LData LRemove(List* plist)
{
    Node* rpos = plist->cur;
    LData rdata = rpos->data;

    plist->before->next = plist->cur->next;
    plist->cur = plist->before;

    free(rpos);
    (plist->numOfData)—;
    return rdata;
}
```
	1. 지울 노드의 주소 따로 저장
	2. 지울 노드의 데이터 따로 저장
	3. 지울 노드 연결끊기 및 다음 노드 연결
	4. 현재 주소 앞으로 한 칸 당기기
	5. 지울 노드 해제
	6. 데이터 수 하나 줄이기
	7. 지운 노드의 데이터 반환

> LRemove()를 실행하면 cur의 주소와 before의 주소가 같아진다.  
> plist->cur = plist->before  

>> 그래도 상관없다. 어차피 노드의 삭제는 노드의 순회과정을 전제로 하므로 LRemove()실행이후 반드시 LFirst() 또는 LNext()함수를 실행하게 되어있다. 전자의 경우 cur과 before모두 맨 앞의 노드, 더미 노드로 초기화 될 것이고 후자의 경우 cur값이 cur->next값으로 초기화된다. 따라서 cur과 before이 같은 값을 갖는 상태에서 리스트를 조작하는 경우는 발생하지 않는다.