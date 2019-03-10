# 03 React JS 기초 1

- JSX
- 요소 렌더링

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

**Clock Component(실습)**

### React는 필요한 것만 업데이트한다.