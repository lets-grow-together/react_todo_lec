# 02 ECMA Script 2015(ES6) 기초

- 블록 스코프
  + let / const
- Template literals
- Function
  + Default Parameter
  + Rest Parameter
- Operator
  + spread operator
- Arrow function
- Enhanced Object
  + Shorthand Property initializer
  + Concise methods
  + Computed property key
- Destructuring Assignment
- class
- modules

*****

## 블록 스코프

- ECMAScript 6에서는 변수 생명 주기를 개발자가 더 잘 제어하도록 하기 위해 블록 레벨 스코프 옵션이 도입되었다.
- 기존의 함수에 의한 스코프처럼 `{ }`으로 감싼 내부에 별도의 스코프가 생성된다.

```js
function getValue(condition) {
    if (condition) {
        var value = 'blue';
        // ...
        return value;
    } else {
        // value는 여기서 undefined로 존재한다.
        return null;
    }
    // value는 여기서 undefined로 존재한다.
}

getValue(false);
```

### let 선언

```js
function getValue(condition) {
    if (condition) {
        let value = 'blue';
        // ...
        return value;
    } else {
        // value는 여기서 존재하지 않음
        return null;
    }
    // value는 여기서 존재하지 않음
}

getValue(false);
```

변수 value는 `var` 대신 `let`으로 선언했기 때문에, value의 선언이 함수 맨 위로 호이스팅되지 않고, `if` 블록 바깥에서 접근할 수 없다. 만약 condition의 값이 `false`이면 value는 선언되거나 초기화되지 않는다.

### 재정의 금지

식별자가 특정 스코프 안에 선언되어 있을 경우, 스코프 안에서 let 선언으로 식별자를 사용하면 에러가 발생한다.

```js
var count = 30;

// 문법 에러 발생
let count = 40;
```

`let`은 같은 스코프 안에 존재하는 식별자를 재정의할 수 없으므로 에러가 발생한다. 반대로, `let` 선언을 사용하여 기존 변수와 같은 이름으로 새 변수를 블록 스코프 내부에 만든다면 에러가 발생하지 않는다.

```js
var count = 30;

// 에러가 발생하지 않음
if (condition) {
    let count = 40;
    // ...
}
```

이 `let` 선언은 블록을 둘러싼 스코프에 count를 만드는 대신, `if`문 안에 새로운 변수 count를 만들기 때문에 에러를 발생시키지 않는다.

### const 선언

`const`를 사용하여 선언된 바인딩은 상수(constants)로 간주되며, 설정되면 변경할 수 없다. 그러므로 모든 `const` 바인딩은 선언할 때 초기화해야 한다.

```js
// 유효한 상수
const maxItems = 30;

// 문법 에러: 초기화되지 않음
const name;
```

`let`과 마찬가지로 상수도 블록-레벨 선언이다. 이는 상수 역시 선언된 블록 바깥에서는 더 이상 접근할 수 없으며, 호이스팅되지 않는다.

```js
if (condition) {
    const maxItems = 5;
    // ...
}

// maxItems는 여기서 접근 불가
```

`const` 선언을 사용할 때도 같은 스코프 내에 같은 이름의 변수가 이미 선언되어 있으면 에러가 발생한다.

```js
var message = 'Hello!';
let age = 25;

// 다음 선언들은 각각 에러가 발생한다.
const message = 'Goodbye!';
const age = 30;
```

유사한 점이 있지만, `let`과 `const` 사이에는 중요한 차이가 있다. 이미 `const`를 사용해 정의한 상수에 새 값을 할당하려 하면 strict와 non-strict 모드에서 에러가 발생한다.

```js
const maxItems = 5;
maxItems = 6;       // 에러 발생
```

다른 언어의 상수와 마찬가지로 이미 선언한 maxItems 변수에는 새로운 값을 할당할 수 없다. 그러나 다른 언어와 달리 **상수로 선언한 값이 객체라면, 상수 객체가 소유한 값을 수정할 수 있다.**

### const로 객체 선언하기

`const` 선언은 바인딩을 변경하지 못하도록 하는 것이지, 바인딩 된 값의 변경을 막는 것은 아니다. 즉, 객체를 `const`로 선언해도 객체가 가진 값은 수정할 수 있다.

