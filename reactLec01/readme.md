# 01 Intro

- [React and the Virtual DOM](https://youtu.be/muc2ZF0QIO4)
- View 영역을 컨트롤(관리)
  + 리액트는 자바스크립트 라이브러리로 유저 인터페이스를 만드는 데 사용한다. 구조가 MVC, MVW 등인 프레임워크와 달리, 오직 V(view)만 신경쓰는 라이브러리이다.
- Component 기반으로 모든 작업이 이뤄진다.
  + 리액트의 핵심 개념은 사용자 인터페이스의 생성과 유지 관리의 복잡성을 줄이는 것이다. 이를 위해 UI를 컴포넌트(재사용, 확장, 유지 관리하기 쉽고 특정 목적을 가진 독립형 구성요소)로 분리하는 개념을 받아들였다.
- Component의 state, props가 바뀔 때마다 render 메소드를 실행하여 DOM을 변경한다.

*****

## 개발환경 세팅

### git

- [Git](https://git-scm.com/)

### node / npm

- [Node.js](https://nodejs.org/ko/)

**package.json**

- 프로젝트에서 사용하고 있는 module들과 그 버전 정보를 가지고 있다.
- [[NodeJS] 모두 알지만 모두 모르는 package.json | 감성 프로그래밍](https://programmingsummaries.tistory.com/385)

```bash
npm init                # 초기화

npm i lodash            # i === install

npm i -S lodash         # -S === --save

npm i -D lodash         # -D === --save-dev

npm un lodash           # un === uninstall

npm un -S lodash
npm un -D lodash

npm i -S react react-dom
npm i -D react react-dom babel-core

npm un -S react react-dom
npm un -D react react-dom babel-core
```

### creat-react-app 설치

```bash
npm i create-react-app -g
```

*****

## React playground - Counter App 만들기

### create-react-app 을 이용하여 playground 프로젝트 생성

```bash
create-react-app std_react_playground
```

- src 폴더 index.js와 serviceWorker.js 제외하고 모두 삭제
- 관련 리소스 참조 라인 삭제 후 저장

```bash
git commit -m 'project start'
```

### CounterApp 생성

- `src/Apps/CounterApp` 폴더 생성
- `src/Apps/CounterApp/index.js` 파일 생성
- `App.js` -> `AppRoot.js`로 변경하고 CounterApp 불러오기
- `src/Apps/CounterApp/components` 폴더 생성하고 Counter 컴포넌트 작성


```css
/* src/Apps/CounterApp/components/Counter.css */
.Counter {
  width: 10rem;
  height: 10rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 1rem;
  color: white;
  font-size: 3rem;
  border-radius: 100%;
  cursor: pointer;
  background-color: black;
}
```

- `src/Apps/CounterApp/index.js` 에서 handleIncrement, handleDecrement 메서드 작성 후 컴포넌트에 전달
- `src/Apps/CounterApp/index.js` 에서 메서드 작성 마무리