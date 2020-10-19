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
~~사실 **분산 환경에서의 워크플로** 등 git workflow 키워드로 검색하면 잘나오는데, 과거의 나는 이걸 잘 몰랐다...~~

progit에 대표적인 3가지 방법이 소개되어있는데, 이 글에서는 이걸 다뤄보려고 한다.
* 중앙집중식 워크플로
* Integration-Manager 워크플로
* Dictator and Lieutenants 워크플로

요약해서 결론을 말하자면,
> 혼자 내지는 친한 친구 몇 명과 비공개 프로젝트 할 때: 중앙집중식 워크플로
> 다른 사람들과 함께 비공개 프로젝트 할 때           : Integration-Manager 워크플로
> 다른 사람들과 함께 공개 프로젝트 할 때             : Dictator and Lieutenants 워크플로
정도로 생각하면 되겠다.

## 중앙집중식 워크플로

중앙집중형 버전 관리 시스템에서 사용하는 방식이다. 단, Git은 모든 자료를 모든 사람이 가지고 있기 때문에 서버가 아닌, 클라이언트 쪽에서 Merge한다.

가령 두 개발자 철수와 영희가 저장소를 공유한다고 할 때, 철수와 영희 둘다 로컬환경에서 저장소를 clone, 수정, 커밋할 것이다.
철수가 커밋한 내용을 먼저 서버에 push하면, 영희는 push하지 못한다. 그 전에 철수가 수정한 커밋을 fetch하고 merge해야한다.
토픽 브랜치를 만들어 수정하는 경우에도, 그 브랜치를 로컬의 master브랜치에 merge한 후 origin/master브랜치를 fetch하고 merge해야 push할 수 있다.
무조건 배포 브랜치인 master브랜치에 push한다고 보면 될 것 같다. 규모가 크지 않고 개발자의 수가 적은 프로젝트에 적합하다.

## Integration-Manager 워크플로

각 팀마다 브랜치를 하나씩 만들고 Integration-Manager가 그 브랜치를 Pull해서 Merge하는 워크플로이다.
