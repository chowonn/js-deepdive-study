# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

---

<br>

# 📌중요한 점

# 함수의 구분

- **ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다**
- 즉, ES6 이전의 모든 함수는 callable이면서 constructor이다.
- 이러한 방식은 문법적으로도 성능면에서도 좋지 않다. => ES6부터 목적에 따라 함수 구분

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

<br>

|  ES6 함수의 구분  | constructor | prototype | super | arguments |
| :---------------: | :---------: | :-------: | :---: | :-------: |
| 일반함수(Normal)  |      O      |     O     |   X   |     O     |
|  메서드(Method)   |      X      |     X     |   O   |     O     |
| 화살표함수(Arrow) |      X      |     X     |   X   |     X     |

<br>

# 메서드

- `ES6 부터 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.`
- ES6 메서드는 인스턴스를 생성할 수 없는 non-constructor다 => prototype 프로퍼티 x , 프로토타입 생성 x

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

new obj.foo(); // -> TypeError: obj.foo is not a constructor
new obj.bar(); // -> bar {}
```

```javascript
// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty("prototype"); // -> false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty("prototype"); // -> true
```

<br>

**ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯을 갖는다.**

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

<br>

# 화살표 함수와 일반 함수 차이

`1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.`

```javascript
const Foo = () => {};
// 화살표 함수는 생성자 함수로서 호출할 수 없다.
new Foo(); // TypeError: Foo is not a constructor
```

```javascript
const Foo = () => {};
// 화살표 함수는 prototype 프로퍼티가 없다.
Foo.hasOwnProperty("prototype"); // -> false
```

`2. 중복된 매개변수 이름을 선언할 수 없다.`

```javascript
function normal(a, a) {
  return a + a;
}
console.log(normal(1, 2)); // 4
```

```javascript
"use strict";

function normal(a, a) {
  return a + a;
}
// SyntaxError: Duplicate parameter name not allowed in this context
```

```javascript
const arrow = (a, a) => a + a;
// SyntaxError: Duplicate parameter name not allowed in this context
```

`3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.`

스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조한다.

<br>

# this

- 일반 함수로서 호출되는 모든 함수 내부의 this는 전역 객체를 가리킨다.
- 클래스 내부는 strict mode가 적용되는데 메서드의 콜백 함수에도 적용되므로 내부의 this에는 undefined가 바인딩된다.
- `반면에 화살표 함수에서는 자체 this 바인딩을 갖지 않고 상위 스코프의 this를 그대로 참조한다` => **lexical this**

```javascript
// 화살표 함수는 상위 스코프의 this를 참조한다.
() => this.x;

// 익명 함수에 상위 스코프의 this를 주입한다. 위 화살표 함수와 동일하게 동작한다.
(function () {
  return this.x;
}).bind(this);
```

화살표 함수끼리 중첩되어 있을 경우 스코프 체인 상으로 가장 가까운 상위 함수의 this를 참조한다.

```javascript
// 중첩 함수 foo의 상위 스코프는 즉시 실행 함수다.
// 따라서 화살표 함수 foo의 this는 상위 스코프인 즉시 실행 함수의 this를 가리킨다.
(function () {
  const foo = () => console.log(this);
  foo();
}).call({ a: 1 }); // { a: 1 }

// bar 함수는 화살표 함수를 반환한다.
// bar 함수가 반환한 화살표 함수의 상위 스코프는 화살표 함수 bar다.
// 하지만 화살표 함수는 함수 자체의 this 바인딩을 갖지 않으므로 bar 함수가 반환한
// 화살표 함수 내부에서 참조하는 this는 화살표 함수가 아닌 즉시 실행 함수의 this를 가리킨다.
(function () {
  const bar = () => () => console.log(this);
  bar()();
}).call({ a: 1 }); // { a: 1 }
```

<br>

# super

- 화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다.
- this와 마찬가지로 상위 super를 참조함

```javascript
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  // 화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킨다.
  sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived("Lee");
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

<br>

# arguments

- 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다.
- this와 마찬가지로 상위 arguments를 참조함

```javascript
(function () {
  // 화살표 함수 foo의 arguments는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
  const foo = () => console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
  foo(3, 4);
})(1, 2);

// 화살표 함수 foo의 arguments는 상위 스코프인 전역의 arguments를 가리킨다.
// 하지만 전역에는 arguments 객체가 존재하지 않는다. arguments 객체는 함수 내부에서만 유효하다.
const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError: arguments is not defined
```

# 📗배운 점

- **callable** : 호출할 수 있는 함수 객체
- **constructor** : 인스턴스를 생성할 수 있는 함수 객체

<br>
