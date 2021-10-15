# React는 어떻게 만들어졌고 왜 사용할까?

## 프론트엔드 라이브러리와 프레임워크의 등장
프론트엔드 관련 라이브러리와 프레임워크의 등장 이전에는 HTML, CSS, 순수 Javascript 만을 사용해서 웹 페이지를 만들었다. 웹 페이지가 브라우저에 렌더링되는 과정에서 DOM 트리가 아래처럼 형성되는데 예를들어, 회원가입시 입력 form 검사 후 빨간색 경고메시지 표시, 추가 버튼 클릭시 아이템 추가, 단순 텍스트, 엘리먼트 요소 표시 등 단순한 기능 추가시에도 직접 DOM에 접근하여 개발이 필요하다. 

예를들어 다음과 같은 HTML 코드가 있다고 가정해보자.

```html
<div>
  <h1>Counter</h1>
  <h2 id="number">0</h2>
  <button id="increase">+</button>
</div>
```
우리가, 버튼을 눌러서 저 숫자 0 값을 바꿔주려면 각 DOM 엘리먼트에 대한 레퍼런스를 찾고, 해당 DOM 에 접근하여 원하는 작업을 해줘야하는데 지금은 간단해 보이지만 다양한 기능이 있을 경우 이러한 작업을 계속 해줘야 하는 것 이다.

```javascript
let number = 0;
const $elementNumber = document.getElementById('number');
const $increaseBtn = document.getElementById('increase');

$increaseBtn.onclick = function() {
  number++;
  elementNumber.innerText = number;
}
```

![dom-tree](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/DOM-model.svg/1200px-DOM-model.svg.png)

그러나 요즘 웹 서비스는 다양한 기능과 이용자와 페이지간에 즉각적인 상호작용을 요구하는 등 웹 서비스의 규모가 커짐에 따라 위 DOM 트리에 접근하여 개발하는데 상당한 공수가 들어가고 생산성이 떨어질거라 예상할 수 있다. 또한, DOM의 한 요소에 변경이 필요하더라도 전체 DOM들을 다시 렌더링 해야 하기 때문에 성능저하의 영향이 있을 수 있다. 이에 따라 프론트엔드를 더 편리하게 개발할 수 있도록 해주는 다양한 프레임워크와 라이브러리가 등장하게 되었다. 그 중 현재 많은 사랑을 받고있는 React 라이브러리에 대해 좀 더 깊게 조사해보고자 했다.  

![js-framework](https://user-images.githubusercontent.com/35620465/137409798-17cdf3a9-0a17-47d6-8865-23b1b351a9cb.png)

## React 소개

> 사용자 인터페이스를 만들기 위한 JavaScript 라이브러리   

> -선언형-  
React는 상호작용이 많은 UI를 만들 때 생기는 어려움을 줄여줍니다. 애플리케이션의 각 상태에 대한 간단한 뷰만 설계하세요. 그럼 React는 데이터가 변경됨에 따라 적절한 컴포넌트만 효율적으로 갱신하고 렌더링합니다. 선언형 뷰는 코드를 예측 가능하고 디버그하기 쉽게 만들어 줍니다.

> -컴포넌트 기반-  
스스로 상태를 관리하는 캡슐화된 컴포넌트를 만드세요. 그리고 이를 조합해 복잡한 UI를 만들어보세요. 컴포넌트 로직은 템플릿이 아닌 JavaScript로 작성됩니다. 따라서 다양한 형식의 데이터를 앱 안에서 손쉽게 전달할 수 있고, DOM과는 별개로 상태를 관리할 수 있습니다.

> -한 번 배워서 어디서나 사용하기-  
기술 스택의 나머지 부분에는 관여하지 않기 때문에, 기존 코드를 다시 작성하지 않고도 React의 새로운 기능을 이용해 개발할 수 있습니다. React는 Node 서버에서 렌더링을 할 수도 있고, React Native를 이용하면 모바일 앱도 만들 수 있습니다.

React 공식문서에 가보면 제일 먼저 React를 위와같이 소개하고 있다. 그런데 저 중 선언형 프로그래밍이 정확히 어떤 말을 하는것인지 와닿지 않을 수 있다.  

### 선언형 프로그래밍이란?
- 규칙에 기반하여 input과 output에 집중해서 프로그래밍 하는 방법이다.  

명령형
- input이 주어졌을 때에 정해진 output이 존재하는 것이 아니라 특정 task를 실행한다.  
  ex) `export const ExecuteSomeTask = () => console.log('hello world')`