```js
const person = {
    name: 'Chris'
};

// 문제 없이 동작
person.name = 'Choi';

// 에러 발생
person = {
    name: 'Michael'
};
```

`const`는 바인딩된 값의 수정을 막는 것이 아니라, 바인딩의 수정을 방지한다.

### 반복문 안에서의 블록 바인딩

```js
for (var i = 0; i < 10; i++) {
    process(items[i]);
}

// 여기서도 여전히 i에 접근할 수 있다.
console.log(i);             // 10
```

블록 레벨 스코프가 기본인 언어에서는 `for` 반복문에서만 `i` 변수에 접근할 수 있다. 그러나 자바스크립트에서는 `var` 선언이 호이스팅되기 때문에 반복문이 완료된 이후에도 여전히 `i` 변수에 접근할 수 있다.

```js
for (let i = 0; i < 10; i++) {
    process(items[i]);
}

// 여기서는 i에 접근할 수 없다 - 에러 발생
console.log(i);
```

### 블록 바인딩을 위한 모범 사례

**`const`를 기본으로 사용하고 변수 값을 변경해야 할 때만 `let`을 사용한다.** 예상치 못한 값 변경은 버그의 원인이 될 수 있기 때문에, 변수는 초기화 후에 대부분 그 값을 변경해서는 안된다.

*****

## Template literals

템플릿 리터럴은 ECMAScript 5와 이전 버전에서 부족했던 자바스크립트의 다음 특징에 대한 ECMAScript 6의 해결책이다.

- Multiline strings: 멀티라인 문자열의 공식적인 개념
- Basic string formatting: 변수 값을 문자열 일부분으로 대체하는 능력
- HTML escaping: HTML 안에 안전하게 삽입될 수 있도록 문자열을 변경하는 능력

템플릿 리터럴은 큰따옴표나 작은따옴표 대신에 백틱으로 구분한 일반적인 문자열처럼 동작한다.

### 멀티라인 문자열

ECMAScript 6의 템플릿 리터럴은 특별한 문법 없이 멀티라인 문자열을 쉽게 만들어 준다. 원하는 위치에 줄바꿈을 하면 다음과 같이 바로 결과에 나타난다.

```js
let message = `Multiline
string`;

console.log(message);   // Multiline
                        // string
console.log(message.length);    // 16
```

### 치환자 만들기

치환자는 내부에 어떤 자바스크립트 표현식이든 가질 수 있고 `${`로 열고 `}`로 닫음으로써 구분된다.

```js
let name = 'Chris',
    message = 'Hello, ${name}.';

console.log(message);       // 'Hello, Chris.'
```

모든 치환자는 자바스크립트 표현식이기 때문에 간단한 변수 이름 외에도 다양한 것을 치환할 수 있다. 다음 예제처럼 계산식, 함수 호출 등도 쉽게 넣을 수 있다.

```js
let count = 10,
    price = 0.25,
    message = `${count} items cost $${(count * price).toFixed(2)}.`;
    
console.log(message);       // '10 items cost $2.50.'
```

*****

### Function - Default Parameter(매개변수 기본값)

```js
// ES5
function makeRequest(url, timeout, callback) {
    // timeout = timeout || 2000;
    timeout = (typeof timeout !== 'undefined') ? timeout : 2000;
    callback = (typeof callback !== 'undefined') ? callback : function () {};
    
    // 함수의 나머지 부분
}
```

```js
// ES6
function makeRequest(url, timeout = 2000, callback = function () {}) {
    // 함수의 나머지 부분
}
```

### Function - Rest Parameter(나머지 매개변수)

```js
function func(arg) {
    const restArgs = Array.prototype.slice.call(arguments, 1);
    console.log(arg);   // 1

    if (restArgs.length) {
      console.log(restArgs);    // [2, 3]
    }
}

func(1, 2, 3);
```

```js
function func(arg, ...restArgs) {
    console.log(arg);   // 1

    if (restArgs.length) {
      console.log(restArgs);    // [2, 3]
    }
}

func(1, 2, 3);
```

나머지 매개변수는 반드시 하나여야 하며, 마지막 위치에만 사용할 수 있다.

