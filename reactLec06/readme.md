# 04 React JS 기초 4

- state 끌어올리기
- ref: DOM 참조하기
- 컴포넌트 스타일링

*****

## state 끌어올리기

가끔 일부 컴포넌트가 동일한 변경 데이터를 보여줘야 할 필요가 있다. 이럴 때 공통 조상에 state를 끌어올리는 걸 권장한다. React에서는 공유하는 state가 필요하다면 컴포넌트의 가까운 공통 조상에 state를 옮겨서 수행한다. 이를 state 끌어올리기라고 부른다.

state를 올리는 것은 버그를 찾고 격리하는 작업이 줄어든다. 특정 컴포넌트에 모든 state가 존재하기 때문에, 버그가 나타날 수 있는 표면적이 크게 줄어든다.

**InchToCm Component(실습)**

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

### DOM에 ref 달기 / 부모 컴포넌트에 DOM ref 노출하기

부모 컴포넌트에서 자식의 DOM 노드에 접근해야할 때가 있습니다. 캡슐화를 깨는 방식이기 때문에 추천하지 않지만 포커스를 발동시키거나 자식 DOM 노드의 포지션이나 사이즈를 계산할 때 유용하게 쓰인다.

**InputRef Component(실습)**

### 컴포넌트에 ref 달기

리액트에서는 컴포넌트에서도 ref를 달 수 있다. 이 방법은 주로 컴포넌트 내부에 있는 DOM을 컴포넌트 외부에서 사용할 때 쓴다.

```jsx
<MyComponent
    ref={ref => {this.myComponent = ref}}
/>
```

이렇게 하면 MyComponent 내부의 메서드 및 멤버 변수에도 접근할 수 있다. 즉, 내부의 ref에도 접근할 수 있다(예: `myComponent.handleClick, myComponent.input` 등).

**ComponentRef Component(실습)**

*****

## 컴포넌트 스타일링

### 인라인 스타일링

```jsx
// Result style: '10px'
<div style={{ height: 10 }}>
  Hello World!
</div>

// Result style: '10%'
<div style={{ height: '10%' }}>
  Hello World!
</div>
```

```jsx
function HelloWorldComponent(props) {
    const divStyle = {
        color: 'blue',
        backgroundImage: 'url(' + props.imgUrl + ')',
    };
    
    return <div style={divStyle}>Hello World!</div>;
}
```

**Styling Component(실습)**

### css

```css
/* Apps/Styling/Styling.css */
.Styling {
  font-family: Georgia, serif;
  color: #555;
}

.Styling__content {
  position: relative;
  padding-bottom: 4px;
}

.Styling__content::after {
  position: absolute;
  left: 0;
  bottom: 0;
  right: 0;
  height: 4px;
  border-top: 1px solid #ccc;
  background: #eaeaea;
  content: ''
}

.Styling__contentHead {
  font-size: 14px;
  color: #333;
}
```

### scss

**프로젝트에 Sass 적용**

리액트 프로젝트 CRA 환경에서 Sass를 사용하려면 v2 기준 node-sass를 설치해야 한다.

```bash
npm i node-sass --save-dev
```

- sass-loader: compiles Sass to CSS, using node-sass by default
- css-loader: translate CSS into CommonJS
- style-loader: creates style nodes from JS strings

```scss
/* Apps/Styling/Styling.scss */
$font-family: Georgia, serif;
$primary-text-color: #555;
$heading-text-color: #333;

.Styling {
  font-family: $font-family;
  color: $primary-text-color;

  &__content {
    position: relative;
    padding-bottom: 4px;
  }

  &__content::after {
    position: absolute;
    left: 0;
    bottom: 0;
    right: 0;
    height: 4px;
    border-top: 1px solid #ccc;
    background: #eaeaea;
    content: '';
  }

  &__contentHead {
    font-size: 14px;
    color: $heading-text-color;
  } 
}
```

**StylingScss 컴포넌트 추가**

```scss
/* Apps/Styling/components/StylingScss.scss */
$red: red;
$orange: orange;
$yellow: yellow;
$green: green;
$blue: blue;
$navy: navy;
$purple: purple;

@mixin square($size) {
  $calculated: 15px * $size;
  width: $calculated;
  height: $calculated;
}

.StylingScss {
  display: flex;
  .box {
    cursor: pointer;
    transition: all 0.3s ease-in;
    &.red {
      background: $red;
      @include square(1);
    }
    &.orange {
      background: $orange;
      @include square(2);
    }
    &.yellow {
      background: $yellow;
      @include square(3);
    }
    &.green {
      background: $green;
      @include square(4);
    }
    &.blue {
      background: $blue;
      @include square(5);
    }
    &.navy {
      background: $navy;
      @include square(6);
    }
    &.purple {
      background: $purple;
      @include square(7);
    }
    &:hover {
      background: black;
    }
  }
}
```

**utils.scss 분리**

### CSS module

CSS module은 css를 불러올 때 [파일이름]_[클래스이름]__[해쉬] 형태로 클래스네임을 자동으로 고유한 값으로 만들어준다. CRA v2에서 [파일이름].module.css 로 파일을 저장하면 사용할 수 있다.

**CssModule 컴포넌트(실습)**

```scss
/* Apps/Styling/components/CssModule.module.css */
.box {
  width: 40px;
  height: 40px;
  background: gold
}

.desc {
  text-decoration: underline;
  color: gray;
}

.emp {
  font-weight: bold;
}

.highlight {
  text-transform: uppercase;
  color: gold;
}

:global .box {
  border: 1px solid #000;
}
```

**classNames 라이브러리**

```bash
npm i classnames --save
```

```js
classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
```

```js
import classNames from 'classnames/bind';

const styles = {
  foo: 'abc',
  bar: 'def',
  baz: 'xyz'
};

const cx = classNames.bind(styles);

const className = cx('foo', ['bar'], { baz: true }); // => "abc def xyz"
```

**classNames 라이브러리 적용**