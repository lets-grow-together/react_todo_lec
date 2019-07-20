# (Todo 만들기) TodoList Redux 적용

## redux / redux-thunk

- input 모듈 작성
- 리듀서 내보내기
- modules/containers 폴더 생성, AppContainer 컴포넌트 생성
- App 컴포넌트 수정
- todos 모듈 작성
- insertTodo, clearInput 액션 생성자 추가
- removeTodo 액션 생성자 추가
- toggleTodo 액션 생성자 추가
- toggleAllTodo 액션 생성자 추가
- clearCompleted 액션 생성자 추가
- Status 컴포넌트 추가

```scss
// src/components/common/Status/Status.module.scss;
@import 'utils';

.status {
  position: absolute;
  overflow: hidden;
  width: 100%;
  bottom: 0;
  height: 2px;
  background: $oc-teal-0;

  &__loader {
    display: none;
    position: relative;
    width: 100%;
    height: 100%;
  
    &::before,
    &::after {
      position: absolute;
      display: block;
      height: 100%;
      background: $oc-teal-3;
      content: '';
      will-change: left right;
    }
  
    &::before {
      animation: before 2.1s cubic-bezier(0.65, 0.815, 0.735, 0.395) infinite;
    }
  
    &::after {
      animation: after 2.1s cubic-bezier(0.165, 0.84, 0.44, 1) 1.15s infinite;
    }
  }

  &__error {
    display: none;
    width: 100%;
    height: 100%;
    background: $oc-pink-5;
  }

  &.pending &__loader {
    display: block;
  }

  &.error &__error {
    display: block;
  }
}

@keyframes before {
  0% {
    left: -35%;
    right: 100%;
  }
  60% {
    left: 100%;
    right: -90%;
  }
}

@keyframes after {
  0% {
    left: -200%;
    right: 100%;
  }
  60% {
    left: 107%;
    right: -8%;
  }
}
```

- redux-thunk 브랜치 달기