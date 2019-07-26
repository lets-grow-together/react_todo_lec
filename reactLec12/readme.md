# (Todo 만들기) Redux middleware

웹 애플리케이션을 만들 때는 대부분 서버와 데이터를 연동해야 한다. 데이터를 연동하려면 일반적으로 서버에 구현된 REST API에 Ajax를 요청하여 데이터를 가져오거나 입력해야 한다. 서버의 API를 호출할 때는 총 세가지 상태를 관리해야 한다.

- 요청했을 때: **로딩**
- 응답했을 때
    + **성공**
    + **실패**

서버에 요청했을 때 서버가 응답할 때까지 로딩 상태를 설정해야 하며, 해당 요청이 성공했을 때와 실패했을 때 상태를 어떻게 업데이트할지 결정해야 한다.

이런 작업은 리액트 컴포넌트의 state만 사용해도 관리할 수 있다. 하지만 리덕스와 리덕스 미들웨어를 사용하여 상태를 관리한다면 작업이 훨씬 간편해진다.

리덕스 미들웨어 개념을 이해하고, 미들웨어를 사용하여 비동기 작업을 처리하는 방법을 알아본다.

*****

## 미들웨어의 이해

### 미들웨어란?

리덕스 미들웨어(middleware)는 액션을 디스패치했을 때 리듀서에서 이를 처리하기 전에 사전에 지정된 작업들을 실행한다. 미들웨어는 액션과 리듀서 사이의 중간자라고 볼 수 있다. 리듀서가 액션을 처리하기 전에 미들웨어가 여러가지 작업을 할 수 있다.

```
액션 -> (미들웨어) -> 리듀서 -> 스토어
```

*****

### CounterReduxMiddleware(실습)

- store.js 분리
- redux-logger 적용

```shell
npm i redux-logger --save
```

- CounterReduxMiddleware 수정
    + CounterListContainer.js 삭제
    + CounterList.js 삭제
    + Counter.js 삭제
    + Buttons.js 삭제
    + getRandomColor.js 삭제
    + App.js 수정

*****

### CounterReduxThunk(실습)

- redux-thunk 설치

```shell
npm i redux-thunk --save
```

- 스토어를 생성할 때 redux-thunk 미들웨어 적용
- 카운터를 비동기적으로 생성
- 웹 요청 처리

```shell
npm i axios --save
```

- redux-thunk와 axios 사용
- post.js 모듈 생성
- 루트 리듀서 수정, counter 모듈 initial 값 수정
- App.js에서 액션으로 웹 요청 시도

*****

### CounterReduxPromiseMiddleware(실습)

redux-promise-middleware는 Promise 기반의 비동기 작업을 좀 더 편하게 해 주는 미들웨어이다. 이 미들웨어는 Promise 객체를 payload로 전달하여 요청을 시작, 성공, 실패할 때 액션의 뒷부분에 `_PENDING`, `_FULFILLED`, `_REJECTED`를 붙여서 반환한다. 

```shell
npm i redux-promise-middleware --save
```

- store에 redux-promise-middleware 적용
- post 모듈 액션 생성자 수정