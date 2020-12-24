---
title: "[임베디드/프로젝트] 집 밖에서 가전제품을 제어해 보자 2탄"
excerpt: "ATmega128로 구현하는 IR 프로젝트 2탄"
last_modified_at: 2020-12-24T16:27:00+09:00
categories: embedded
tag: ['ATmega128','avr']
toc: true
toc_sticky: true
author_profile: false
---

# 적외선 신호 디코딩

저번 포스트에서 NEC 프로토콜을 설명한 적이 있다.

![NEC](https://mblogthumb-phinf.pstatic.net/MjAxNzA2MDlfMzkg/MDAxNDk2OTM3NDAzNjYw.WfPievnMENyLxQofvOE_Q0O8aZ500j2uHDPtKBnMQr0g.F4sOTf9Tdp6_oEtVHsacbzbwj1wJYIP8Jw-i-j-Uo8Eg.PNG.specialist0/Capture_08.PNG?type=w2)

NEC 프로토콜은 상승 에지 사이의 시간을 통해 0과 1을 구분하는 방법인 펄스 거리 인코딩, OOK 방식을 쓰는데, 시간간격은 다음과 같다.

| 유형 | . | 시간간격(ms) | 클록 수 |
| :---: | :---: | :---: | :---: |
| 리드 코드 | 일반 데이터 | 13.5 | 210.94 |
| 리드 코드 | 반복 데이터 | 11.25 | 175.78 |
| 논리 코드 | 논리 0 | 1.125 | 17.58 |
| 논리 코드 | 논리 1 | 2.25 | 35.16 |

적외선 신호를 디코딩하려면 적외선을 수신하는 모듈이 있어야 할 것이다. 다양한 수신모듈이 있겠지만 우리가 사용하는 37~8kHz 주파수 대역을 수신할 수 있는 모듈을 사용해야 하고
대부분의 모듈의 경우 평소에 high 상태를 유지하다가 신호를 받을 경우 low가 될 것이다.

저번 포스트의 맨 마지막 부분에도 적어뒀지만, 하강에지 때마다 외부 인터럽트가 걸리게 하고 외부 인터럽트 안에서 타이머/카운터를 초기화, 다음 외부 인터럽트걸릴 때 지금까지 센 타이머/카운터를 통해 논리 0과 논리 1을 구분하면 된다.