- 항상 똑같은 output을 보장하지 않는다.

선언형 
- input이 주어지면 어떤 output이 나올지 예상할 수 있다.  
  ex) `export const toLowerCase = (input) => input.toLowerCase()`
- 똑같은 input에 대해서는 항상 똑같은 output을 보장한다.

UI 개발도 선언형 프로그래밍 방식으로 작성하면 훨씬 더 수월하게 작성할 수 있다고 한다.  
선언형으로 작성하게 되면 함수안은 어떻게 구현되어 있는지, 이 함수는 무슨일을 하는지 등을 생각하지 않아도 되는데 명령형 프로그래밍 방식으로 작성하게 되면 함수를 사용하는데 생각을 더 하게 되고 또 코드 규모가 커지면 더 파악하기가 어려워지는 일이 발생한다.

### 선언형 vs 명령형 예제

### 명령형
```javascript
export default () => {
   const container = document.getElementById('app')
   const countdownContainer = document.createElement('div')
   countdownContainer.classList.add('countdown-container')
   container.appendChild(countdownContainer)

   const titleElement = document.createElement('div')
   titleElement.classList.add('title')
   titleElement.innerHTML = '남은 시간'
   countdownContainer.appendChild(titleElement)
   
   // ...
   // ...
   // ...
}
```
위 코드의 특징은 정해진 규칙 안에서 UI를 그리고 컴포넌트화 시키는 코드를 작성하기보다는 수행이 필요한 작업들을 순차적으로 진행한다.  

### 선언형
```javascript
// Component라는 Class를 정의하여 UI 개발의 규칙을 만들고
// 하나하나 직접 과정들을 명령하는 것이 아니고 Component를 선언하고
// render, state만을 컨트롤하고 UI를 그리는 것은 Component의 규칙에 따른다.
export default () => {
   class Component {
      constructor({ $target, as = 'div', initialState = {}, ...props }) {
         this.$target = $target
         this.$element = document.createElement(as)
         this.props = props || {}
         this.state = initialState
         this.render()
      }

      renderComponent() {
         // ...
      }

      setState(partialState) {
         this.state = { ...this.state, ...partialState }
         this.render()
      }

      updateDOM() {
         // ...
      }
   }
}
```
위 코드의 특징은 Component라는 클래스를 만들어서 그 안에서 정해진 규칙(contructor, renderComponent, setState 등)을 만들고 그 규칙에 따라 컴포넌트를 그리는 개발을 한다.

React는 선언형 프로그래밍을 할 수 있게 도와준다. React가 내부에 어떻게 구현되어 있고 어떻게 output을 내는지는 생각할 필요 없이 개발자는 View만 설계하면 된다. 그렇다면 React가 알아서 구현된 생명주기 메소드를 통해 적절하게 state와 props를 초기화, 업데이트 하고 렌더링 하여 화면에 요소들을 그려줄 것 이다.  

## React의 Virtual DOM(가상 DOM)

React가 Virtual DOM을 활용하는 방식이라는 얘기를 많이 들어봤을 것이다. Virtual DOM은 단순 Javascript Object 들이다. 이런 Object들은 Facebook이 만든 'diffing' 알고리즘(비교 알고리즘) 통해 변경을 확인하는데 이 과정을 재조정(Reconciliation) 이라고 한다. 이후 변경된 내용으로 DOM트리 전체를 렌더링 하는것이 아니라 해당 부분만 변경된 내용을 업데이트 시킨다. 무엇보다 DOM을 직접 조작하는 것이 아니라 In-memory의 가상 DOM을 조작하기 때문에 효율적이라고 빠르다.