```js
function pick(object, ...keys, last) {
    // ...
}
// SyntaxError
```

*****

### Operator - Spread operator(전개 연산자)

```js
let values = [25, 50, 75, 100];
console.log(...values); // 25 50 75 100
```

전개 연산자(`...` spread operator)는 나머지 매개변수와 밀접하게 연관되어 있다. 나머지 매개변수가 여러 개의 독립적인 인자를 배열로 합쳐주는 반면, 전개 연산자는 배열을 나누어 함수에 개별적인 인자로 전달한다.

```js
let value1 = 25;
let value2 = 50;

console.log(Math.max(value1, value2));      // 50
```

배열에서 가장 큰 값을 찾고자 할 때 ECMAScript 5까지는 `Math.max()` 메서드에 배열을 전달할 수 없으므로, 배열을 수동으로 변환하거나 `apply()`를 사용해야 했다.

```js
let values = [25, 50, 75, 100];

console.log(Math.max.apply(Math, values));  // 100
```

ECMAScript 6 전개 연산자는 `apply()`를 호출하는 대신, 나머지 매개변수에 사용했던 `...`를 `Math.max()`에 전달하려는 배열 앞에 붙여 바로 전달할 수 있다.

```js
let values = [25, 50, 75, 100];

// console.log(Math.max(25, 50, 75, 100)); 과 같다
console.log(Math.max(...values));
```

그리고 전개 연산자는 다른 인자와 함께 사용할 수 있다. 다음 예제처럼 별도로 인자를 전달할 수 있으며 다른 인자에 전개 연산자를 사용할 수도 있다.

```js
let values = [-25, -50, -75, -100];

console.log(Math.max(...values, 0));        // 0
```

**전개 연산자를 이용하여 배열/객체(stage-2 proposal) 복사(immutable)**

```js
const origin = [1, 2];
const copied = [...origin];
// const [...copied] = origin

origin.push(3);
console.log(origin);    // [1, 2, 3]
console.log(copied);    // [1, 2]
```

```js
const origin = { a: 'aa', b: 'bb' };
const copied = { ...origin };
//const {...copied} = origin;

origin.a = 'aaaa';
console.log(origin);    // { a: 'aaaa', b: 'bb' }
console.log(copied);    // { a: 'aa', b: 'bb' }
```

```js
// Object.assign(ES6)
const origin = { a: 'aa', b: 'bb' };
const nextOrigin = { ...origin, ...{ a: 'aaaa' } };
//const nextOrigin = Object.assign({}, origin, { a: 'aaaa' });
console.log(nextOrigin);    // { a: 'aaaa', b: 'bb' }
```

*****

## Arrow function(화살표 함수)

화살표 함수는 중요한 몇 가지 부분에서 기존의 자바스크립트 함수와 다르게 동작한다.

- `this`와 `arguments` 바인딩: 함수 내에서 `this`와 `arguments`의 값은 그 화살표 함수를 가장 근접하게 둘러싸고 있는 일반함수에 의해 정의된다.
- **`new`를 사용할 수 없음**: 화살표 함수는 `[[Construct]]` 메서드가 없으므로 생성자로 사용될 수 없다. 화살표 함수는 `new`와 함께 사용하면 에러를 발생시킨다.
- **프로토타입이 없음**: 화살표 함수에 `new`를 사용할 수 없기 때문에 프로토타입이 필요없다. 화살표 함수에는 `prototype` 프로퍼티가 없다.
- **`this`를 변경할 수 없음**: 함수 내부의 `this`는 변경할 수 없다. 함수의 전체 생명주기 내내 같은 값으로 유지된다.
- **`arguments` 객체 없음**: 화살표 함수는 `arguments` 바인딩이 없기 때문에 함수 인자에 접근하기 위해서는 명시한 매개변수와 나머지 매개변수에 의존해야 한다.

### 화살표 함수 문법

```js
var reflect = value => value;

// 같은 코드
var reflect = function (value) {
    return value;
}
```

만약 하나 이상의 인자를 전달하려면 인자를 괄호로 감싸야 한다.

```js
let sum = (num1, num2) => num1 + num2;

// 같은 코드
let sum = function (num1, num2) {
    return num1 + num2;
}
```

