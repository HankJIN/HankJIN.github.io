---
title:  "리액트(React) 정리#2"
excerpt: "리액트에 대한 기초적인 개념을 정리#2"

categories:
  - Leaning
tags:
  - React
last_modified_at: 2020-9-22 T10:00:00-05:00
toc: true
toc_sticky: true
---

<span style="color:grey; font-size:12px;">
리액트의 element, state, component에 대해 학습한다.<br> 리액트의 핵심이 되는 개념이므로, 각 예시를 반드시 구현하고 codepen에서 실행해보자.
</span>

---



## React element  
element는 리액트 앱의 최소 단위로, 화면에 표시할 내용을 기술함.   
Babeld은 JSX를 React.createElement() 호출로 컴파일하고, 다음과 같다.  
__React Element는 불변 객체__ 이다  : 특정 시점의 UI를 보여주는것뿐이다.  
(엘리멘트 업데이트는 element 렌더링에서 다룬다.)  
```jsx
//다음 두 문장은 동일하다.  
const element = (  
	<h1 className="greeting">  
		Hello, world!  
	</h1>  
);  
const element = React.createElement(  
	'h1',
	{className: 'greeting'},
	'Hello, world!'
);
```
 이때 createElement로 생성된 객체(= JSX로 생성된 객체)를 React element이고, 화면에 표시하고자 하는 항목 값이라 생각하면된다. React가 이러한 element들을 읽고, DOM에 렌더링을 한다.  


### __element 렌더링__
root DOM 노드 : index.html에 있는 ``<div id="root"></div>``에 사용되는 모든 element들은 React DOM에서 관리함.   
엘리멘트를 root DOM 노드에 렌더링하는 방법  

 ``ReactDOM.render(element, document.getElementById('root'));`` 
<br>
 업데이트를 하려면 element가 변경 될시 다시 ReactDOM.render()을 호출해야될까?   

> 아니다. 컴포넌트(component)와 상태(state)의 개념으로 해결한다. 보통 ReactDOM.render()은 한번만 호출된다.  

<br>

---

## __component__
재사용가능한 개별적인 UI조각  
- 함수 컴포넌트  JS의 함수와 유사한형태로 'props'라는 파라미터를 받아 React element를 반환한다.
- 클래스 컴포넌트 : ES6의 class로 정의된 컴포넌트로 React.Component를 상속받는다.  

```js
// 함수로 정의된 컴포넌트 : props 파라미터를 받는다.
function Welcome(props){
	return <h1>Hello, {props.name}</h1>;
}
// ES6의 class로 정의된 컴포넌트 : React.Component를 상속받는다.
class Welcome extends React.Component{
	render(){
		return <h1>Hello, {this.props.name}</h1>;
	}
}
//컴포넌트를 사용한 엘리먼트 생성

const element = <Welcome name="hank" />;
ReactDOM.render(
	element,
	document.getElementById('root')
);

//1. <Welcome name="hank" /> element로 ReactDOM.render호출
//2. {name = 'hank'}의 객체를 매개변수로하여 Welcome 컴포넌트 호출 
//3. 컴포넌트는 element인 <h1>hello,hank</h1>반환
//4. ReactDOM이 반환된 element에 일치하도록 DOM을 업데이트해줌.
```

### __규칙__  

