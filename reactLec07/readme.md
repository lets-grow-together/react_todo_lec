# (Todo 만들기) 프로젝트 생성 및 레이아웃

### create-react-app 을 이용하여 프로젝트 생성

```shell
create-react-app std_react_todo
```

```shell
git commit -m 'init'
```

## css module 및 Sass 적용

### 프로젝트 환경설정 eject

```shell
npm eject
```

```shell
git commit -m 'ejected'
```

src 폴더 index.js, App.js와 serviceWorker.js 제외하고 모두 삭제

### classnames 라이브러리 설치

```shell
npm i classnames --save
```

### node-sass 설치 및 open-color 적용

```shell
npm i node-sass open-color --save-dev
```

src/styles 폴더에 

- utils.scss
- lib/_all.scss 
- lib/_mixin.scss

파일을 만들고 유틸리티들과 open-color 라이브러리를 불러온다.

```scss
// src/styles/utils.scss
@import 'lib/all';

@mixin clearfix() {
  &::before,
  &::after {
    content: '';
    display: table;
  }
  &::after {
    clear: both;
  }
}
```

```scss
// src/styles/lib/_all.scss
@import '~open-color/open-color';
@import 'mixins';
```

```scss
// src/styles/lib/_mixins.scss
// source: https://codepen.io/dbox/pen/RawBEW
@mixin material-shadow($z-depth: 1, $strength: 1, $color: black) {
  @if $z-depth == 1 {
    box-shadow: 0 1px 3px rgba($color, $strength * 0.14), 0 1px 2px rgba($color, $strength * 0.24);
  }
  @if $z-depth == 2 {
    box-shadow: 0 3px 6px rgba($color, $strength * 0.16), 0 3px 6px rgba($color, $strength * 0.23);
  }
  @if $z-depth == 3 {
    box-shadow: 0 10px 20px rgba($color, $strength * 0.19), 0 6px 6px rgba($color, $strength * 0.23);
  }
  @if $z-depth == 4 {
    box-shadow: 0 15px 30px rgba($color, $strength * 0.25), 0 10px 10px rgba($color, $strength * 0.22);
  }
  @if $z-depth == 5 {
    box-shadow: 0 20px 40px rgba($color, $strength * 0.30), 0 15px 12px rgba($color, $strength * 0.22);
  }
  @if($z-depth < 1) or ($z-depth > 5) {
    @warn '$z-depth must be between 1 and 5';
  }
}
```

### 기본 스타일 설정

```scss
// src/styles/base.scss
@import 'utils';

body {
  background: $oc-gray-1;
  margin: 0;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

button {
  margin: 0;
  padding: 0;
  border: 0;
  background: none;
  vertical-align: baseline;
  font-family: inherit;
  font-weight: inherit;
  font-size: 100%;
  color: inherit;
  -webkit-appearance: none;
  appearance: none;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.sr-only {
  overflow: hidden;
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  border: 0;
  line-height: 0;
  white-space: normal;
  word-wrap: break-word;
  word-break: break-all;
  clip: rect(0, 0, 0, 0);
}

.sr-only:before {
  display: block;
  width: 0;
  height: 0;
  font-size: 0;
  content: '\00a0';
}
```

index.js 파일에서 스타일을 불러온다. 나중에 만들 components/App.js 파일도 불러오도록 한다.

**src/index.js** 수정

### App 컴포넌트 생성 후 webpack 개발 서버 시작

**src/components/App.js** 작성

```shell
npm run start
```

```shell
git commit -m 'project start'
```

*****

## UI 디자인 및 구성

### 컴포넌트 계획

- App
    + PageTemplate
    + Header
    + TodoList
        + TodoItem
    + Footer

### PageTemplate 컴포넌트 생성

**컴포넌트 생성 흐름**

1. 폴더 만들기
2. [component].js 파일 만들기
3. scss 파일 만들기
4. index.js 파일 만들기

