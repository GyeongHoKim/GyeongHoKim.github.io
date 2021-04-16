---
title: "[FE/Toy_project] Remaining Time 크롬 확장프로그램 제작기(1)"
excerpt: "a chrome extension for lazy people"
last_modified_at: 2021-04-16T23:58+09:00
categories: Front-end
tag: "Front-end"
toc: true
toc_sticky: true
author_profile: false
---

# Remaining Time

[깃헙 저장소](https://github.com/GyeongHoKim/RemainingTime)에 기능을 정리해 두었다.
며칠전, 책을 읽다가 어느 나이가 되면 자는 시간, 유튜브보는 시간, 등이 평균적으로 몇 시간정도 된다라는 문장을 읽었는데  
갑자기, 인타임이라는 영화가 생각났다.  

![인타임](https://upload.wikimedia.org/wikipedia/ko/0/01/%EC%9D%B8%ED%83%80%EC%9E%84_%ED%8F%AC%EC%8A%A4%ED%84%B0.jpg)

> **남은 인생을 초단위로 보여주면서 실시간으로 카운팅해주면 무서워서라도 시간낭비를 줄일 수 있지 않을까?  **

# 제작 계획

하고 싶은게 너무 많았다.  
내가 유튜브, 웹툰 사이트 등 비생산적인 활동에 할애한 시간도 표시해주고 싶고, 상단에 카운트다운을 작게 표시해주고 싶고, 알림도...  

이러다가 아무것도 못만들고 뇌내 망상에서 끝나겠다 싶어서 일단 만들어보기로 했다.  
이 앱의 목적은 남은 인생을 초단위로 카운팅해주는 것이므로  
크롬에서 **새 탭을 열었을 때 카운트다운, 오늘 해야 할 일을 보여주는 것**을 목표로 코딩을 시작했다.  

## 버전 1.0.0

![htmlcssjs](https://miro.medium.com/max/5120/1*l4xICbIIYlz1OTymWCoUTw.jpeg)

카운트다운, 할일 보여주기 정도는 프론트 단에서 바닐라 JS가지고도 충분히 할 수 있다.  
물론 리엑트로 프론트 만들고, 노드로 백엔드에서 연산하면 있어보이겠지만 닭잡는데 함마드릴 가지고 올 필요가 없다.  
그래서 일주일정도 조금씩 시간을 내서 HTML/CSS/JS로 구현을 마쳤다.

## 버전 1.0.1

> 그리드 레이아웃을 이용해볼까?

문득 이런 생각이 들었고 실제로 구현도 해봤는데 flex로 충분한 것 같아서 실제로 반영하지는 않았다.  
flex로 충분하다고 판단한 기준은 라인수이다.  
내가 아직 경험많은 개발자가 아니라서 그런지 굳이 grid로 구현할 이유를 못느꼈다. 다음에 grid의 필요성을 느끼면 그때 고치기로 한다.

## 버전 1.0.2

> 리엑트 연습해볼까?

리엑트 배웠는데 아무데도 안쓰면 섭섭해서 리엑트로 만들어보기로 했다.  
나는 리엑트를 [리엑트 공식문서](https://reactjs.org/docs/hello-world.html)로 배웠는데,  
공식문서에는 class component 중심으로 설명되있다. Hooks라는 탭이 따로 있길래 읽어봤는데 거긴 function으로도 state를 구현하더라?  
이번 기회에 Hooks를 써보자.  

나중에, 기능을 추가하고 페이지가 나뉘면 리엑트 라우터나 리덕스를 사용하면 편할 것 같으니 react로 구현하는게 확장성 부분에서 좋을 것 같다.  
(현재 개발 중)