> React는 항상 전체 앱을 재렌더링할 수도 있지만, 최종적으로 출력되는 결과는 항상 같을 것입니다. 좀 더 정확히 말하자면, 여기서 말하는 재렌더링은 모든 컴포넌트의 render를 호출하는 것이지 React가 언마운트시키고 다시 마운트하는 것은 아닙니다.
즉, 앞서 설명했던 규칙에 따라 렌더링 전후에 변경된 부분만을 적용할 것입니다.

### Diffing 알고리즘

Diffing 알고리즘은 트리를 비교할 때 기본적으로 서브트리들의 위치(level-by-level)를 기준으로 비교한다.

![diffing](https://user-images.githubusercontent.com/35620465/137418952-bccc766c-8cb7-44d7-b578-984b17927c28.png)

### 같은 위치에서 엘리먼트의 타입이 다른 경우

- 기존 트리를 제거 후 새로운 트리 만든다.
- 기존 트리 제거시 트리 내부의 엘리먼트/컴포넌트들은 모두 제거한다.
- 새로운 트리를 만들 때 내부 엘리먼트/컴포넌트들도 모두 새로 만든다.

```javascript
{/* Before */}
<div>
  <Counter /> {/* Will unmount */}
</div>

{/* After */}
<span>
  <Counter /> {/* Will mount, Did mount */}
</span>
```

### 같은 위치에서 엘리먼트가 DOM을 표현하고 그 타입이 같은 경우

- 엘리먼트의 attributes를 비교한다.
- 변경된 attributes만 업데이트한다.
- 자식 엘리먼트들에 diff 알고리즘을 재귀적으로 적용한다.

```javascript
{/* Before */}
<div className="before" title="stuff" />

{/* After */}
<div className="after" title="stuff" /> {/* Update className */}
```

### 같은 위치에서 엘리먼트가 컴포넌트를 표현하고 그 타입이 같은 경우

- 컴포넌트 인스턴스 자체는 변하지 않는다.(때문에 컴포넌트의 state가 유지된다.)
- 컴포넌트 인스턴스의 업데이트 전 라이프 사이클 메서드들이 호출되며 props가 업데이트된다.
- render()를 호출하고, 컴포넌트의 이전 엘리먼트 트리와 다음 엘리먼트 트리에 대해 diff 알고리즘을 재귀적으로 적용한다.

```javascript
{/* Before */}
<Counter value="3" />

{/* After */}
{/* Will recevie props, Will update, Render --> diff algorithm recurses */}
<Counter value="4" />
```

### 리스트 엘리먼트
기본적으로 자식 엘리먼트들에 대해 반복적인 비교를 할 때, React는 이전/다음 상태의 자식 엘리먼트 목록을 함께 반복하고 그 차이를 본다. 따라서 엘리먼트들의 정렬과 같은 상황에 취약하다.

```javascript
{/* Before */}
<ul>
  <li>first</li> {/* prev-first */}
  <li>second</li>  {/* prev-second */}
</ul>

{/* After */}
<ul>
  <li>second</li> {/* Compares prev-first --> Update dom */}
  <li>first</li> {/* Compares prev-second --> Update dom */}
  <li>third</li> {/* Compares prev --> Insert dom */}
</ul>
```

### Key
엘리먼트들에게 Key 속성을 명시적으로 부여하여 위와 같은 상황에 발생하는 필요 없는 업데이트를 최소화시킬 수 있다. key는 하나의 서브 트리에서만 유니크한 값을 가지면 되고 각 렌더링에서 변경이 없어야 한다. 이 때문에 React 개발시 key 값을 안넣어 주면 에러에 직면하게 되는 것 이다.

참고: [React의 비교 알고리즘](https://ko.reactjs.org/docs/reconciliation.html#the-diffing-algorithm)

## Virtual DOM 구현해보기

[babeljs.io](https://babeljs.io/)

```javascript
const list = [
    { title: "React에 대해 알아봅시다." },
    { title: "Redux에 대해 알아봅시다." },
    { title: "Typescript에 대해 알아봅시다." },
]

const rootElement = document.getElementById("root")
```

위와 같이 간단한 list 객체 배열이 있다.  
이것은 rootElement의 innerHTML 을 이용해 문자열 Element를 생성해 직접적으로 넣어줄 수 있다.

```javascript
function app(items) {
  rootElement.innerHTML = `
    <ul>
      ${items.map((item) => `<li>${item.title}</li>`).join("")}
    </ul>
  `
}

app(list)
```
하지만 위의 app 함수는 innerHTML 같이 DOM에 직접 접근해 구조를 변경하고 있다.
React는 DOM을 Virtual DOM으로 변환해 조금 더 간단하게 개발자가 사용할 수 있도록 만들었다.

실제 React를 활용 했을때 기본적인 구조는 아래와 같이 구성된다.
```javascript
import React from "react"
import ReactDOM from "react-dom"

function App() {
  return (
    <div>
      <h1>Hello?</h1>
      <ul>
        <li>React</li>
        <li>Redux</li>
        <li>MobX</li>
        <li>Typescript</li>
      </ul>
    </div>
  )
}

ReactDOM.render(<App />, document.getElementById("root"))
```

App 함수의 반환값은 JSX 문법이며 HTML 태그와 비슷하게 사용해 컴포넌트를 만든다.
ReactDOM은 render라는 정적메서드를 가지며 2개의 인자를 받는다.
첫 번째 인자는 화면에 렌더링할 컴포넌트이며 두 번째는 컴포넌트를 렌터링할 요소다.
위의 코드를 아래와 같이 컴포넌트를 분리할 수 도 있다.

```javascript
import React from "react"
import ReactDOM from "react-dom"

function StudyList() {
  return (
    <ul>
      <li>React</li>
      <li>Redux</li>
      <li>MobX</li>
      <li>Typescript</li>
    </ul>
  )
}

function App() {
  return (
    <div>
      <h1>Hello?</h1>
      <StudyList />
    </div>
  )
}

ReactDOM.render(<App />, document.getElementById("root"))
```

App 내부에 있던 ul > li 컴포넌트들을 StudyList 함수로 만들어 분리했다.
컴포넌트를 분리하면서 StudyList 같이 이름을 갖는 함수가 생기며 코드를 관리하기 더 쉬워졌다.

```javascript
function StudyList() {
  return (
    <ul>
      <li className="item">React</li>
      <li className="item">Redux</li>
      <li className="item">MobX</li>
      <li className="item">Typescript</li>
    </ul>
  )
}
```
위의 StudyList 함수에서 반환하는 JSX 엘리먼트들을 vdom이라는 객체로 바꾸어 아래와 같이 표현해봤다.  

```javascript
const vdom = {
  type: "ul",
  props: {},
  children: [
    { type: "li", props: { className: "item" }, children: "React" },
    { type: "li", props: { className: "item" }, children: "Redux" },
    { type: "li", props: { className: "item" }, children: "Typescript" },
    { type: "li", props: { className: "item" }, children: "MobX" },
  ],
}
```
```<StudyList/>```에 item, id라는 속성을 넘기는 경우 StudyList 함수는 아래와 같이 바뀌고  

```javascript
function StudyList(props) {
  return (
    <ul>
      <li className="item">React</li>
      <li className="item">Redux</li>
      <li className="item">MobX</li>
      <li className="item">Typescript</li>
    </ul>
  )
}

function App() {
  return (
    <div>
      <h1>Hello?</h1>
      <StudyList item="abcd" id="hoho" />
    </div>
  )
}
```
위와 같이 변경된 코드에 따라 생기는 StudyList의 Virtual DOM은 아래와 같을 것이다.

```javascript
const vdom = {
  type: "ul",
  props: { item: "abcd", id: "hoho" },
  children: [
    { type: "li", props: { className: "item" }, children: "React" },
    { type: "li", props: { className: "item" }, children: "Redux" },
    { type: "li", props: { className: "item" }, children: "Typescript" },
    { type: "li", props: { className: "item" }, children: "MobX" },
  ],
}
```
위와 같이 생성된 객체를 render 메서드가 실제 HTML 태그로 변경시켜 화면에 그려준다.
위의 vdom 객체 구성에 따라 React는 하나의 부모에서 아래의 자식들을 만드는 방식을 갖는다.

참고: [didact](https://pomb.us/build-your-own-react/)