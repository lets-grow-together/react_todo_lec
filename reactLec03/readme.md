# 03 React JS 기초 1

- JSX
- 요소 렌더링
- 컴포넌트와 props
- state와 라이프사이클

*****

## JSX

```jsx
const element = <h1>Hello, world!</h1>;
```

JSX는 자바스크립트의 문법 확장이다. 리액트는 JSX를 사용하여 UI가 어떻게 보일 지 설명하는 걸 권장한다. JSX는 템플릿 언어처럼 보일 수 있지만, 자바스크립트를 기반으로 한다. 이 형식으로 작성한 코드는 나중에 번들링되면서 `React.createElement(...)` 함수를 실행하고 자바스크립트 객체로 변환된다.

### JSX에 표현식 포함하기

JSX 안에 자바스크립트 표현식 을 중괄호(`{}`)로 묶어서 포함시킬 수 있다.

```jsx
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)!
  </h1>
);
```

### JSX 또한 표현식이다.

트랜스파일이 끝나면, JSX 표현식이 자바스크립트 함수 호출로 전환되고, 자바스크립트 객체를 반환한다.

이 말은 `if` 문이나 `for` 반복 내에서 JSX를 사용할 수 있으며, 변수에 할당하거나 매개 변수로 전달하거나 함수에서 반환할 수 있음을 의미한다.

```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### JSX의 props(속성) 정의

속성에 따옴표를 이용해 문자열 리터럴을 정의할 수 있다.

```jsx
const element = <div tabIndex="0"></div>;
```

속성에 중괄호를 이용해 자바스크립트 표현식을 포함시킬 수 있다.

```jsx
const element = <img src={user.avatarUrl} />;
```

속성에서 자바스크립트 표현식을 포함시킬 때 중괄호를 따옴표로 묶지 않는다.

### JSX의 자식 정의

JSX 태그는 자식을 가질 수 있습니다.

```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

만약 태그가 비어있다면, XML 처럼 `/>` 를 이용해 닫아준다.

```jsx
const element = <img src={user.avatarUrl} />;
```

### JSX 객체 표현

Babel은 JSX를 `React.createElement()`를 호출하도록 트랜스파일한다.

```jsx
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>

// compiles into

React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
);
```

```jsx
<div className="sidebar" />

// compiles into

React.createElement(
  'div',
  {className: 'sidebar'},
  null
);
```

```jsx
const element = (
  <div className="shopping-list">
    <h1>Shopping List for {this.props.name}</h1>
    <ul>
      <li>Instagram</li>
      <li>WhatsApp</li>
      <li>Oculus</li>
    </ul>
  </div>
);

// compiles into

const element = React.createElement('div', {className: 'shopping-list'}, 
  React.createElement('h1', /* ... h1 children */),
  React.createElement('ul', /* ... ul chindren */)
);
```