src/components 폴더에 PageTemplate 폴더를 만들고. PageTemplate.js 파일을 생성하여 컴포넌트의 JS를 작성한다.

**src/components/PageTemplate/PageTemplate.js** 작성

컴포넌트를 스타일링 한다.

```scss
// src/components/PageTemplate/PageTemplate.module.scss
@import '../../styles/utils';

.page-template {
  min-width: 230px;
  max-width: 550px;
  margin: 130px auto 60px;
  background: $oc-gray-0;
  @include material-shadow(3);
  color: $oc-gray-7;
}
```

컴포넌트 인덱스 파일을 만든다.

**src/components/PageTemplate/index.js** 작성

PageTemplate 컴포넌트를 App 컴포넌트에서 렌더링 한다.

**src/components/App.js** 수정

**webpack sass-loader 설정 커스터마이징**

sytles 파일 path 설정하기

```js
// config/webpack.config.js
// common function to get style loaders
  const getStyleLoaders = (cssOptions, preProcessor) => {
    // ...
    if (preProcessor) {
      loaders.push({
        loader: require.resolve(preProcessor),
        options: {
          sourceMap: isEnvProduction && shouldUseSourceMap,
          includePaths: [paths.appSrc + '/styles'], // 추가
          //data: `@import 'utils';`,
        },
      });
    }
    // ...
```

**utils path 수정**

```diff
/* src/components/PageTemplate/PageTemplate.module.scss */
- @import '../../styles/utils';
+ @import 'utils';
```

### Header 컴포넌트 생성

**src/component/Header/Header.js** 생성

**Header.module.scss** 생성

```scss
// src/component/Header/Header.module.scss
@import 'utils';

header {
  position: relative;
}

.todo {
  &__title {
    position: absolute;
    top: -200px;
    width: 100%;
    text-align: center;
    line-height: 1.4em;
    font-size: 100px;
    font-weight: 100;
    color: $oc-grape-2;
  }

  &__new-input {
    position: relative;
    width: 100%;
    margin: 0;
    padding: 16px 16px 16px 60px;
    border: 0 none;
    background: $oc-gray-0;
    font-size: 24px;
    font-family: inherit;
    font-weight: inherit;
    line-height: 1.4em;
    color: inherit;
    box-sizing: border-box;
    box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
    &::placeholder {
      font-style: italic;
      font-weight: 300;
      color: $oc-gray-3;
    }
  }

  &__toggle-all input {
    position: absolute;
    left: 3px;
    top: 2px;
    opacity: 0;
    width: 34px;
    height: 60px;
    margin: 0;
    border: none;
    & + label {
      width: 60px;
      height: 34px;
      position: absolute;
      top: 15px;
      left: -10px;
      transform: rotate(90deg);
    }
    & + label::before {
      content: '❯';
      display: block;
      width: 100%;
      height: 100%;
      text-align: center;
      line-height: 34px;
      font-size: 22px;
      color: $oc-gray-3;
    }
    &:focus + label {
      overflow: hidden;
      outline: 5px auto rgb(0, 103, 244);
      outline-offset: -2px;
    }
    &:checked + label::before {
      color: $oc-green-4;
    }
  }
}
```

### TodoList 컴포넌트 생성

**src/components/TodoList/TodoList.js** 생성

```scss
// src/components/TodoList/TodoList.module.scss
@import 'utils';

.todo__list {
  margin: 0;
  padding: 0;
  list-style: none;
}
```

### TodoItem 컴포넌트 생성

**src/components/TodoItem/TodoItem.js** 생성

**TodoItem.module.scss** 생성

