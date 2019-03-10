# 04 React JS 기초 4 (작성 중)

- ref: DOM 참조하기
- React스럽게 생각하기
- 컴포넌트 스타일링

*****

## ref: DOM 참조하기

일반적인 React 데이터 플로우에서 props는 부모 컴포넌트와 자식이 상호작용할 수 있는 유일한 수단이다. 자식을 수정하려면 새로운 props와 함께 다시 렌더링해야한다. 그러나 가끔은 전형적인 데이터 플로우 밖에서 자식을 부득이하게 수정해야할 필요가 있다. 여기서 변경될 자식이란 React 컴포넌트의 인스턴스일 수도 있고, DOM 요소일 수도 있다. React는 양쪽 모두를 위한 비상구를 제공한다.

**언제 ref를 사용하는가**

* 포커스 제어, 텍스트 선택, 미디어 재생을 관리할 때
* 명령형 애니메이션을 발동시킬 때
* 서드 파티 DOM 라이브러리를 통합할 때
* state나 props로는 수행할 수 없는 DOM 제어 등을 위한 추가수단.
* 가급적 '데이터흐름'과 무관한 경우에만 활용할 것.
* stateless component에는 사용 불가.

이때는 어쩔 수 없이 DOM에 직접 접근해야 한다.

### ref 사용법

```jsx
<input ref={el => {this.input=el}} />
```

이렇게 하면 컴포넌트가 마운팅 될 때 `this.input`은 `input` 요소의 DOM을 가리킨다.

### 부모 컴포넌트에 DOM ref 노출하기

부모 컴포넌트에서 자식의 DOM 노드에 접근해야할 때가 있습니다. 캡슐화를 깨는 방식이기 때문에 추천하지 않지만 포커스를 발동시키거나 자식 DOM 노드의 포지션이나 사이즈를 계산할 때 유용하게 쓰인다.

**InputRef Component(실습)**

### 컴포넌트에 ref 달기

리액트에서는 컴포넌트에서도 ref를 달 수 있다. 이 방법은 주로 컴포넌트 내부에 있는 DOM을 컴포넌트 외부에서 사용할 때 쓴다.

```jsx
<MyComponent
    ref={ref => {this.myComponent = ref}}
/>
```

이렇게 하면 MyComponent 내부의 메서드 및 멤버 변수에도 접근할 수 있습니다. 즉, 내부의 ref에도 접근할 수 있습니다(예: `myComponent.handleClick, myComponent.input` 등).

**ComponentRef Component(실습)**