만약 함수에 인자가 없다면 선언에 빈 괄호를 사용한다

```js
let getName = () => 'Chris';

// 같은 코드
let getName = function () {
    return 'Chris';
}
```

아무것도 하지 않는 함수를 만들고 싶다면 중괄호를 사용한다.

```js
let doNothing = () => {};

// 같은 코드
let doNothing = function () {};
```

객체 리터럴을 반환하는 화살표 함수는 괄호로 객체 리터럴을 감싸야 한다.

```js
let getTempItem = id => ({ id: id, name: 'Temp' });

// 같은 코드
let getTempItem = function (id) {
    return {
        id: id,
        name: 'Temp'
    };
};
```

### 화살표 함수와 배열

화살표 함수의 간결한 문법은 배열을 가공하는 데도 적합하다.

```js
var result = values.sort(function (a, b) {
    return a - b;
});

var result = values.sort((a, b) => a - b);
```

`sort()`와 `map()`, `reduce()` 같은 콜백 함수를 받는 배열 메서드에서는 화살표 함수 문법을 사용하면, 대개 외관상 복잡한 처리를 더 단순한 코드로 변경할 수 있다.

*****

## Enhanced Object

### Shorthand Property initializer(프로퍼티 초기자 축약)

객체 프로퍼티 이름이 지역 변수 이름과 같으면, 콜론(:)과 값을 명시하지 않고 간단하게 이름만 사용할 수 있다.

```js
function createPerson(name, age) {
    return {
        name: name,
        age: age
    };
}
```

```js
// 축약
function createPerson(name, age) {
    return {
        name,
        age
    };
}
```

### Concise methods(간결한 메서드)

ECMAScript 6에서는 콜론과 function 키워드가 제거되어 문법이 좀더 간결해졌다.

```js
var person = {
    name: 'Chris',
    sayName: function () {
        console.log(this.name);
    }
};
```

```js
// 축약
var person = {
    name: 'Chris',
    sayName() {
        console.log(this.name);
    }
};
```

### Computed property key(계산된 프로퍼티 이름)

ECMAScript 5까지는 점 표기법이 아닌 대괄호 표기법을 사용할 때만 객체 인스턴스의 프로퍼티 이름을 계산할 수 있었다.

```js
var person = {};
var lastName = 'last name';

person['first name'] = 'Chris';
person[lastName] = 'Choi';

console.log(person['first name']);
console.log(person[lastName]);
```

추가로, 객체 리터럴 안에 프로퍼티 이름으로 문자열 리터럴로 바로 사용할 수 있다.

```js
var person = {
    'first name': 'Chris'
};

console.log(person['first name']);
```

이 방식은 미리 정의되어 있고 문자열 리터럴로 표현될 수 있는 프로퍼티 이름의 경우에는 잘 동작한다. 그러나 만약 프로퍼티 이름 'first name'이 변수에 할당되어 있거나 계산되어야 한다면 ECMAScript 5에서 기존의 객체 리터럴을 사용하여 정의하는 방법은 없다.

ECMAScript 6에서, 계산된 프로퍼티 이름은 객체 리터럴 문법의 일부이며 대괄호 표기법을 사용하여 객체 인스턴스에 계산된 프로퍼티 이름을 참조할 수 있다.

```js
let lastName = 'last name';

let person = {
    'first name': 'Chris',
    [lastName]: 'Choi'
};

console.log(person['first name']);
console.log(person[lastName]);
```

객체 리터럴 안에 대괄호는 프로퍼티 이름을 계산한다는 것을 나타내며, 대괄호의 내용은 문자열로 평가된다. 또한 표현식을 포함할 수 있다.

```js
var suffix = ' name';
var person = {
    ['first' + suffix]: 'Chris',
    ['last' + suffix]: 'Choi'
};

console.log(person['first name']);
console.log(person['last name']);
```

*****

## Destructuring Assignment(구조분해 할당)

ECMAScript 5까지는 객체와 배열로부터 정보를 가져오거나 지역 변수에 특정 데이터를 가져오는 경우, 코드를 중복해서 작성했다.

