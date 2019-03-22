# (Todo 만들기) 프로젝트 생성

### create-react-app 을 이용하여 프로젝트 생성

```bash
create-react-app std_reactTodo
```

```bash
git commit -m 'init'
```

## css module 및 Sass 적용

### 프로젝트 환경설정 eject

```bash
npm eject
```

```bash
git commit -m 'ejected'
```

src 폴더 index.js와 serviceWorker.js 제외하고 모두 삭제

```bash
git commit -m 'project start'
```

### sass 관련 모듈과 classnames 설치

```bash
npm i node-sass classnames --save
```

### open-color 적용

```bash
npm i open-color --save-dev
```

src/styles 폴더에 utils.scss 파일을 만들고 open-color 라이브러리를 불러온다.

```bash
@import '~open-color/open-color';
```

### 메인 스타일 설정

```scss
/* src/styles/main.scss */
@import 'utils';

body {
  background: $oc-gray-1;
  margin: 0;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

.blind {
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
.blind:before {
  display: block;
  width: 0;
  height: 0;
  font-size: 0;
  content: '\00a0';
}
```

index.js 파일에서 스타일을 불러온다. 나중에 만들 components/App.js 파일도 불러오도록 한다.

### App 컴포넌트 생성 후 webpack 개발 서버 시작

```bash
npm run start
```

*****

## UI 디자인 및 구성

### 컴포넌트 계획

- PageTemplate
- Header
- TodoList
    + TodoItem
- Footer

### PageTemplate 컴포넌트 생성

**컴포넌트 생성 흐름**

1. 폴더 만들기
2. js 파일 만들기
3. scss 파일 만들기
4. index.js 파일 만들기

src/components 폴더에 PageTemplate 폴더를 만든다. PageTemplate.js 파일을 생성하여 컴포넌트의 JSX를 작성한다.

컴포넌트를 스타일링 한다.

```scss
/* src/components/PageTemplate/PageTemplate.module.scss */
@import '../../styles/utils';

.page-template {
  min-width: 230px;
  max-width: 550px;
  margin: 0 auto;
  background: $oc-gray-0;
  color: $oc-gray-7;
}
```

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
+ @import '../../styles/utils';
- @import 'utils';
```

컴포넌트 인덱스 파일을 만든다.

PageTemplate 컴포넌트를 App 컴포넌트에서 렌더링 한다.

```bash
git commit -m 'add PageTemplate'
```

### Header 컴포넌트 생성