[변환되는 JSX 확인하기 - Babel](http://bit.ly/2GffJsp)

JSX는 React.createElement 를 호출하도록 트랜스파일되고, `React.createElement()`는 아래와 같은 객체를 생성한다.

```jsx
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

const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

JSX 장점.

- XML의 특성을 이용한 요소트리로 UI를 표현하는 데 적합하다.
- 애플리케이션의 구조를 시각화하기 쉬우며 간결하다.
- 일반 자바스크립트이므로 언어의 의미를 변형시키지 않는다.

### JSX 문법요약

- 최상단에는 반드시 하나의 엘리먼트만 존재해야 한다(단일 루트 노드).
- 모든 태그는 닫는 태그가 존재해야 한다. 단일 태그로도 표현이 가능하다.
    + ex. `<div></div>` === `<div />`
- 리액트 컴포넌트를 태그처럼 사용할 수 있다.
    + ex. `<Parent> <Child /> </Parent>`
- 리액트 컴포넌트는 html 태그와 구분하기 위해 관행적으로 첫글자를 대문자로 사용한다.
- 주석은 `{/* ... */}` 식으로, container의 안쪽에서만 사용 가능하다.
- 자바스크립트 표현식 처리 : `{ }`으로 감싼다('문'형태는 불가).
- html의 hyphens -> **camelCase** 로 표기해합니다.
- `class` => `className` (javascript property)
- `<input type="text" maxlength="30">` -> `maxLength`
- `<label for="id">` -> `htmlFor`

*****

## 요소 렌더링

리액트 요소는 React 앱의 가장 작은 구성 블록이다.

```jsx
const element = <h1>Hello, world</h1>;
```

### DOM 에서 렌더링 하기

```html
<div id="root"></div>
```

```jsx
const element = <h1>Hello, world</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

### 렌더링된 요소 업데이트

React 요소는 변경 불가능하다. 한번 요소를 만들었다면, 그 자식이나 속성을 변경할 수 없다. UI를 업데이트할 수 있는 유일한 방법은 새로운 요소를 만드는 것이며, 이 요소를 `ReactDOM.render()` 로 전달하는 것이다.

**Clock Component**

### React는 필요한 것만 업데이트한다.

*****

## 컴포넌트와 props

개념상 컴포넌트는 자바스크립트 함수와 비슷하다. 임의의 입력(props)을 받고 어떤게 화면에 나타나야 하는 지를 설명하는 React 요소를 반환한다.

- 재사용, 확장, 유지 관리하기 쉽고 특정 목적을 가진 독립형 구성요소
- UI를 구성하는 개별적인 뷰 단위. 전체 앱은 각 Component들이 결합해서 만들어지고, 각 Component들은 앱의 다른 부분, 또는 다른 앱에서 쉽게 재사용이 가능하다.

### 함수형 / 클래스 컴포넌트

컴포넌트를 정의하는 가장 간단한 방법은 자바스크립트 함수로 작성하는 것이다.

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

이 함수는 단일 props(속성) 객체 인수를 받고 React 요소를 반환하기 때문에 유효한 React 컴포넌트이다. 이러한 컴포넌트는 말 그대로 자바스크립트 함수이기 때문에 **함수형**이라고 부른다.

컴포넌트를 정의하기 위해 ES6 class 를 사용할 수도 있다.

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

위의 두 컴포넌트는 React 관점에서 보면 동일하다. 클래스는 몇가지 기능을 더 가지고 있다.

### 컴포넌트 렌더링

리액트 요소에 유저가 정의한 컴포넌트를 나타낼 수도 있다.

```jsx
const element = <Welcome name="Sara" />;
```

React가 사용자 정의 컴포넌트를 나타내는 요소를 만나면, JSX 속성을 이 컴포넌트의 단일 객체로 전달한다. 우리는 이 객체를 props 라고 부른다.

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> 컴포넌트 이름은 항상 대문자로 시작하기를 권장한다. `<div />` 는 DOM 태그를 나타내지만 `<Welcome />` 은 컴포넌트를 나타내며 스코프에 Welcome 을 필요로 한다.

### 컴포넌트 결합

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

### props는 읽기 전용

컴포넌트를 함수나 클래스 중 어떤 걸로 선언했 건, props를 수정할 수 없다. sum 함수를 살펴본다.

```js
function sum(a, b) {
  return a + b;
}
```

이런 함수는 입력을 변경하려 하지 않고 항상 동일한 입력에 대해 동일한 결과를 반환하기 때문에 **순수함수**라고 부른다.

대조적으로, 이 함수는 입력을 변경하기 때문에 순수하지 않다.

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

모든 React 컴포넌트는 props와 관련한 동작을 할 때 순수 함수처럼 동작해야한다.

물론 어플리케이션 UI는 동적이며 시간이 지남에 따라 변한다. React의 **state**는 React 컴포넌트가 이 규칙을 어기지 않고 유저 액션, 네트워크 응답, 기타 등등에 대한 응답으로 시간 경과에 따라 출력을 변경할 수 있게 한다.

*****

## state와 라이프사이클

**Clock Component state 추가**

state는 props와 비슷하지만 컴포넌트에 의해 완전히 제어되며 private 속성이다.

클래스로 정의한 컴포넌트에는 몇가지 추가 기능이 있다. 로컬 state는 클래스 컴포넌트에서만 사용 가능한 기능이다.

### 함수형을 클래스로 변환

클래스로 컴포넌트를 정의하면 로컬 `state`나 라이프사이클 훅 같은 추가 기능을 사용할 수 있다.

### Class에 로컬 state 추가하기

### 클래스에 라이프사이클 메서드 추가하기

많은 컴포넌트를 가진 어플리케이션에서, 컴포넌트가 제거될 때 리소스를 풀어주는 건 아주 중요한 일이다.

Clock 이 DOM에 최초로 렌더링 될 때 타이머를 설정 하려한다. React에서 이를 **mounting** 이라고 부른다.

그리고 DOM에서 Clock 을 삭제했을 때 타이머를 해제 하려하느데, React에서 이를 **unmounting** 이라고 부른다.

컴포넌트가 마운트(mount) 되고 언마운트(unmount) 될 때 특정 코드를 실행하기 위해 컴포넌트 클래스에 특별한 메서드를 선언할 수 있다.

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    // ...
  }

  componentDidMount() { }

  componentWillUnmount() { }

  render() {
    return // ...
  }
}
```

이런 메서드들을 **라이프사이클 훅** 이라고 부른다.

`componentDidMount()` 훅은 컴포넌트 출력이 DOM에 렌더링 된 이후 동작한다. 여기에 타이머를 설정한다.

`componentWillUnmount()` 라이프사이클 훅에서 타이머를 종료한다.

어떤 작업을 했는 지와 메서드가 호출되는 순서를 간단히 요약해보자.

1. `<Clock />` 이 `ReactDOM.render()` 에 전달될 때, React는 Clock 컴포넌트의 생성자 함수를 호출한다. Clock이 현 시간 화면에 보여질 때, 현 시간을 포함하는 `this.state` 객체를 초기화한다. 이 state는 추후 업데이트한다.

2. React가 Clock 컴포넌트의 `render()` 메서드를 호출한다. React가 어떤 걸 화면에 보여줘야 하는 지 배우는 방법이다. 그다음 React는 Clock 의 렌더링 출력과 일치하도록 DOM을 업데이트한다.

3. Clock 출력이 DOM에 주입되었을 때, React는 `componentDidMount()` 라이프 훅을 호출한다. 내부에서, Clock 컴포넌트는 브라우저에게 컴포넌트의 `tick()` 메서드를 초당 한번 호출하는 타이머를 설정하라고 요구한다.

4. 브라우저에서 매 초마다 `tick()` 메서드를 호출합니다. 내부에서, Clock 컴포넌트는 현재 시간을 포함하는 객체로 `setState()` 를 호출하여 UI 업데이트를 예약한다. `setState()` 호출 덕분에, React는 상태가 변경된 걸 알고 있고, `render()` 메서드를 다시 한번 호출해 스크린에 무엇이 있어야 하는 지 알 수 있다. 이번에는, `render()` 메서드 내의 `this.state.date` 가 달라지므로 렌더 출력에 업데이트된 시간이 포함된다. React는 그에 따라 DOM을 업데이트한다.

만약 Clock 컴포넌트가 DOM에서 삭제되었다면, React는 `componentWillUnmount()` 라이프사이클 훅을 호출하고 타이머를 멈춘다.

*****

## 라이프사이클의 이해

라이프사이클 메서드의 종류는 총 열 가지이다. **Will** 접두사가 붙은 메서드는 어떤 작업을 작동하기 **전**에 실행되는 메서드고, **Did** 접두사가 붙은 메서드는 어떤 작업을 작동한 **후**에 실행되는 메서드이다.

라이프 사이클은 총 세 가지, 즉 **마운트**, **업데이트**, **언마운트** 카테고리로 나눈다.

| 구분 | 동작 |
| ---- | ---- |
| 마운트 | 페이지에 컴포넌트가 나타남 |
| 업데이트 | 컴포넌트 정보를 업데이트 -> 리렌더링 |
| 언마운트 | 페이지에서 컴포넌트가 사라짐 |

### 마운트

DOM이 생성되고, 웹 브라우저상에 나타나는 것을 마운트(mount)라고 한다. 이때 호출하는 메서드는 다음과 같다.

**마운트할 때 호출하는 메서드**

* constructor: 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드
* getDerivedStateFromProps: props에 있는 값을 state에 동기화하는 메서드
* render: UI를 렌더링하는 메서드
* componentDidMount: 컴포넌트가 웹 브라우저상에 나타난 후 호출하는 메서드

### 업데이트

컴포넌트를 업데이트할 때는 총 네 가지 경우이다.

1. props가 바뀔 때
2. state가 바뀔 때
3. 부모 컴포넌트가 리렌더링될 때
4. `this.forceUpdate`로 강제로 렌더링을 트리거할 때

컴포넌트를 업데이트할 때는 다음 메서드를 호출한다.

* getDerivedStateFromProps: 이 메서드는 마운트 과정에서도 호출하며, props가 바뀌어서 업데이트할 때도 호출한다.
* shouldComponentUpdate: 컴포넌트가 리렌더링을 해야 할지 말아야 할지를 결정하는 메서드, 여기서 `false`를 반환하면 아래 메서드들을 호출하지 않는다.
* render: 컴포넌트를 리렌더링 한다.
* getSnapshotBeforeUpdate: 컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드
* componentDidUpdate: 컴포넌트의 업데이트 작업이 끝난 후 호출하는 메서드

### 언마운트

마운트의 반대 과정, 컴포넌트를 DOM에서 제거하는 것을 언마운트(unmount)라고 한다.

* componentWillUnmount: 컴포넌트가 웹 브라우저상에서 사라지기 전에 호출하는 메서드

*****

## 살펴보기

### `render()` 함수

```jsx
render() { ... }
```

render 메서드는 컴포넌트의 모양을 정의한다. 라이프사이클 메서드 중 유일한 필수 메서드.

이 메서드 안에서 `this.props`와 `this.state`에 접근할 수 있으며, 리액트 요소를 반환한다. 아무것도 보여주고 싶지 않다면 `null` 이나 `false`를 반환한다.

이 메서드 안에서는 절대로 state를 변경해서는 안되며, 웹 브라우저에 접근해도 안된다. DOM 정보를 가져오거나 변화를 줄 때는 componentDidMount에서 처리한다.

### `constructor` 메서드

```js
constructor(props) { ... }
```

컴포넌트의 생성자 메서드로 컴포넌트를 만들 때 처음으로 실행된다. 이 메서드에서는 초기 state를 정할 수 있다.

### `getDerivedStateFromProps` 메서드

리액트 v16.3 이후에 새로 만든 라이프사이클 메서드. props로 받아온 값을 state에 동기화시키는 용도로 사용하며, 컴포넌트를 마운트하거나 props가 변경될 때 호출한다.

```jsx
static getDerivedStateFromProps(nextProps, prevState) {
  // 여기서는 setState 를 하는 것이 아니라
  // 특정 props 가 바뀔 때 설정하고 설정하고 싶은 state 값을 리턴하는 형태로
  // 사용된다.
  if (nextProps.value !== prevState.value) {  // 조건에 따라 특정 값 동기화
    return { value: nextProps.value };
  }
  return null;    // state를 변경할 필요가 없다면 null을 반환
}
```

### `componentDidMount` 메서드

```jsx
componentDidMount() {
  // 외부 라이브러리 연동: D3, masonry, etc
  // 컴포넌트에서 필요한 데이터 요청: Ajax, GraphQL, etc
  // DOM 에 관련된 작업: 스크롤 설정, 크기 읽어오기 등
}
```

컴포넌트를 만들고, 첫 렌더링을 다 마친 후 실행되는 메서드. 이 안에서 다른 자바스크립트 라이브러리 또는 프레임워크의 함수를 호출하거나 이벤트 등록, `setTimeout`, `setInterval`, 네트워크 요청 같은 비동기 작업을 처리한다.

### `shouldComponentUpdate` 메서드

```jsx
shouldComponentUpdate(nextProps, nextState) {
  // return false 하면 업데이트를 안함
  // return this.props.checked !== nextProps.checked
  return true
}
```

props 또는 state를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 메서드. 이 메서드에서는 `true` 값 또는 `false` 값을 반환해야 한다. 컴포넌트를 만들 때 이 메서드를 따로 생성하지 않으면 기본적으로 `true` 값을 반환한다. 이 메서드가 `false` 값을 반환한다면 업데이트 과정은 여기에서 중지된다.

이 메서드 안에서 현재 props와 state는 `this.props`와 `this.state`로 접근하고, 새로 설정될 props 또는 state는 `nextProps`와 `nextState`로 접근한다.

프로젝트 성능을 최적화할 때, 상황에 맞는 알고리즘을 작성하여 리렌더링을 방지할 때는 `false` 값을 반환하게 한다.

### `getSnapshotBeforeUpdate` 메서드

리액트 v16.3 이후 만든 메서드. 이 메서드는 render 메서드를 호출한 후 DOM에 변화를 반영하기 직전에 호출하는 메서드이다. 여기에서 반환하는 값은 componentDidUpdate에서 세 번째 파라미터인 `snapshot` 값으로 전달 받을 수 있다. 주로 업데이트하기 직전의 값을 참고할 일이 있을 때 활용된다(예: 스크롤바 위치).

1. render()
2. getSnapshotBeforeUpdate()
3. 실제 DOM 에 변화 발생
4. componentDidUpdate()

```jsx
getSnapshotBeforeUpdate(prevProps, prevState) {
    if (prevState.array !== this.state.array) {
        const { scrollTop, scrollHeight } = this.list;
        return { scrollTop, scrollHeight };
    }
}
```

### `componentDidUpdate` 메서드

```jsx
componentDidUpdate(prevProps, prevState, snapshot) { ... }
```

리렌더링을 완료한 후(`render()`) 실행된다. 업데이트가 끝난 직후이므로, DOM 관련 처리를 해도 무방합니다. 여기서는 `prevProps` 또는 `prevState`를 사용하여 컴포넌트가 이전에 가졌던 데이터에 접근할 수 있다. 또 `getSnapshopBeforeUpdate`에서 반환한 값이 있다면 여기에서 `snapshot` 값을 전달받을 수 있다.

### `componentWillUnmount` 메서드

```jsx
componentWillUnmount() {
  // 이벤트, setTimeout, 외부 라이브러리 인스턴스 제거
}
```

컴포넌트를 DOM에서 제거할 때 실행한다. `componentDidMount`에서 등록한 이벤트, 타이머, 직접 생성한 DOM이 있다면 여기서 제거 작업을 해야한다.

[React lifecycle methods diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

![react-lifecycle-v16.4](http://f.cl.ly/items/3W1r3K2E2i3c1M3V1C46/react-lifecycle-v16.4.png)

**LifeCycleSample Component**