```js
let options = {
    repeat: true,
    save: false
};

// 객체로부터 데이터를 추출
let repeat = options.repeat;
let save = options.save;
```

### 객체 구조분해

객체 구조분해 문법은 할당 연산자 왼쪽에 객체 리터럴을 사용한다.

```js
let node = {
    type: 'Identifier',
    name: 'foo'
};

let { type, name } = node;

console.log(type);      // Identifier
console.log(name);      // foo
```

구조분해 할당문을 사용하고 객체 안에 존재하지 않는 프로퍼티 이름으로 지역변수를 명시하면, 그 지역 변수에는 undefined 값이 할당된다.

```js
let node = {
    type: 'Identifier',
    name: 'foo'
};

let { type, name, value } = node;

console.log(type);      // Identifier
console.log(name);      // foo
console.log(value);     // undefined
```

명시된 프로퍼티가 없을 때 기본값을 선택적으로 정의할 수 있다.

```js
let node = {
    type: 'Identifier',
    name: 'foo'
};

let { type, name, value = true } = node;

console.log(type);      // Identifier
console.log(name);      // foo
console.log(value);     // true
```

이 예제에서는 value는 `true` 값을 기본값으로 가진다. 이 기본값은 node 객체에 해당 프로퍼티가 존재하지 않거나, 해당 프로퍼티의 값이 `undefined`일 때만 사용된다.

### 이름이 다른 지역 변수에 할당하기

ECMAScript 6에는 객체 프로퍼티를 그와 다른 이름의 지역 변수에 할당할 수 있는 확장 문법이 있는데, 이것은 객체 리터럴의 축약되지 않은 프로퍼티 초기자 문법과 유사해 보인다.

```js
let node = {
    type: 'Identifier',
    name: 'foo'
};

let { type: localType, name: localName } = node;

console.log(localType);     // Identifier
console.log(localName);     // foo
```

### 중첩된 객체 구조분해

```js
let node = {
    type: 'Identifier',
    name: 'foo',
    loc: {
        start: {
            line: 1,
            column: 1
        },
        end: {
            line: 1,
            column: 4
        }
    }
};

let { loc: { start }} = node;

console.log(start.line);        // 1
console.log(start.column);      // 1
```

### 배열 구조분해

```js
let colors = ['red', 'green', 'blue'];

let [ firstColor, secondColor ] = colors;

console.log(firstColor);        // red
console.log(secondColor);       // green
```

```js
let colors = ['red', 'green', 'blue'];
let [ , , thirdColor] = colors;

console.log(thirdColor);        // blue
```

**변수 값 교환하기**

```js
// ECMAScript 5에서 값 교환하기
let a = 1;
let b = 2;
let tmp;

tmp = a;
a = b;
b = tmp;

console.log(a);     // 2
console.log(b);     // 1

// ECMAScript 6에서 변수 교환하기
let a = 1;
let b = 2;

[a, b] = [b, a];    // [b, a] 는 임시배열

console.log(a);     // 2
console.log(b);     // 1
```

### 중첩된 배열 구조분해

```js
let colors = ['red', ['green', 'lightgreen'], 'blue'];

// later
let [firstColor, [secondColor]] = colors;

console.log(firstColor);        // red
console.log(secondColor);       // green
```

### 나머지 요소

함수의 나머지 매개변수처럼 배열 구조분해에도 나머지 요소(rest items)로 불리는 유사한 개념이 있다.

```js
let colors = ['red', 'green', 'blue'];

let [firstColor, ...restColors] = colors;

console.log(firstColor);            // red
console.log(restColors.length);     // 2
console.log(restColors[0]);         // green
console.log(restColors[1]);         // blue
```

### 구조분해된 매개변수

함수에 인자를 전달하는 경우에도 구조분해를 유용하게 사용할 수 있다.

```js
// ES5
// 부가적인 매개변수를 표현하는 options의 프로퍼티
function secCookie(name, value, options) {
    options = options || {};
    
    let secure = options.secure;
    let path = options.path;
    let domain = options.domain;
    let expires = options.expire;
    
    // cookie를 설정하는 코드
}

// 세 번재 인자는 options에 전달됨
setCookie('type', 'js', {
    secure: true,
    expires: 60000
});
```