네이밍은 기본적으로 uppercamelcase 한다. 컴포넌트명이 소문자로 시작하는경우 `<>`안에서 React는 HTML 태그로 인식한다고 한다.  
> 참고 : [React는 소문자로 시작하는 컴포넌트를 DOM 태그로 처리합니다.](https://ko.reactjs.org/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized)  

컴포넌트는 다른 컴포넌트를 자신의 출력에 합성 가능하다.  
<span style="color:grey; font-size:12px;">이를통해 컴포넌트를 여러개의 작은 컴포넌트로 표현가능하다.<br> React 앱에서는 버튼, 폼, 다이얼로그, 화면 등의 모든 것들이 흔히 컴포넌트로 표현된다.
</span>


props는 __읽기전용__ 으로 컴포넌트가 컴포넌트 자체 props를 수정해서는 안된다.  

__즉, React 컴포넌트는 자신의 props를 다룰 때 반드시 순수함수로 동작해야된다__
  


-> 이러한 props제한을 해결하고자 State를 사용한다.

----

## __STATE__
State는 props와 유사하지만, 비공개이며 컴포넌트에 의해 완전히 제어되는 차이가 있음!  

- 함수형 컴포넌트를 클래스로 변경하기
1. React.Component를 확장하는 클래스 생성  
``class Welcome extends React.Component{}``
2. render() 메서드를 추가후, 함수 컴포넌트 내부의 내용을 복사  
``render(){ context }``
3. context내 props를 this.props 로 변경한다.

	```js
	class Welcome extends React.Component{
		render(){
			<h1>
				hello, {this.props.name}
			</h1>
		}
	}
	```

- 클래스에 로컬 state 추가하기
1. this.props를 this.state로 변경
2. 클래스 생성자를 통해 this.state값을 지정한다.  
이때, 클래스컴포넌트는 항상 __super(props)__ 를 통해서 props를 기본 생성자에 전달해준다.

	```js
	class Welcome extends React.Component{
	constructor(props){
		super(props);
		this.state = {name : props.name};
	}
		render(){
			return <h1>Hello, {this.state.name}</h1>;
		}
	}
	//컴포넌트를 사용한 엘리먼트 생성

	ReactDOM.render(
		<Welcome name="hank" />,
		document.getElementById('root')
	);
	```

### __생명주기 메소드__
컴포넌트가 삭제될때, 컴포넌트가 사용중이던 리소스 확보를 위해 생명주기와 생명주기 메서드들을 파악해보자.  
생명주기 메소드 : 컴포넌트클래스에서 마운트 되거나 업데이트 될때마다 호출되는 메서드.  <br>

- Mounting : 컴포넌트 리턴값이 렌더링 되거나  
componentDidMount(): 컴포넌트 리턴값이 DOM에 렌더링 된 후 호출되는 메서드.   
componentWillUnmount() : 컴포넌트가 DOM에 렌더링이 취소 된 후 호출되는 메서드.

### __setState() 메소드__
컴포넌트 로컬 State를 업데이트하고, 변경되었음을 알려 변경된 State값을 다시 렌더링 하도록 하는 메서드.
1. state를 직접 수정하지않고, setState를 이용한다.  
_this.state를 통한 state값 지정은 constructor에서만 가능하다._  
``this.state.commnet = 'hello';`` => ``this.setState({comment: 'hello'});``
2. state 업데이트는 비동기적으로 생각하자  
this.props와 this.state는 비동기적으로 업데이트 될 수 있으므로, 객체값을 state에 넣지 말고, 함수의 인자 형태로 넣어주자.

	```js
	//Wrong
	this.setState({
		counter : this.state.cnt + this.props.inc,
	})
	//Correct
	this.setState((state, props) => {
		counter : state.cnt + props.inc;
	})
	```

3. setState()를 호출할 때 React는 제공한 객체를 현재 state로 병합  
_독립적인 업데이트가 가능하다_

4. state는 종종 로컬 또는 캡슐화라고 불립니다. state가 소유하고 설정한 컴포넌트 이외에는 어떠한 컴포넌트에도 접근할 수 없습니다.

5. 상속에서  컴포넌트는 자신의 state를 자식 컴포넌트에 props로 전달할 수 있습니다.  
``<ChildComponent date={this.state.date} />``  
하향식 데이터 흐름 : ChildComponent에서 사용되는 date는 부모컴포넌트의 props인지 state인지 알수 없음.  모든 state는 항상 특정한 컴포넌트가 소유하고 있으며 그 state로부터 파생된 UI 또는 데이터는 오직 트리구조에서 자신의 “아래”에 있는 컴포넌트에만 영향을 미침.

### __STATE vs PROPS__
리액트 공식 문서페이지에 다음과같은 정보가 나와있다. 인자를 State로 사용할 것인지 Props로 사용할 것인지 다음과 같은 기준에서 생각해보자!.

1. 부모로부터 props를 통해 전달됩니까? 그러면 확실히 state가 아닙니다.
2. 시간이 지나도 변하지 않나요? 그러면 확실히 state가 아닙니다.
3. 컴포넌트 안의 다른 state나 props를 가지고 계산 가능한가요? 그렇다면 state가 아닙니다.









## 참고
> [리액트 공식문서](https://ko.reactjs.org/tutorial/tutorial.html)