# (Todo 만들기) redux

## redux

리덕스는 리액트에서 상태를 더 효율적으로 관리하는 데 사용하는 상태관리 라이브러리이다. 리덕스는 리액트에서 상태관리를 위해 만든 상태 관리 라이브러리이지만, 리액트에 의존하지는 않는다.

- [Redux · A Predictable State Container for JS Apps](https://redux.js.org/)
- [React Redux · Official React bindings for Redux](https://react-redux.js.org/)

*****

리덕스는 효율적으로 상태 관리를 할 수 있는 라이브러리이다. 스토어에 상태 정보를 가진 객체를 넣어 두고, 액션이 디스패치되었을 때 리듀서 함수를 이용하여 상태를 변화시키고 상태가 변화될 때마다 스토어에 구독된 함수를 실행시키는 것이 주요 역할이다.

- 리덕스는 **상태 관리**의 로직을 컴포넌트 밖에서 처리한다.
- 리덕스를 사용하면 **스토어(store)**라는 객체 내부에 상태를 저장하고, 이 스토어에서 모든 상태 관리가 일어난다.
- 리덕스 스토어에 연결하는 함수를 사용하여 컴포넌트를 스토어에 **구독**시킨다.
- 상태에 어떤 변화를 일으켜야 할 때는 **액션(action)을 스토어에 전달**한다.
- 액션은 객체 형태로 되어 있으며, 상태를 변화시킬 때 이 객체를 참조하여 변화를 일으킨다.
- 액션을 전달하는 과정을 **디스패치(dispatch)**라고 한다.
- 스토어가 액션을 받으면 **리듀서(reducer)**가 전달받은 액션을 기반으로 상태를 어떻게 변경시켜야할지 정한다. 
- 액션을 처리하면 새 상태를 스토어에 저장한다.
- 스토어 안에 있는 상태가 바뀌면 스토어를 구독하고 있는 컴포넌트에 바로 전달한다.

**용어**

- **스토어(store)**: 애플리케이션의 상태 값들을 저장
- **액션(action)**: 상태 변화를 일으킬 때 참조하는 객체
- **디스패치(dispatch)**: 액션을 스토어에 전달하는 것을 의미
- **리듀서(reducer)**: 상태를 변화시키는 로직이 있는 함수
- **구독(subscribe/connect)**: 스토어(상태) 값이 필요한 컴포넌트는 스토어를 구독한다.

**TodoList App**

- App
  + Header
  + TodoList
      - TodoItem
      - TodoItem
      - ...
  + Footer

우리가 만든 TodoList는 App에서 상태를 관리하고 있기 때문에 App 컴포넌트의 state를 업데이트 하면 App 컴포넌트가 리렌더링되고, 리액트 특성상 하위 컴포넌트도 모두 리렌더링 된다.

Header의 input만 업데이트하길 원해도 TodoList와 Footer도 함께 리렌더링된다(이는 최적화 과정에서 TodoList 컴포넌트에서 `shouldComponentUpdate`를 구현하여 방지했다).

TodoList에 사용한 컴포넌트는 개수가 많지 않기 때문에 App에서 상태 관리하고 최적화하는 것이 어렵지 않았지만, 프로젝트가 복잡해지면 상황이 달라진다. 상태 관리 라이브러리를 사용하지 않고 state만 사용한다면 다음 문제가 발생할 수 있다.

- 상태 객체가 너무 복잡하고 크다.
- 최상위 컴포넌트에서 상태 관리를 하는 메서드들이 너무 많이 만들어져 코드가 복잡하다.
- 하위 컴포넌트에 props를 전달하려면 여러 컴포넌트를 거쳐야 한다.

*****

![](http://res.cloudinary.com/dyzj2erac/image/upload/c_limit/wjsojsu3qb6f1322sf5m.jpg)

*****

**Redux 사용**

- Action
    + 액션은 스토어에서 상태 변화를 일으킬 때 참조하는 객체이다.
    + 이 객체는 반드시 type 값을 가지고 있어야 한다
    + 액션 타입은 해당 액션이 어떤 작업을 하는 액션인지 정의하며, 대문자와 밑줄을 조합하여 만든다.
    + 액션에서 type 값은 고정이지만, 나머지 값들은 유동적이다.

- Reducer
    + 상태에 변화를 일으키는 함수, 리듀서
    + 리듀서는 파라미터를 두 개 받는다. 첫 번째 파라미터는 현재 상태고, 두 번째 파라미터는 액션 객체다.
    + 함수 내부에서는 switch 문을 사용하여 action.type에 따라 새로운 상태를 만들어서 반환해야 한다.
    + 리듀서가 초기에 사용할 초기 상태 initialState 값부터 먼저 설정해야 리듀서를 만들 수 있다.

*****

### js-redux-tutorial(실습)

- **불변성 관리**
- **스토어 생성**
- **구독**
- **counter**

*****

## react-redux

**프로젝트 src 폴더 세팅**

- actions: 액션 타입과 액션 생성자 파일을 저장한다.
- components: 컴포넌트의 뷰가 어떻게 생길지를 담당하는 프리젠테이셔널(presentational) 컴포넌트를 저장한다.
- containers: 스토어에 있는 상태를 props로 받아 오는 컨테이너(container) 컴포넌트들을 저장한다.
- reducers: 스토어의 기본 상태 값과 상태의 업데이트를 담당하는 리듀서 파일들을 저장한다.
- lib: 일부 컴포넌트에서 함께 사용하는 파일을 저장한다.

### 프리젠테이셔널 컴포넌트

프리젠테이셔널 컴포넌트는 뷰만 담당한다. 안에 DOM 엘리먼트와 스타일이 있으며, 프리젠테이셔널 컴포넌트나 컨테이너 컴포너넌트가 있을 수도 있다. 직접 리덕스 스토어에 접근하지 않으면, 오직 props로 데이터를 받는다. 또 대부분 state가 없거나 UI와 관련된 state만 있다.

주로 함수형 컴포넌트로 작성하며, state가 있어야 하거나 최적화를 위해 라이프사이클 메서드가 필요할 때는 클래스형 컴포넌트로 작성한다.

### 컨테이너 컴포넌트

프리젠테이셔널 컴포넌트와 컨테이너 컴포넌트의 관리를 담당한다. 내부에 DOM 엘리먼트를 직접 가지고 있을 때는 없고, 감싸는 용도로 사용한다. 상태를 가지고 있으며, 리덕스에 직접 접근한다.

프리젠테이셔널 컴포넌트와 컨테이너 컴포넌트로 나누어 컴포넌트를 작성하면, 사용자가 이용할 유저 인터페이스와 상태를 다루는 데이터가 분리되어 프로젝트를 이행하기 쉽고, 컴포넌트 재사용률이 높아진다.

| | Presentational Component | Container Component |
| ---- | ---- | ---- |
| 목적 | 어떻게 보여질 지(마크업, 스타일) | 어떻게 동작할 지(데이터 불러오기, 상태 변경하기) |
| Redux와 연관 | 아니오 | 예 |
| 데이터를 읽을 때 | props에서 데이터를 읽음 | Redux 상태 구독 |
| 데이터를 변경할 때 | props에서 콜백 호출 | Reudx 액션을 보냄 |

![react-redux-overview](./react-redux-overview.png)

*****

### CounterReduxApp(실습)

- CounterReduxApp 생성
- 액션(객체) 생성
- 리듀서 생성
    + 액션의 타입에 따라 변화를 일으키는 함수
- 스토어 생성
    + 현재 상태가 저장되어 있고, 상태를 업데이트할 때마다 구독 중인 함수들을 호출한다.
- Provider 컴포넌트로 리액트 앱에 store 연동
- CounterContainer 컴포넌트 생성
    + 컨테이너 컴포넌트는 스토어와 연동시킨다.
    + connect 함수를 사용하여 컴포넌트를 스토어에 연결시킨다.
        - mapStateToProps: `store.getState()` 결과 값인 state를 파라미터로 받아 컴포넌트의 props로 사용할 객체를 반환한다.
        - mapDispatchToProps: dispatch를 파라미터로 받아 액션을 디스패치하는 함수들을 객체 안에 넣어서 반환한다.

*****

### CounterReduxApp2(실습)

- 서브 리듀서를 생성하고, combineReducers로 루트 리듀서를 만든다.

*****

### CounterReduxApp3(실습)

- 멀티 카운터 생성
- 리덕스 개발자 도구 사용(Redux DevTools)
- 액션타입 추가
- 액션 생성함수 수정
- 리듀서 수정
- Buttons 컴포넌트 생성
- CounterList 컴포넌트 생성
- Counter 컴포넌트 수정
- CounterContainer 컴포넌트 제거, ConterListContainer 컴포넌트 생성
- App 컴포넌트 리덕스에 연결

## Ducks 파일 구조

리덕스에서는 일반적으로 액션 타입, 액션 생성 함수, 리듀서 이렇게 세 종류를 파일로 분리하여 관리한다. 이렇게 파일을 세 종류로 나누어 리덕스 관련 코드를 작성하다 보면 액션을 하나 만들 때마다 파일 세 개를 수정해야 한다. 액션 타입, 액션 생성 함수, 리듀서를 모두 한 파일에서 모듈화하여 관리하는 파일 구조가 Ducks 파일 구조다.

### Ducks 구조를 사용한 모듈 예시

```js
// 액션 타입
const CREATE = 'my-app/todos/CREATE';
const REMOVE = 'my-app/todos/REMOVE';
const TOGGLE = 'my-app/todos/TOGGLE';

// 액션 생성 함수
export const create = todo => ({
    type: CREATE,
    todo
});

export const remove = id => ({
    type: REMOVE,
    id
});

export const toggle = id => ({
    type: TOGGLE,
    id
});

const initialState = {
    // 초기상태
};

// 리듀서
export default function reducer(state = initialState, action) {
    switch (action.type) {
        // 리듀서 관련 코드
    }
};
```

Ducks 구조에서는 이처럼 파일 안에 액션 타입, 액션 생성 함수, 리듀서를 한꺼번에 넣어서 관리하는데, 이를 모듈이라고 한다.

### 규칙

Ducks 구조에서 지켜야할 규칙은 다음과 같다.

* `export default`를 이용하여 리듀서를 내보내야 한다.
* `export`를 이용하여 액션 생성 함수를 내보내야 한다.
* 액션 타입 이름은 `npm-module-or-app/reducer/ACTION_TYPE` 형식으로 만들어야 한다(라이브러리를 만들거나 애플리케이션을 여러 프로젝트로 나눈 것이 아니라면 맨 앞은 생략해도 된다. 예: `counter/INCREMENT`).
* 외부 리듀서에서 모듈의 액션 타입이 필요할 때는 액션 타입을 내보내도 된다.

*****

## redux-actions를 이용한 더 쉬운 액션 관리

redux-actions 패키지에는 리덕스 액션들을 관리할 때 유용한 `createAction` 과 `handleActions` 함수가 있다.

```shell
yarn add redux-actions
```

```js
import { createAction, handleActions } from 'redux-actions';
```

### createAction을 이용한 액션 생성 자동화

리덕스에서 액션을 만들다 보면 모든 액션에서 일일이 액션 생성자를 만드는 것이 번거로울 수 있다. 기존에 만든 `increment`와 `decrement` 코드를 살펴보자.

```js
export const increment = index => ({
    type: types.INCREMENT,
    index
});

export const decrement = index => ({
    type: types.DECREMENT,
    index
});
```

파라미터로 전달받은 값을 객체 안에 넣는 작업을 `createAction`을 사용하면 편하게 자동화할 수 있다.

```js
export const increment = createAction(types.INCREMENT);
export const decrement = createAction(types.DECREMENT);
```

`createAction` 함수는 액션 생성 함수를 간단하게 만들어 준다. 이렇게 만든 함수에 파라미터를 넣어서 호출하면 다음과 같이 payload 키에 파라미터로 받은 값을 넣어 객체를 만들어 준다.

```js
increment(3);
/* 결과:
    {
        type: 'INCEMENT',
        payload: 3
    }
*/
```

전달받은 파라미터가 여러 개일 때는 객체를 만들어 파라미터로 넣어준다.

```js
export const setColor = createAction(types.SET_COLOR);

setColor({ index: 5, color: '#fff' });
/* 결과:
    {
        type: 'SET_COLOR',
        payload: {
            index: 5,
            color: '#fff'
        }
    }
*/
```

액션이 갖고 있을 수 있는 정보의 이름을 payload 값으로 통일함으로써 액션 생성 함수를 한 줄짜리 코드로 작성했다.

어떤 파라미터를 받는지 명시하기 위해 `createAction`의 두 번째 파라미터에 payload 생성 함수를 전달하여 코드상으로 명시해 줄 수도 있다.

```js
export const setColor = createAction(
    types.SET_COLOR,
    ({ index, color }) => ({ index, color })
);
```

### switch 문 대신 handleActions 사용

리듀서에서 `switch` 문을 사용하여 액션 type에 따라 다른 작업을 하도록 했다. 이 방식에는 중요한 결점이 하나 있다. 바로 scope를 리듀서 함수로 설정했다는 것이다. 그렇기 때문에 서로 다른 `case`에서 `let`이나 `const`를 사용하여 변수를 선언하려고 할 때, 같은 이름이 중첩되어 있으면 문법 검사를 하면서 오류가 발생한다. 이 문제를 해결하는 것이 `handleActions` 이다.

```js
const reducer = handleActions({
    INCREMENT: (state, action) => ({
        counter: state.counter + action.payload
    }),
    DECREMENT: (state, action) => ({
        counter: state.counter - action.payload
    })
}, { counter: 0 });
```

첫 번째 파라미터로 액션에 따라 실행할 함수들을 가진 객체를 넣어주고, 두 번째 파라미터로는 상태의 기본 값(initialState)를 넣어준다.

*****

### CounterReduxApp4(실습)

- Ducks 구조 적용
- redux-actions 사용
- index.js modules 불러오기
- counters.js 모듈 작성
- 각 컨테이너에 bindActionCreators로 액션 생성함수 연결
- 각 컨포넌트에 액션 생성함수명 변경