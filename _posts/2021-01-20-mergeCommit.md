---
title: "[Git] merge commit을 내지 않고 병합하는 법"
excerpt: "다른 사람이 push한 내용을 merge하지 않은 체 내 작업을 push해버렸을 때"
last_modified_at: 2021-01-20T23:33:00+09:00
categories: git
tag: ['git','github']
toc: true
toc_sticky: true
author_profile: false
---

# 알고리즘 스터디 리포지토리에 push하다가 생긴 오류

오늘도 어김없이 알고리즘 문제를 풀고 원격 저장소에 내 소스코드를 업로드하는 나, git을 쓰던 도중 다음과 같은 에러를 만나게 된다.

![pushError](/assets/images/git/pushError.png)

그러자, 옆자리에 앉아 코딩하고 있던 후임왈  
"하핫...제가 푸쉬했슴다"

그렇다. 후임이 자신의 코드를 푸쉬했지만 나는 그걸 병합하지 않은 체로 내 작업물을 푸쉬해버린 것이다.

`$git pull origin main` 뒤늦게 병합했지만 깃헙에 찍힌 불편한 커밋 메시지.

![mergeCommit](/assets/images/git/mergeCommit.png)

아...위키에 적어놓은 커밋 메시지 컨벤션이랑 안맞잖아, 불편해

# 이럴 때는 이렇게

![rebase](/assets/images/git/rebase.png)

로컬 저장소의 브랜치가 원격 저장소의 브랜치의 최신 커밋으로 리베이스되어 병합 커밋이 남지 않는다. 와우!
자세한 것은 구글에 git rebase 검색!

끝