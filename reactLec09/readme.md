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

## 라우터

주소에 따라 다른 뷰를 보여 주는 것을 라우팅이라 한다. 리액트 자체에 이 기능이 내장되어 있지는 않지만, 라우팅 관련 라이브러리인 react-router를 설치하여 구현할 수 있다. 이 라이브러리는 클라이언트 사이드에서 진행하는 라우팅 과정을 간략하게 해 준다.

리액트 라우터를 사용하면 페이지 주소를 변경했을 때 주소에 따라 다른 컴포넌트를 렌더링해 주고, 주소 정보(파라미터, URL 쿼리 등)를 컴포넌트의 props로 전달해서 컴포넌트 단에서 주소 상태에 따라 다른 작업을 하도록 설정할 수 있다.

### react-router 설치

```shell
npm i react-router-dom --save
```

BrowserRouter는 HTML5의 history API를 사용하여 새로고침 하지 않고도 페이지 주소를 교체할 수 있게 한다.

```js
// src/Root.js
import React from 'react';
import { BrowserRouter } from 'react-router-dom';

import AppRoot from './AppRoot';

const Root = () => {
  return (
    <BrowserRouter>
      <AppRoot />
    </BrowserRouter>
  );
};

export default Root;
```

```js
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import Root from './Root';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(<Root />, document.getElementById('root'));
```

## Route와 파라미터

### Route 준비

**pages 폴더 만들고 컴포넌트 분리**

**src/pages/Basic.js**
**src/pages/Components.js**
**src/pages/Counters.js**
**src/pages/Stylings.js**
**src/pages/index.js**

### Route 설정

**AppRoot에 Route 설정**

첫 번째 라우트 Basic은 주소가 `/`와 일치할 때 보여 주도록 설정한다. `exact` 값은 주소가 여기에 설정한 `path`와 정확히 일치할 때만 보이도록 설정한다. 이 `exact` 값을 제거하면 `/components` 경로로 들어와도 `/` 경로의 내부이기 때문에 일치하는 것으로 간주하여 Basic 컴포넌트도 보인다.

## 라우트 이동

### Link 컴포넌트

Link 컴포넌트를 사용하면 페이지를 새로고침하여 불러오지 않고, 주소 창 상태를 변경하고 원하는 라우트로 화면을 전환한다.

**Menu 컴포넌트 생성**

### NavLink 컴포넌트

NavLink 컴포넌트는 Link 컴포넌트와 비슷하지만, 추가 기능이 있다. 현재 주소와 해당 컴포넌트의 목적지 주소가 일치한다면 특정 스타일 또는 클래스를 지정할 수 있다.

**NavLink 컴포넌트로 변경**

NavLink 컴포넌트를 사용하면 해당 링크를 활성화했을 때 activeStyle로 스타일을 지정할 수 있다. css 클래스를 적용하고 싶다면 activeClassName 값을 지정한다.

NavLink 컴포넌트를 사용할 때 사용한 exact 키워드는 라우트를 설정할 때와 용도가 동일하다.

### RoutesPage/RouterExample Component

**RoutesPage page 생성**
**RouteExample Component 생성**
**pages/Home, pages/About Component 생성**
**components/Menu Component 생성**

### 라우트 파라미터와 쿼리 읽기

라우트의 경로에 특정 값을 넣는 방법은 두 가지이다. 하나는 params를 사용하는 것이고, 다른 하나는 Query String을 사용하는 것이다.

### params

**src/Apps/RouteExample/index.js** routepage/about/:name 라우트를 추가

URL의 params를 지정할 때는 :key 형식으로 설정한다. 이렇게 하면 key라는 params가 생긴다.

**src/Apps/RouteExample/pages/About.js**

params 객체는 컴포넌트를 라우트로 설정했을 때 props로 전달받는 match 객체 내부에 있다. `http://localhost:3000/routepage/about/react` 주소로 들어가면 About 컴포넌트가 중복노출 된다.

/about 경로로 설정된 라우트에 `exact`를 설정하거나 `:name` 값을 선택적으로 받을 수 있게 params 뒷부분에 물음표 `?`를 입력한다.

**src/Apps/RouteExample/index.js**

파라미터가 여러 개일 때는 /about/:name/:anotherValue 처럼 입력한다.

### Query String

Query String은 URL 뒤에 `/about/something?key=value&anotherKey=value` 형식으로 들어가는 정보이다. 이 문자열로 된 쿼리를 객체 형태로 파싱하려면 `query-string` 라이브러리를 사용한다.

Query String은 App.js에서 라우트를 설정할 때 정의하지 않고, 라우트 내부에서 정의한다.

Query 내용을 받아 오려면 라우트로 설정된 컴포넌트에서 받아 오는 props 중 하나인 location 객체의 search 값을 조회해야 한다.

```shell
npm i query-string --save
```

**src/Apps/RouterExample/pages/About.js**

`http://localhost:3000/routerpage/about/react?color=red` 로 들어가면 콘솔에 `{color: red}` 가 출력된다. 이를 이용해 폰트 색상을 변경한다.

### 자바스크립트에서 라우팅

링크를 클릭하는 단순한 경우가 아니라, 자바스크립트에서 페이지를 이동해야 하는 로직을 작성해야 할 때도 있다. 예를 들어 로그인을 구현한다면 로그인이 성공했을 때 특정 경로로 이동시킬 때 사용한다.

