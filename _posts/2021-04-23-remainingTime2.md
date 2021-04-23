---
title: "[FE/Toy_project] Remaining Time 크롬 확장프로그램 제작기(2)"
excerpt: "a chrome extension for lazy people"
last_modified_at: 2021-04-23T23:58+09:00
categories: Front-end
tag: "Front-end"
toc: true
toc_sticky: true
author_profile: false
---

# 리엑트로 다시 만들자

앞으로 유튜브를 오래볼 때 알림을 주는 기능, 검색기능이랑 로그인 넣고  

리엑트 네이티브써서 하이브리드 앱으로 확장도 해야 한다. 결과적으로 리엑트 바탕으로 짜는게 앞으로 더 유리할 것 같다.

## Hooks와 Class 컴포넌트

Hooks의 장점은 함수기반 프로그래밍을 하면서 state를 사용할 수 있다는 것이다.  
몇 년전까지 함수기반 컴포넌트를 잘 사용하지 않았던 이유는 class 컴포넌트에서 쓰는 state 기능을 사용하지 못했기 때문이다.  
Hooks를 이용하면 함수기반으로 컴포넌트를 만들면서도 state를 이용할 수 있다.  
사용자 기반 Hook을 만들 수 있고, useEffect나 useRef 등을 쓰면 class 기반 컴포넌트에서 사용하는 didmount 등의 함수들도 똑같이 구현할 수 있다. 내가 느낀건 여기까지.  
하지만 리엑트 공식문서 도큐먼트에 class 컴포넌트 기반으로 설명이 나와있는 만큼, class 컴포넌트도 중요하다는 거겠지? 그래서 둘 다 썼다.  

``` js
function App() {
	const app = useRef();
	useEffect(() => {
		let imgSelected = imgUrl[Math.floor(Math.random() * 3) + 0];
		app.current.style.backgroundImage = 'url(' + imgSelected + ')';
	});
	return (
		<div className="App" ref={app}>
			<LeftTime />
		</div>
	);
}
```
함수형으로 짤 수도 있고?

``` js
class LeftTime extends React.Component {
	constructor(props) {
		super(props);
		this.state = {
			date: new Date(),
			age: 0,
			seconds: MAX_LIFE_TIME,
			ageExist: false
		};
		this.handleAge = this.handleAge.bind(this);
		this.handleSeconds = this.handleSeconds.bind(this);
		this.timer = this.timer.bind(this);
	}
//중략
```
class 컴포넌트식으로 짜는게 아직 더 편하기는 하다.


## 스타일링

버전 1.0.0의 Remaining Time은 그냥 CSS를 사용했다.  
React로 다시 만들다 보니, CSS 모듈, SASS, Styled Component 등의 방법이 있다는 것을 알게 되었고
레거시 코드를 그대로 재사용하는 것이 편한 곳에는 css를 그대로 쓰고, 조건부로 css가 작동해야 하는 부분에는 SCSS를 이용한 Styled Component를 사용했다.  

``` js
const StyledParagraph = styled.p`
	${props => props.srOnly === true ?
		css`
			font-size: 40px;
			color: #899098;
		`
		:
		css`
			border: 0 !important;
			clip: rect(1px, 1px, 1px, 1px) !important;
			-webkit-clip-path: inset(50%) !important;
			clip-path: inset(50%) !important;
			height: 1px !important;
			margin: -1px !important;
			overflow: hidden !important;
			padding: 0 !important;
			position: absolute !important;
			width: 1px !important;
			white-space: nowrap !important;
		`
	}
`;
```

이런 식으로.

# 앞으로 해야할 일

백엔드를 배워서 로그인기능을 추가하고 싶다. 또, 구글 검색기능이랑 유튜브 오래보면 알림가는 기능도.

끝