예제 함수에서 name, value는 필수지만, secure와 path, domian, expires는 선택적이다. 선택적인 인자는 우선순위가 없으므로 매개변수를 나열하는 것 보다 프로퍼티로 구성된 options 객체를 두는 것이 효율적이다. 이런 접근법은 함수 정의를 통해서 어떤 값이 입력될지 예상할 수 없으므로 함수 본문을 살펴봐야 한다.

구조분해된 매개변수를 사용하면 함수에 어떤 값이 전달될지 예상할 수 있다.

```js
// ES6
function setCookie(name, value, { secure, path, domain, expires } = {}) {
    // cookie를 설정하는 코드
}

setCookie('type', 'js', {
    secure: true,
    expires: 60000
});
```

```js
// 구조분해된 매개변수의 기본값
function setCookie(name, value, {
    secure = false,
    path = '/',
    domain = 'example.com',
    expires = new Date(Date.now() + 360000000)
} = {}) {
    // ...
}
```

*****

## class

### ECMAScript 5의 유사 클래스 구조

```js
function PersonType(name) {
    this.name = name;
}

PersonType.prototype.sayName = function () {
    console.log(this.name);
}

let person = new PersonType('Chris');
person.sayName();       // Chris 출력

console.log(person instanceof PersonType);      // true
console.log(person instanceof Object);          // true
```

### 클래스 선언

```js
class PersonClass {
    // PersonType 생성자에 해당하는 부분
    constructor(name) {
        this.name = name;
    }
    
    // PersonType.prototype.sayName에 해당하는 부분
    sayName() {
        console.log(this.name);
    }
}

let person = new PersonClass('Chris');
person.sayName();       // Chris 출력

console.log(person instanceof PersonClass);     // true
console.log(person instanceof Object);          // true

console.log(typeof PersonClass);                    // function
console.log(typeof PersonClass.prototype.sayName);  // function
```

### 차이점

- 함수 선언과 달리 클래스 선언은 호이스팅되지 않는다.
- 클래스 선언 내의 모든 코드는 자동으로 strict 모드에서 실행된다. 클래스 내에서 strict 모드를 피할 방법은 없다.
- 모든 메서드는 열거할 수 없다. 이는 사용자 정의 타입과 다른 중요 변경사항으로, 사용자 정의 타입에서는 메서드를 열거 불가능하도록 만들기 위해 Object.defineProperty()를 사용해야 한다.
- new 없이 클래스 생성자를 호출하면 에러가 발생한다.
- 클래스 메서드 내에서 클래스 이름을 덮어쓰려 하면 에러가 발생한다.

### 클래스 표현식

```js
let PersonClass = class {
    // PersonType 생성자와 같음
    constructor(name) {
        this.name = name;
    }
    
    // PersonType.prototype.sayName과 같은
    sayName() {
        console.log(this.name);
    }
};
```

### 정적 메서드

```js
// ES5
function PersonType(name) {
    this.name = name;
}

// 정적 메서드
PersonType.create = function (name) {
    return new PersonType(name);
};

// 인스턴스 메서드
PersonType.prototype.sayName = function () {
    console.log(this.name);
};

var person = PersonType.create('Chris');
```

```js
// ES6
class PersonClass {
    // PersonType 생성자와 같음
    constructor(name) {
        this.name = name;
    }
    
    // PersonType.prototype.sayName와 같음
    sayName() {
        console.log(this.name);
    }
    
    // PersonType.create와 같음
    static create(name) {
        return new PersonClass(name);
    }
}

let person = PersonClass.create('Chris');
```

### 상속

```js
// ES5 클래스 상속
function Rectangle(length, width) {
    this.length = length;
    this.width = width;
}

Rectangle.prototype.getArea = function () {
    return this.length * this.width;
};

function Square(length) {
    Rectangle.call(this, length, length);
}

Square.prototype = Object.create(Rectangle.prototype, {
    constructor: {
        value: Square,
        enumerable: true,
        writable: true,
        configurable: true
    }
});

var square = new Square(3);

console.log(square.getArea());              // 9
console.log(square instanceof Square);      // true
console.log(square instanceof Rectangle);   // true
```

