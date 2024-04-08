## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 26.1 함수의 구분

```javascript
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // -> 1

// 생성자 함수로서 호출
new foo(); // -> foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // -> 1
```

```javascript
var foo = function () {};

// ES6 이전의 모든 함수는 callable이면서 constructor다.
foo(); // -> undefined
new foo(); // -> foo {}
```

- ES6 이전의 모든 함수는 일반 함수로서 호출할 수 잇는 것은 물론 생성자 함수로서 호출할 수 있음
- ES6 이전의 모든 함수는 callable이면서 constructor

```javascript
// 프로퍼티 f에 바인딩된 함수는 callable이며 constructor다.
var obj = {
  x: 10,
  f: function () {
    return this.x;
  },
};

// 프로퍼티 f에 바인딩된 함수를 메서드로서 호출
console.log(obj.f()); // 10

// 프로퍼티 f에 바인딩된 함수를 일반 함수로서 호출
var bar = obj.f;
console.log(bar()); // undefined

// 프로퍼티 f에 바인딩된 함수를 생성자 함수로서 호출
console.log(new obj.f()); // f {}
```

- 객체에 바인딩된 함수가 constructor라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며, 프로토타입 객체도 생성한다는 것을 의미
- 이는 성능 면에서 문제가 있음
- 콜백 함수도 constructor이기 때문에 불필요한 프로토타입 객체를 생성

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/ebdaee98-e760-46c4-9818-4828baf93fe9)

ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분

## 26.2 메서드

```javascript
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() {
    return this.x;
  },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function () {
    return this.x;
  },
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```

- ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor임

```javascript
new obj.foo(); // -> TypeError: obj.foo is not a constructor
new obj.bar(); // -> bar {}
```

- ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않음

```javascript
const base = {
  name: "Lee",
  sayHi() {
    return `Hi! ${this.name}`;
  },
};

const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드다. ES6 메서드는 [[HomeObject]]를 갖는다.
  // sayHi의 [[HomeObject]]는 sayHi가 바인딩된 객체인 derived를 가리키고
  // super는 sayHi의 [[HomeObject]]의 프로토타입인 base를 가리킨다.
  sayHi() {
    return `${super.sayHi()}. how are you doing?`;
  },
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 가짐
- ES6 메서드가 아닌 함수는 super 키워드를 사용할 수 없음
  - 내부 슬롯 [[HomeObject]]를 갖지 않기 때문

## 26.3 화살표 함수

### 26.3.2 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor.

```javascript
const Foo = () => {};
// 화살표 함수는 생성자 함수로서 호출할 수 없다.
new Foo(); // TypeError: Foo is not a constructor
```

- 화살표 함수는 인스턴스를 생성할 수 없으므노 prototype 프로퍼티가 없고 프로토타입도 생성하지 않음

2. 중복된 매개변수 이름을 선언할 수 없음

```javascript
const arrow = (a, a) => a + a;
// SyntaxError: Duplicate parameter name not allowed in this context
```

3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않음

- 화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 스코프 체인을 통해 상위 스코프의 this.arguments, super, new.target을 참조함

### 26.3.3 this

```javascript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map((item) => this.prefix + item);
  }
}

const prefixer = new Prefixer("-webkit-");
console.log(prefixer.add(["transition", "user-select"]));
// ['-webkit-transition', '-webkit-user-select']
```

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음
- 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조, 이를 lexical this라 함

```javascript
// 화살표 함수는 상위 스코프의 this를 참조한다.
() => this.x;

// 익명 함수에 상위 스코프의 this를 주입한다. 위 화살표 함수와 동일하게 동작한다.
(function () {
  return this.x;
}).bind(this);
```

- 만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조

```javascript
// Bad
const person = {
  name: "Lee",
  sayHi: () => console.log(`Hi ${this.name}`),
};

// sayHi 프로퍼티에 할당된 화살표 함수 내부의 this는 상위 스코프인 전역의 this가 가리키는
// 전역 객체를 가리키므로 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는
// window.name과 같다. 전역 객체 window에는 빌트인 프로퍼티 name이 존재한다.
person.sayHi(); // Hi
```

- 화살표 함수로 메서드를 정의하는 것은 바람직하지 않음

```javascript
// Bad
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = () => console.log(`Hi ${this.name}`);

const person = new Person("Lee");
// 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는 window.name과 같다.
person.sayHi(); // Hi
```

- 프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 동일한 문제 발생

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 재설정
  constructor: Person,
  sayHi() {
    console.log(`Hi ${this.name}`);
  },
};

const person = new Person("Lee");
person.sayHi(); // Hi Lee
```

- 일반 함수가 아닌 ES6 메서드를 동적 추가하고 싶다면 객체 리터럴을 바인딩하고 프로토타입의 constructor 프로퍼티와 생성자 함수 간의 연결을 재설정

```javascript
// Bad
class Person {
  // 클래스 필드 정의 제안
  name = "Lee";
  sayHi = () => console.log(`Hi ${this.name}`);
}

const person = new Person();
person.sayHi(); // Hi Lee
```

- 클래스 필드 정의 제안을 사용하여 클래스 필드에 화살표 함수를 할당할 수도 있음
- 이때 sayHi 클래스 필드에 할당한 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this 바인딩을 참조
- 여기서의 상위 스코프는 무엇?

```javascript
class Person {
  constructor() {
    this.name = "Lee";
    // 클래스가 생성한 인스턴스(this)의 sayHi 프로퍼티에 화살표 함수를 할당한다.
    // sayHi 프로퍼티는 인스턴스 프로퍼티이다.
    this.sayHi = () => console.log(`Hi ${this.name}`);
  }
}
```

- sayHi 클래스 필드는 인스턴스 프로퍼티이므로 화살표 함수의 상위 스코프는 클래스 외부
- 하지만 this는 클래스 외부의 this를 참조하지 않고 클래스가 생성할 인스턴스를 참조
- 따라서 sayHi 클래스 필드에서의 this는 constructor 내부의 this 바인딩과 같음

### 26.3.4 super

화살표 함수는 함수 자체의 super 바인딩을 갖지 않음

따라서 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조

### 26.3.5 arguments

화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않음

따라서 화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 suparguments를 참조

## 26.4 Rest 파라미터

Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받음

화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 함

## 26.5 매개변수 기본값

매개변수에 인수가 전달되었는지 확인하여 인수가 전달되지 않은 경우 매개변수에 기본값을 할당할 필요가 있음

```javascript
function sum(x, y) {
  // 인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값을 할당한다.
  x = x || 0;
  y = y || 0;

  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```

## 🤔궁금한 점

## 📌중요한 점