```scss
// src/components/TodoItem/TodoItem.module.scss
@import 'utils';

.todo-item {
  position: relative;
  border-bottom: 1px solid $oc-gray-3;
  font-size: 24px;

  &:last-child {
    border-bottom: 0 none;
  }

  &.is-editing {
    border-bottom: none;
    &:last-child {
      margin-bottom: -1px;
    }
  }

  &__edit {
    display: none;
    position: relative;
    width: 100%;
    border: 1px solid $oc-gray-5;
    font-size: 24px;
    font-family: inherit;
    font-weight: inherit;
    line-height: 1.4em;
    color: inherit;
    box-sizing: border-box;
    box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
  }

  &.is-editing &__edit {
    display: block;
    width: 506px;
    margin: 0 0 0 43px;
    padding: 12px 16px;
  }

  &.is-editing &__view {
    display: none;
  }

  &__toggle {
    opacity: 0;
    position: absolute;
    top: 0;
    bottom: 0;
    width: 40px;
    /* auto, since non-WebKit browsers doesn't support input styling */
    height: auto;
    margin: auto 0;
    border: none; /* Mobile Safari */
    text-align: center;
    -webkit-appearance: none;
    appearance: none;

    & + label {
      background-image: url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23ededed%22%20stroke-width%3D%223%22/%3E%3C/svg%3E');
      background-repeat: no-repeat;
      background-position: center left;
    }

    &:checked + label {
      background-image: url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23bddad5%22%20stroke-width%3D%223%22/%3E%3Cpath%20fill%3D%22%235dc2af%22%20d%3D%22M72%2025L42%2071%2027%2056l-4%204%2020%2020%2034-52z%22/%3E%3C/svg%3E');
    }
  }

  label {
    display: block;
    padding: 15px 15px 15px 60px;
    line-height: 1.2;
    transition: color 0.4s;
    word-break: break-all;
    &.is-done {
      text-decoration: line-through;
      color: $oc-gray-5;
    }
  }

  &__del {
    display: none;
    overflow: hidden;
    position: absolute;
    top: 0;
    right: 10px;
    bottom: 0;
    width: 40px;
    height: 40px;
    margin: auto 0;
    font-size: 30px;
    color: $oc-grape-2;
    transition: color 0.2s ease-out;
    &:hover {
      color: $oc-grape-4;
    }
    &:after {
      content: '×';
      display: block;
      height: 100%;
    }
  }
  
  &:hover &__del {
    display: block;
  }
}
```

### Footer 컴포넌트 생성

**src/components/Footer/Footer.js** 생성

**Footer.module.scss** 생성

```scss
// src/components/Footer/Footer.module.scss
@import 'utils';

footer {
  position: relative;
  padding: 10px 15px;
  height: 20px;
  text-align: center;
  border-top: 1px solid $oc-gray-3;
  color: #777;
  &::before {
    content: '';
    overflow: hidden;
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;
    height: 50px;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.3),
                0 8px 0 -3px #f6f6f6,
                0 9px 1px -3px rgba(0, 0, 0, 0.3),
                0 16px 0 -6px #f6f6f6,
                0 17px 2px -6px rgba(0, 0, 0, 0.3);
  }
}

.todo {
  &-count {
    float: left;
    text-align: left;
    strong {
      font-weight: 300;
    }
  }

  &-filters {
    position: absolute;
    margin: 0;
    padding: 0;
    right: 0;
    left: 0;
    list-style: none;
    li {
      display: inline;
      a {
        color: inherit;
        margin: 3px;
        padding: 3px 7px;
        text-decoration: none;
        border: 1px solid transparent;
        border-radius: 3px;
        &:hover {
          border-color: $oc-grape-2;
        }
      }
      &.is-selected a {
        border-color: $oc-grape-4;
      }
    }
  }

  &-clear {
    display: none;
    position: relative;
    float: right;
    line-height: 20px;
    text-decoration: none;
    cursor: pointer;
    &:hover {
      text-decoration: underline;
    }
    &.is-show {
      display: block;
    }
  }
}
```

### state 추가

**Header 컴포넌트 value props 추가**

**TodoList 컴포넌트 state todos mapping**

**TodoItem 컴포넌트 state display**

**레이아웃 완료**

```shell
git commit -m '레이아웃 완료'
```