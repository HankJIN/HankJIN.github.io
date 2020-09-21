---
title:  "리액트(React) 정리#1"
excerpt: "리액트에 대한 기초적인 개념을 정리"

categories:
  - Leaning
tags:
  - React
last_modified_at: 2020-9-21 T10:00:00-05:00
toc: true
toc_sticky: true
---


* * *

<span style="color:grey; font-size:12px;">리액트 설치후 간단하게 리액트를 정리한다.  기초를 정리하고 프로젝트를 48시간 진행해 해커톤처럼 하루종일 달라 붙어 진행 계획이다.
</span>

---


# 리액트 시작전에 정리해보기.


## 요소별 접근
- JSX : js안에 html 태그가 있는 리액트 컴포넌트 언어. 생성물은 React element를 생성함
- React : 컴포넌트”라고 불리는 작고 고립된 코드의 파편을 이용하여 복잡한 UI를 구성하도록 돕는 UI라이브러리.
- React DOM : React를 웹에 render 보조하는 모델    React Native : 모바일 앱에 Render를 보조  
	React Dom은 1개의 컴포넌트를 Render함.
	- Render : ??
	- 컴포넌트 : ??
	
<br>

---

<br>


## 파일 구조로 접근  
- Public : 정적 파일을 저장하는 디렉토리. 웹브라우저로 보이는 html 파일이나, img 등을 저장.  
	- public/ index.html	
	`<div id="root"></div>` : 리액트의 컴포넌트는 모두 id가 root인 element 안에 들어가도록 함.
- SRC : source를 저장하는 디렉토리로, 주로 작업하는 디렉토리
	- index.js

	```js
		<script>
		
		import React from 'react';
		import ReactDOM from 'react-dom';
		import './index.css';
		import App from './App';
		import * as serviceWorker from './serviceWorker';
		ReactDOM.render(
		<React.StrictMode>
		<App />
		</React.StrictMode>,
		document.getElementById('root')
		);
		serviceWorker.unregister();
		</script>
	```
<br>

---

<br>

## JSX 
### JS 표현식 포함하기  
JSX의 중괄호 `{}`에 모든 JS 표현식 삽입 가능.  


```jsx
//1. 기본
const name1 = 'jin';
const element1 = (
	<h1>
		hello, {name}
	</h1>
)

//2. 함수 호출 
function formatName(user) {
	return user.firstName + ' ' + user.lastName;
}
const user2 = {
	firstName: 'Harper',
	lastName: 'Perez'
};
const element2 = (
	<h1>
		Hello, {formatName(user)}!
	</h1>
);	

//3. 호출제어문
//컴파일이 끝나면, JSX 표현식이 정규 JavaScript 함수 호출이 되고 JavaScript 객체로 인식 => if나 loop에 사용가능
function getGreeting(user) {
	if (user) {
		return <h1>Hello, {formatName(user)}!</h1>;
	}
	return <h1>Hello, Stranger.</h1>;
}

//4.태그 속성(attribute)에도 JS 표현식 사용가능
const element = <img src={user.avatarUrl}></img>;
const element = <img src={user.avatarUrl} />;

ReactDOM.render(
element,
document.getElementById('root')
);
```


> 4 태그 속성에서 사용시, 중괄호 주변에 따옴표 사용 금지.  
> JSX는 html보다 JS에 가까워서  태그 속석에서 class대신 className를 사용합니다.  

-----


## 참고
> https://ko.reactjs.org/tutorial/tutorial.html  
> https://codinghub.tistory.com/    
> https://velog.io/@jeongmin_sung/  
> https://velog.io/@hidaehyunlee/