```js
// ECMAScript 6 클래스 상속
class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }
    
    getArea() {
        return this.length * this.width;
    }
}

class Square extends Rectangle {
    constructor(length) {
        // Rectangle.call(this, length, length)와 같음
        super(length, length);
    }
}

var square = new Square(3);

console.log(square.getArea());                  // 9
console.log(square instanceof Square);          // true
console.log(square instanceof Rectangle);       // true
```

Square 클래스는 `extends` 키워드를 사용하여 Rectangle을 상속한다. Square 생성자는 지정된 인자와 함께 Rectangle 생성자를 호출하기 위하여 `super()`를 사용한다.

다른 클래스를 상속한 클래스는 파생 클래스(derived classed)로 불린다. 파생 클래스에서 생성자를 명시하려면 반드시 `super()`를 사용해야만 하고, 그렇게 하지 않으면 에러가 발생한다. 클래스 선언에서 생성자를 사용하지 않는 경우, 클래스의 새 인스턴스를 만들 때 전달된 모든 인자와 함께 `super()`가 자동으로 호출된다. 예를 들면, 다음 예제의 두 클래스는 같다.

```js
class Square extends Rectangle {
    // 생성자 없음
}

// 위와 같음

class Square extends Rectangle {
    constructor(...args) {
        super(...args);
    }
}
```

*****


## 모듈

### 모듈화 장점

- 코드를 여러 모듈로 분리하여 깔끔하게 구획하고 조직화 할 수 있다.
- 전역 스코프를 통해 인터페이스 하지 않고 모듈 각자 자신의 스코프를 가지므로 전역 변수 사용을 줄일 수 있고 그로 인한 문제점을 예방할 수 있다.
- 코드 재사용성이 향상된다.
- 특정 모듈에 버그가 한정되어 디버깅이 쉬워진다.

### 모듈구현 - IIFE

```js
// math.js
// 모듈 시작
(function(window) {
	var sum = function(x, y) {
		return x + y;
	}

	var sub = function(x, y) {
		return x - y;
	}

	var math = {
		findSum: function(a, b) {
			return sum(a, b);
		},
		findSub: function(a, b) {
			return sub(a, b);
		}
	}

	window.math = math;
})(window)

// 모듈 끝

// link math.js script
console.log(math.findSum(1, 2));	// 3
console.log(math.findSub(1, 2));	// -1
```

### 모듈구현 - AMD (Require JS)

```js
// math.js
// math 객체 export
define(function () {
  var sum = function(x, y) {
		return x + y;
	}

	var sub = function(x, y) {
		return x - y;
	}

	var math = {
		findSum: function(a, b) {
			return sum(a, b);
		},
		findSub: function(a, b) {
			return sub(a, b);
		}
	}
  
  return math;
});

// index.js
// math 객체 import
require(['math'], function (math){

  console.log(math.findSum(1, 2));  // 3
  console.log(math.findSub(1, 2));  // -1
});
```

### 모듈구현 - Common JS (node)

```js
// math.js
// 모듈 생성, export
var sum = function (x, y) {
  return x + y;
}

var sub = function (x, y) {
  return x - y;
}

var math = {
  findSum: function (a, b) {
    return sum(a, b);
  },
  findSub: function (a, b) {
    return sub(a, b);
  }
}

exports.math = math;

// index.js
// 모듈 import
var math = require('./math').math;

console.log(math.findSum(1, 2));  // 3
console.log(math.findSub(1, 2));  // -1
```

### 모듈구현 - UMD

```js
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    define([], factory);
  } else if (typeof exports === 'object') {
    module.exports = factory();
  } else {
    root.math = factory();
  }
} (this, function () {
  // 모듈 정의
  var sum = function (x, y) {
    return x + y;
  }
  var sub = function (x, y) {
    return x - y;
  }
  var math = {
    findSum: function (a, b) {
      return sum(a, b);
    },
    findSub: function (a, b) {
      return sub(a, b);
    }
  }
  return math;
}));
```

*****

## ES6 module import / export

### export 기본

변수나 함수, 클래스 선언 앞에 `export`를 사용하여 익스포트 시킬 수 있다.

