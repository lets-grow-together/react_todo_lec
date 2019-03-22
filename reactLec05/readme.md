# 04 React JS 기초 3

- 이벤트
- 조건부 렌더링
- 리스트와 키
- 폼

*****

### 리액트의 이벤트

[SyntheticEvent – React](https://reactjs.org/docs/events.html)

**터치와 마우스 이벤트**

* onTouchStart, onTouchMove, onTouchEnd, onTouchCancel
* onClick, onDoubleClick, onMouseUp, onMouseOver
* onMouseMove, onMouseEnter, onMouseLeave, onMouseOut, onContextMenu
* onDrag, onDragEnger, onDragLeave, onDragExit, onDragStart
* onDragEnd, onDragOver, onDrop

**키보드 이벤트**

* onKeyDown, onKeyUp, onKeyPress

**포커스와 폼 이벤트**

* onFocus, onBlur
* onChange, onInput, onSubmit

**기타 이벤트**

* onScroll, onWheel, onCopy, onCut, onPaste

*****

### 리액트의 이벤트 제어하기

- 이벤트 이름은 camelCase로 작성합니다.(HTML의 onclick은 리액트에서 onClick)
- 이벤트에 실행할 자바스크립트 코드를 전달하는 것이 아니라, 함수 형태의 값을 전달한다.

```jsx
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>

// react
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

- DOM 요소에만 이벤트를 설정할 수 있다.(컴포넌트에는 이벤트를 설정할 수 없다.)
    
```jsx
<button onClick={doSomething} />
    
// prop 전달
<Button onClick={doSomething} />
```

- React를 사용할 때 일반적으로 DOM 요소가 생성된 후에 리스너를 추가하기 위해 `addEventListener` 를 호출할 필요가 없다. 요소가 처음 렌더링될 때 리스너를 제공한다.

### 리액트의 이벤트 핸들러 bind와 this

- bind
- bind in constructor
- arrow function
- transform-class-properties

[Class properties transform · Babel](https://babeljs.io/docs/plugins/transform-class-properties/)

**BindEventHandle Component(실습)**

*****

## 조건부 렌더링

React에서 원하는 동작을 수행하는 캡슐화된 별개의 컴포넌트를 생성할 수 있다. 그리고 나서, **어플리케이션의 state에 의존하여 그 중 일부만 렌더링시키는 것도 가능하다.**

React의 조건부 렌더링은 자바스크립트에서 동작하는 것과 동일하게 동작한다. if 나 조건 연산자 같은 자바스크립트 연산자를 사용하여 현재 state를 나타내는 요소를 만들고 React가 UI를 업데이트하여 일치시킨다.

**LoginControl Component(실습)**

*****

## 리스트와 키

`Array.prototype.map()`

```js
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(number => number * 2);
console..log(doubled);  // [2, 4, 6, 8, 10]
```

### 다수 컴포넌트 렌더링

리액트 요소 배열을 만들고 중괄호 `{}`를 사용하여 JSX에 포함시킬 수 있다.

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);

ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

### 키

키는 React가 어떤 아이템이 바뀌었는지, 혹은 추가되었는지, 혹은 삭제되었는지를 인식하는 데 도움을 준다. 요소에 안정적인 ID를 제공하려면 배열 내부요소에 키를 주어야 한다.

키를 선택하는 가장 좋은 방법은 리스트 아이템의 형제 중 리스트 아이템을 고유하게 식별할 수 있는 문자열을 사용하는 것이다. 대부분의 경우 데이터의 ID를 키로 사용한다.

```jsx
const todoItems = todos.map(todo => (
    <li key={todo.id}>{todo.text}</li>
));
```

만약 렌더링된 아이템에서 안정적인 id가 없다면, 아이템 인덱스를 키로 넣어 추후에 다시 정렬할 수도 있다.

```jsx
// Only do this if items have no stable IDs
const todoItems = todos.map((todo, index) => (
    <li key={index}>{todo.text}</li>
));
```

*****

## 폼

- 비제어 컴포넌트
- 제어 컴포넌트

**NameForm Component(실습) - 비제어**

**NameForm Component(실습) - 제어**

- 여러 Input 제어하기
  + 여러개의 input 요소를 제어해야할 때 각 요소에 `name` 속성을 추가한 후 `event.target.name` 값을 기반으로 핸들러 함수에서 로직을 선택할 수 있다.

**Reservation Component(실습)**