자바스크립트로 라우팅을 하려면 라우트로 사용된 컴포넌트가 받아 오는 props 중 하나인 history 객체의 push 함수를 사용한다.

**src/Apps/RouteExample/pages/Home.js**

### 라우트 안의 라우트

Post라는 페이지 컴포넌트를 만들고 이 컴포넌트에서 `params.id` 값을 받아와서 렌더링 한다.

**src/Apps/RouteExample/pages/Post.js** 생성

Page 컴포넌트를 만든 후 이 컴포넌트를 pages 디렉터리의 index에 등록한다.

**src/Apps/RouteExample/pages/index.js**

다음으로 포스트 목록을 보여 줄 Posts 페이지 컴포넌트를 만든다. 이 컴포넌트에서 Link를 만들 때는 현재 주소 뒤에 id를 붙여서 목적지 주소를 설정한다.

그리고 Link 아래쪽에 Route를 사용하여 조건에 따라 원하는 결과를 보여주도록 설정한다.

**src/Apps/RouteExample/pages/Posts.js**

Posts 컴포넌트를 pages 폴더의 index에 등록한다.

**src/Apps/RouteExample/pages/index.js**

index.js에서 라우트를 등록하고, Menu에 링크도 추가한다.

### 라우트로 사용된 컴포넌트가 전달받는 props

- location
- match
- history

### location

location은 현재 페이지의 주소 상태를 알려준다. Post 페이지 컴포넌트에서 location을 조회하면 다음 결과가 나온다.

```js
{
    "pathname": "/routepage/posts/1",
    "search": "",
    "hash": "",
    "key": "xmsczi"
}
```

주로 search 값에서 URL Query를 읽는 데 사용하거나 주소가 바뀐 것을 감지하는 데 사용한다. 주소가 바뀐 것을 감지하려면 다음과 같이 컴포넌트 라이프사이클 메서드에서 location을 비교하면 된다.

```js
componentDidUpdate(prevProps, prevState) {
    if (prevProps.location !== this.props.location) {
        // 주소가 바뀜
    }
}
```

### match

match는 `<Route>` 컴포넌트에 설정한 path와 관련된 데이터들을 조회할 때 사용한다. 현재 URL이 같을지라도 다른 라우트에서 사용된 match는 다른 정보를 알려 준다. match 객체는 주로 params를 조회하거나 서브 라우트를 만들 때 현재 path를 참조하는 데 사용한다.

### history

history는 현재 라우터를 조작할 때 사용한다. 예를 들어 뒤쪽 페이지로 넘어가거나, 다시 앞쪽 페이지로 가거나, 새로운 주소로 이동해야 할 때 이 객체가 지닌 함수들을 호출한다.

이 객체에서 헷갈릴 수 있는 함수는 push와 replace이다. replace는 `replace('/posts')` 형식으로 작성하며, push와 차이점은 페이지 방문 기록을 남기지 않아서 페이지 이동 후 뒤로가기 버튼을 눌렀을 때 방금 전의 페이지가 아니라 전의 전 페이지가 나타난다.

action은 현재 history 상태를 알려준다. 페이지를 처음 방문했을 때는 POP이 나타나고, 링크를 통한 라우팅 또는 push를 통한 라우팅을 했을 때는 PUSH가 나타나며, replace를 통한 라우팅을 했을 때는 REPLACE가 나타난다.

block 함수는 페이지에서 벗어날 때, 사용자에게 정말 페이지를 떠나겠냐고 묻는 창을 띄운다.

```js
const unblock = history.block('정말로 떠나시겠습니까?');

// 막는 작업을 취소할 때:
unblock();
```

go, goBack, goForward는 이전 페이지 또는 다름 페이지로 이동하는 함수이다. go 함수에서는 `go(-1)`로 뒤로가기를 할 수 있고, `go(1)`로 다음으로 가기를 할 수 있다.

### withRouter로 기타 컴포넌트에서 라우터 접근

location, match, history 세 가지 props는 라우트로 사용된 컴포넌트에서만 접근할 수 있다. 라우트 내부 또는 외부 컴포넌트에서는 history, location, match 등 값을 사용할 수 없다. 예를 들어 Menu 컴포넌트는 라우트 외부에 있기 때문에 이 세 가지 props를 사용할 수 없다.

이때 withRouter를 사용하여 해당 props에 접근할 수 있다.

withRouter를 리액트 라우터에서 불러온 후, 컴포넌트를 내보낼 때 withRouter 함수로 감싸 주면 Menu 컴포넌트에서도 history 등 객체를 사용할 수 있다.

```js
import { NavLink, withRouter } from 'react-router-dom';
// (...)
export default withRouter(Menu);
```

withRouter를 사용한 컴포넌트에서 match 값은 현재 해당 컴포넌트가 위치한 상위 라우트의 정보이다. withRouter는 주로 history에 접근하여 컴포넌트에서 라우터를 조작하는 데 사용한다.

*****

### TodoList filter에 라우터 적용

**react-router** 설치

```shell
npm i react-router-dom --save
```

**Root.js**: 생성

- 라우터 추가
- Root.js/App.js 렌더링 교체

**App.js**: filterName 값 params에서 가져오기

- state에서 filterName 삭제
- handleFilterChange 제거
- match.params 객체에서 filterName 파라미터 가져오기
- Footer 컴포넌트에 전달