```js
// 데이터 익스포트
export var color = 'red';
export let name = 'Nicholas';
export const magicNumber = 7;

// 함수 익스포트
export function sum(num1, num2) {
    return num1 + num2;
}

// 클래스 익스포트
export class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }
}

// 이 함수는 모듈에 비공개
function subtract(num1, num2) {
    return num1 - num2;
}

// 함수 정의
function multiply(num1, num2) {
    return num1 * num2;
}

// 위에서 정의한 함수를 익스포트
export { multiply };
```

### import 기본

익스포트한 모듈이 있을 때, `import` 키워드를 사용하여 다른 모듈에 접근할 수 있다. 

```js
import { identifier1, identifier2 } from './exmaple.js';
```

모듈로부터 바인딩을 임포트할 때, 바인딩은 const를 사용하여 정의한 것처럼 동작한다.

> 임포트할 바인딩의 리스트는 구조분해된 객체와 유사해보이지만, 구조분해된 객체가 아니다.

```js
// 단 하나의 식별자 임포트
import { sum } from './example.js';

console.log(sum(1, 2));         // 3

sum = 1;                        // 에러 발생
```

```js
// 여러 개 임포트
import { sum, multiply, magicNumber } from './example.js';
console.log(sum(1, magicNumber);        // 8
console.log(multiply(1, 2));            // 2
```

```js
// 모두 임포트
import * as example from './example.js';

console.log(example.sum(1, example.magicNumber));   // 8
console.log(example.multiply(1, 2);                 // 2
```

### import/export에 새로운 이름 사용하기

```js
function sum(num1, num2) {
    return num1 + num2;
}

export { sum as add };
```

```js
import { add } from './example.js';
```

```js
import { add as sum } from './example.js';
console.log(typeof add);        // undefined
console.log(sum(1, 2));         // 3
```

### 모듈의 기본값 default

모듈 문법은 모듈에서 기본 값을 익스포트하고 임포트하도록 최적화되어 있는데, 그 이유는 이 패턴이 CommonJS(브라우저가 아닌 곳에서 자바스크립트를 사용하기 위한 또 다른 명세) 같은 다른 모듈시스템에서도 매우 일반적이기 때문이다. 모듈의 기본 값은 `default` 키워드로 지정된 변수나 함수, 클래스이고, 모듈마다 하나의 익스포트 기본 값을 설정할 수 있다. `default` 키워드를 사용하여 여러 개를 익스포트하면 에러가 발생한다.

```js
export default function(num1, num2) {
    return num1 + num2;
}
```

`default` 키워드는 이것이 익스포트 기본 값임을 나타낸다. 모듈 자체가 함수를 나타내기 때문에 이름은 필요하지 않다.

```js
// 기본 값을 명시할 수도 있다.
function sum(num1, num2) {
    return num1 + num2;
}

export default sum;
```

```js
function sum(num1, num2) {
    return num1 + num2;
}

export { sum as default }
```

```js
// 기본 값 임포트
import sum from './example.js';

console.log(sum(1, 2));             // 3
```

기본 값과 다른 바인딩을 익스포트

```js
export let color = 'red';

export default function(num1, num2) {
    return num1 + num2;
}
```

```js
import sum, { color } from './example.js';

console.log(sum(1, 2));         // 3
console.log(color);             // red
```

```js
import { default as sum, color } from 'example';

console.log(sum(1, 2));         // 3
console.log(color);             // red
```

바인딩을 다시 익스포트하기

```js
import { sum } from './example.js';
export { sum };
```

```js
export { sum } from './example.js';
```

```js
export { sum as add } from './example.js';
```

*****

### CounterApp 마무리

- color state 추가
- onSetColor prop / handleSetColor 추가
- getRandomColor 추가

```js
// CounterApp/index.js

// ...
function getRandomColor() {
  const colors = [
    '#495057',
    '#f03e3e',
    '#d6336c',
    '#ae3ec9',
    '#7048e8',
    '#4263eb',
    '#1c7cdb',
    '#1098ad',
    '#0ca678',
    '#37b24d',
    '#74b816',
    '#f59f00',
    '#f76707'
  ];

  const random = Math.floor(Math.random() * 13);

  return colors[random];
}
// ...
```

- Counter 컴포넌트 수정
- getRandomColor 함수 모듈로 분리