# (Todo 만들기) ajax, immutable

## ajax

### Firebase를 사용하여 웹 요청 처리

[Firebase | 공식 사이트](https://firebase.google.com/)
[REST API 설치 및 설정  |  Firebase](https://firebase.google.com/docs/database/rest/start?hl=ko)

- 시작하기
- 프로젝트 추가
  + 프로젝트 이름
  + 국가/지역
  + 프로젝트 만들기
  + 웹 앱에 Firebase 추가 - config
- 개발 > Authentication
  + 로그인 방법 설정
  + 구글 로그인
    - 사용설정
    - 개발 > Database
  + Realtime Database
  + 잠금모드로 시작
  + 규칙 탭 수정
  
  ```js
  // 일반적인 규칙 세팅
  {
    "rules": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
  
  // for dev
  {
    "rules": {
      ".read": true,
      ".write": true
    }
  }
  ```

### Firebase CLI 설치

```shell
npm i firebase-tools -g
```

- Firebase CLI 로그인

```shell
firebase login

# 모든 Firebase 프로젝트의 목록을 출력합니다.
firebase list
```

[Firebase CLI 참고 | Firebase](https://firebase.google.com/docs/cli/?hl=ko)

### Firebase 초기화

```shell
cd [project folder]
firebase init
```

- Databse 선택 (space)
- Hosting 선택
- don't setup a default project
- Database Rules: database.rules.json
- Hosting directory: build
- spa: y
- rewrite: n
- firebase use --add
  + select project / staging|dev

배포하기

```shell
firebase serve|deploy
```

### 웹 요청 처리

**axios 라이브러리 설치**

```shell
npm i axios --save
```

**lib/api.js**에 axios 불러오고 api 요청 함수 `fetchTodos` 작성

**App 컴포넌트의 componentDidMount에서 api 요청**

**추가 api 요청 작성**

*****

## immutable

### immer.js를 이용하여 불변성 관리

```shell
npm i immer --save
```

**example**

```js
import produce from "immer"

// object mutations
const todosObj = {
    id1: {done: false, body: "Take out the trash"},
    id2: {done: false, body: "Check Email"}
}

// add
const addedTodosObj = produce(todosObj, draft => {
    draft["id3"] = {done: false, body: "Buy bananas"}
})

// delete
const deletedTodosObj = produce(todosObj, draft => {
    delete draft["id1"]
})

// update
const updatedTodosObj = produce(todosObj, draft => {
    draft["id1"].done = true
})

// array mutations
const todosArray = [
    {id: "id1", done: false, body: "Take out the trash"},
    {id: "id2", done: false, body: "Check Email"}
]

// add
const addedTodosArray = produce(todosArray, draft => {
    draft.push({id: "id3", done: false, body: "Buy bananas"})
})

// delete
const deletedTodosArray = produce(todosArray, draft => {
    draft.splice(draft.findIndex(todo => todo.id === "id1"), 1)
    // or (slower):
    // return draft.filter(todo => todo.id !== "id1")
})

// update
const updatedTodosArray = produce(todosArray, draft => {
    draft[draft.findIndex(todo => todo.id === "id1")].done = true
})
```

**immer 적용**

**react build**

```shell
npm run build
```

**firebase deploy**

```shell
firebase deploy
```