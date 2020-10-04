---
title: "[자료구조론/C]자료구조론 2편 원형연결리스트"
excerpt: "원형연결리스트의 C 구현"
last_modified_at: 2020-09-30T18:50:00+09:00
categories: dataStructure
toc: true
toc_sticky: true
author_profile: false
---
# 원형연결리스트

가장 뒤의 노드가 가장 앞의 노드를 가리킨다.
![CLinkedList](../assets/images/dataStructure/CLinkedList.jpeg)

원형연결리스트의 경우, 마지막 노드를 가리키는 포인터 tail만 쓰는데, 어차피 tail이 가리키는 노드의 다음 노드가 head가 가리키는 노드이므로 head를 tail->next로 표현할 수 있기 때문이다.

## 원형연결 리스트의 헤더파일

``` c
#ifndef __C_LINKED_LIST_H__
#define __C_LINKED_LIST_H__

#define TRUE 1
#define FALSE 0

typedef int LData; //쓸 데이타에 따라 여기에 int대신 다른 자료형, 구조체를 넣자

typedef struct _node
{
	LData data;
	struct _node *next;
} Node;

typedef struct _CLL
{
	Node *tail;
	Node *cur;
	Node *before;
	int numOfData;
} CList;

typedef CList List;

void ListInit(List *plist);
void LInsert(List *plist, Data data);
void LInsertFront(List *plist Data data);

int LFirst(List *plist, Data *pdata);
int LNext(List *plist, Data *pdata);
Data LRemove(List *plist);
int LCount(List *plist);

#endif
```

operator는 더미노드 연결리스트의 경우와 같다.

## 리스트 초기화

다 널값을 주고 데이타의 개수는 0으로 잡으면 된다.

``` c
void ListInit(List *plist)
{
	plist->tail = NULL;
	plist->cur = NULL;
	plist->before = NULL;
	plist->numOfData = 0;
}
```

## 리스트 삽입

첫번째에 삽입할 것인가, 마지막에 삽입할 것인가에 따라 조금 다르다.
첫번째에 삽입한다고 하면 tail을 옮길 필요가 없으나, 마지막에 삽입한다면 tail이 가리키는 노드를 새로 삽입한 노드로 바꾸어야 할 것이다.

``` c
void LInsert(List *plist, LData data)
{
	//새로운 노드 생성
	Node *newNode = (Node*)malloc(sizeof(Node));
	newNode->data = data;
	
	if(plist->tail == NULL) {
		//첫 번째로 삽입되는 노드일 경우
		newNode->next = newNode; //자기 자신을 가리킴
		plist->tail = newNode; //마지막 노드임을 표시
	}
	else {
		//이미 다른 노드들이 존재하는 경우
		//무조건 새롭게 생성된 노드부터 조작
		newNode->next = plist->tail->next;
		//현재 마지막 노드의 다음을 새로운 노드로
		plist->tail->next = newNode;
		//마지막 노드임을 표시
		plist->tail = newNode;
	}
	
	(plist->numOfData)++;
}
```

마지막에 삽입하는 경우는 tail을 옮겨주어야 하나,

``` c
void LInsertFront(List *plist, LData data)
{
	Node *newNode  = (Node*)malloc(sizeof(Node));
	newNode->data = data;
	
	if(plist->tail == NULL) {
		newNode->next  = newNode;
		plist->tail = newNode;
	}
	else {
		//여기서부터 바뀐다
		//무조건 새로운 노드부터  조작
		newNode->next = plist->tail->next;
		plist->tail->next = newNode;
	}
	
	(plist->numOfData)++;
}
```

## 리스트 접근

접근은 for문 루프에서 대부분 이루어진다. 그러므로 첫번째 노드를 접근하는 함수, 특정 노드의 다음 노드에 접근하는 함수 두 가지가 있으면 순회가 가능하다.

``` c
int LFirst(List *plist, LData *pdata)
{
	if(plist->tail == NULL) return FALSE;
	
	//cur, before변수를 이용하여 접근한다
	plist->before = plist->tail;
	plist->cur = plist->tail->next;
	
	//해당하는 노드에 담긴 데이타를 반환한다
	*pdata = plist->cur->data;
	return TRUE;
}

int LNext(List *plist, LData *pdata)
{
	if(plist->cur->next == NULL) return FALSE;
	
	//before는 current가 되고 current는 그 다음 노드를 가리키게 된다
	plist->before = plist->cur;
	plist->cur = plist->cur->next;
	
	*pdata = plist->cur->data;
	return TRUE;
}
```

## 노드 삭제

노드의 연결을 끊고 cur을 한 칸 당긴다.

``` c
LData LRemove(List *plist)
{
	Node *rpos = (Node*)malloc(sizeof(Node));
	LData rdata = rpos->data;
	
	//삭제될 노드의 전 노드와 다음 노드를 이어준다
	plist->before->next = plist->cur->next;
	//current를 한 칸 당긴다.
	plist->cur = plist->before;
	
	//삭제할 노드의 메모리 헤제
	free(rpos);
	(plist->numOfData)--;
	return data;
}
```

끝