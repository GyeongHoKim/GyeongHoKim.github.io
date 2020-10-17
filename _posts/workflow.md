---
title: "[Git]프로젝트를 진행할 때 깃허브(Github)를 쓰는 법"
excerpt: "progit에 나와있는 대표적인 3가지 워크플로우"
last_modified_at: 2020-10-17T22:43:00+09:00
categories: git
tag: ['git','github']
toc: true
toc_sticky: true
author_profile: false
---

# Git과 워크플로우

git이 무엇인지, 그리고 어떻게 레포지토리를 만들 수 있는지 등은 구글에 검색하면 굉장히 많다.
그러나 git을 협업에, 특히 프로젝트 진행할 때 어떻게 이용하는지에 대한 설명은 잘 없는 것 같다.
~~사실 **분산 환경에서의 워크플로** 등 git ~~ workflow 키워드로 검색하면 잘나오는데, 과거의 나는 이걸 잘 몰랐다...~~

progit에 대표적인 3가지 방법이 소개되어있는데, 이 글에서는 이걸 다뤄보려고 한다.
* 중앙집중식 워크플로
* Integration-Manager 워크플로
* Dictator and Lieutenants 워크플로

요약해서 결론을 말하자면,
> 혼자 내지는 친한 친구 몇 명과 비공개 프로젝트 할 때: 중앙집중식 워크플로
> 다른 사람들과 함께 비공개 프로젝트 할 때           : Integration-Manager 워크플로
> 다른 사람들과 함께 공개 프로젝트 할 때             : Dictator and Lieutenants 워크플로
정도로 생각하면 되겠다.