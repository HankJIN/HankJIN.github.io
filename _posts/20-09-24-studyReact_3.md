---
title:  "리액트(React) 정리#3"
excerpt: "리액트에 대한 기초적인 개념을 정리#3"

categories:
  - Leaning
tags:
  - React
last_modified_at: 2020-9-24 T10:00:00-05:00
toc: true
toc_sticky: true
---

<span style="color:grey; font-size:12px;">
리액트의 이벤트와 컴포넌트 합성 및 재사용에 대해 학습한다.
</span>

---



## 이벤트처리

### 규칙
React event는 camelCase를 사용. `onclick(x)` `onClick(o)`    
문자열이 아닌 함수로 이벤트 핸들러를 전달함.
```js
<button onclick="eventName()"> //js의 event처리 방식
<button onClick={eventName}>   //jsx의 이벤트 처리 방식
```
false를 반환하는 이벤트여도 기본 동작 수행.  
preventDefault를 호출하여 해당 기본 동작 수행을 방지.
```js
<a href="#" onclick="console.log('the link was clicked.'); return false">
//js 에서 새페이지 여는것을 방지하는 코드

//jsx에서 새페이지 여는것을 방지하는 코드
fucntion ActionLink(){
	function handleClick(e){//합성 event를 파라미터로 갖는다.
		e.preventDefault();
		console.log('the link was clicked.');
	}
	return(
		<a href="#" onClick={handleClick}>
		<>
	)
}
```

> [합성이벤트란?](https://ko.reactjs.org/docs/events.html)  


### __클래스 컴포넌트에서 이벤트 핸들링__ 

- 이벤트핸들러를 클래스 메소드로 만듬.  
이때, construtor에서 바인딩해줘야 콜백에서 this가 작동한다. 만약 setState등을 사용하고자 한다면 바인딩하자.   
 이왕이면 default로 바인딩한다 생각하자.   
일반적으로 js에서 onClick={this.function}같이 뒤에 ()를 사용하지 않고 메서드를 참조하는 경우 해당 메서드를 바인딩한다.

> [바인딩](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)  

```js
class Counting extends React.Component {
	constructor(props){
		super(props);
		this.state = {count : 0};
		//콜백에서 this를 사용하고싶으면 바인딩!
		this.handleClick = this.handleClick.bind(this);
	}
  
	handleClick(){
		this.setState((state)=>({
        count : state.count+1
      })
    )
	}
	render(){
		return (
      <button onClick={this.handleClick}>
        {this.state.count}
      </button>
		);
	}
}

ReactDOM.render(
	<Counting />,
  document.getElementById('root')
);
```

바인딩 사용이 싫다면 콜백에 에로우 펑션을 사용하면된다.

```js
//대안 1 퍼블릭클래스 필드문법 사용
  handleClick = () =>{ 
    this.setState((state)=>({
      count: state.count+1
    }))
  }
//대안 2 errow function 사용
<button onClick={() => this.handleClick()}>
```

<span style="color:grey; font-size:12px;">
대안 1의 방법은 실험적인 문법으로 문제가 발생 할 수 있고, 대안 2는 컴포넌트의 리턴값이 렌더링 될때마다 다른 콜백이 생성될수 있다.<br> 콜백이 하위 컴포넌트에 props에 전달되는경우 하위 컴포넌트들은 다시 렌더링되는 성능문제가 발생 할수 있으므로, 바인딩이나 대안 1을 사용하는것을 권장한다.</span>

__이벤트 핸들러에 인자 전달__

다음 두 문장은 동일한 의미를 갖는다.
```jsx
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>  
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
에로우펑션 사용시 명시적인 e 인자 전달을 사용하고, bind를 사용하는경우 추가 인자가 자동으로 전달된다.


## 조건부 렌더링
JS에서 조건 처리와 동일하지만 몇가지 집고 넘어갈점을 체크하자.  
1. 논리 && 연산자 사용  
JS에서 `true && expression` 연산은 항상 `expression`으로 평가되고, `false && expression`은 항상 `false`로 평가된다. 다음과 같은 활용이 가능하다.
```jsx
return(
	{this.state.count < 10 &&
		<h1>
		{this.state.count}
		</h1>
	}
)
```
2. ``condition ? true : false`` 조건부 연산자 사용.  

3. 컨포넌트가 렌더링 안되게 하기.  
컴포넌트가 렌더링 결과를 반환하지 않고, ``null``을 반환 하면 렌더링하지 않는다.

4. 컴포넌트에 복잡한 조건문이 사용된다 -> 컴포넌트를 분리하자!

## 리스트 & 키
JSX에서 리스는 JS의 리스트와 map사용이 같다. 그러므로 리스트 사용에서 key의 쓰임을 알아보자.  

### __key__
리스트내 엘리먼트에 안정적인 고유성을 부여하기위해 지정한 값  
보통 데이터의 ID값을 key로 사용.
- 항목에 대한 ID값이 없는경우 항목의 인덱스를 key로 사용가능  
	=> 항목의 순서가 바뀌는 경우 성능저하, 안정성 저하가 있음.  
	만약 key를 별도로 지정하지 않으면 Deafult는 인덱스를 key로 사용.
key는 주변 배열의 context에서만 의미를 갖는다.
- map 함수 내부에 있는 엘리먼트에 key를 넣는것을 추천
- 배열 내부에서 key는 고유한 값을 갖는다.
- key는 구분하기위한 값으로 컨포넌트에 전달되지 않는다

## 제어 컨포넌트 (폼)
React에 의해 값이 제어되는 입력 폼 엘리먼트로 폼을 렌더링하는 ReactComponent는 사용자 입력값을 제어한다.

해당내용은 너무 길기에 [__gist링크__](https://gist.github.com/HankJIN/d0b9c557d1204c5c3110059096db5845)를 참고.  
 공식문서의 Forms를 참고하여 작성.[링크](https://ko.reactjs.org/docs/forms.html) 

## 코드 재사용

### State 끌어올리기

### 상속이 아닌 합성


<br>
<br>
<br>
<br>

---


# React로 클론코딩하기

## React로 생각하기
[링크 thinking-in-react](https://ko.reactjs.org/docs/thinking-in-react.html)를 보면 리액트로 프로젝트를 접근하는 법에대해 표현되어있다. 이에 맞춰 원하는 페이지를 개선시켜보자.
- 목표  
[롤링페이퍼](https://rollingpaper.site/) 사이트를 목표로 진행한다.  
해당 사이트에 인공지능 API를 추가해, 추가적인 기능구현을 목표로 진행한다.  
계획중인 인공지능은 감정분석을 통해, 필터링을 제공하는것이 목표이다.



## 참고
> [리액트 공식문서](https://ko.reactjs.org/tutorial/tutorial.html)