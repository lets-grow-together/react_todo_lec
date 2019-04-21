# (Todo 만들기) rerendering 최적화, router

## rerendering 최적화

**리액트 개발자 도구 - Highlight Updates**

### 불필요한 리렌더링 제거하기

리액트는 부모 컴포넌트가 리렌더링되면 자식 컴포넌트도 리렌더링된다. 리액트가 어떤 DOM을 변경할지 인지하려면 우선 Virtual DOM에 모두 렌더링해 보아야 한다. 이 과정에서 Virtual DOM에 하는 불필요한 리렌더링을 shouldComponentUpdate로 방지할 수 있다. 그러면 Virtual DOM에 렌더링하는 과정에서 `render()` 함수를 실행하지 않고, 이전에 썼던 DOM정보를 그대로 사용하여 자원을 아낄 수 있다.

```js
shouldComponentUpdate(nextProps, nextState) {
    return this.props.todos !== nextProps.todos;
}
```

**shouldComponentUpdate를 사용해서 리렌더링 성능을 향상시킬 수 있는 상황**

1. 컴포넌트 배열이 렌더링되는 리스트 컴포넌트일 때
2. 리스트 컴포넌트 내부에 있는 아이템 컴포넌트일 때
3. 하위 컴포넌트 개수가 많으며, 리렌더링되지 말아야 할 상황에서도 리렌더링이 진행될 때

리스트를 렌더링할 때는 언제나 shouldComponentUpdate를 구현하는 것이 좋다.

### TodoList App 렌더링 최적화

- TodoList 컴포넌트 렌더링 최적화
- TodoItem 컴포넌트 렌더링 최적화
- Footer 컴포넌트 렌더링 최적화

```shell
git commit -m 'render 최적화 완료'
```

*****

## router