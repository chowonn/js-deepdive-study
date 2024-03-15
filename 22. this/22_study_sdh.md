## 목차

[📗배운 점](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

> **객체**: 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 붂은 복합적인 자료구조.

### this 키워드

메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

- 이를 위해 `this`라는 특수한 식별자가 존재한다.
- `this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
- `this`의 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.
- `this`는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 객체의 메서드내부 또는 생성자 함수 내부에서만 의미가 있다.

### 함수 호출 방식과 this의 바인딩

> this의 바인딩은 함수 호출방식에 의해 결정된다.

```javascript
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: "bar" };

foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar
```

- **일반 함수 호출** : this는 일반함수에선 의미가 없기 때문에 기본적으로 `this` 에는 전역객체(window)가 바인딩 된다. 엄격모드에서는 undefined가 바인딩 된다.

- **콜백 함수 호출** : 콜백함수 또한 전역객체가 바인딩 된다. 어떠한 함수도 일반 함수로 호출되면 전역객체가 바인딩 된다. (중첩함수 포함)

- **화살표 함수 호출** : 화살표 함수 내에선 상위 스코프의 `this`를 가리킨다.

- **매서드 호출** : 메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩 된다,

- **생성자 함수 호출** : 생성자 함수 내부의 `this`에는 생성자 함수가 생성할 인스턴스가 바인딩 된다.
  만약 new 연산자와 함께 호출하지 않았다면 일반 함수로 동작하게 된다.

- apply, call, bind : Function.prototype의 메서드다. 즉 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

이 둘의 본질적인 기능은 함수를 호풀하는 것이다.
apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
call 메서드는 호풀할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

bind 매서드의 `this`와 메서드 내부의 중첩함수 또는 콜백함수의 this가 불일치 하는 문제를 해결 하기 위해 유용하게 사용된다.

| 함수 호출 방식                                             | this 바인딩                                                             |
| ---------------------------------------------------------- | ----------------------------------------------------------------------- |
| 일반 함수 호출                                             | 전역 객체                                                               |
| 메서드 호출                                                | 메서드를 호출한 객체                                                    |
| 생성자 함수 호출                                           | 생성자 함수가 (미래에) 생성할 인스턴스                                  |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에서 첫번째 인수로 전달한 객체 |

## 🤔궁금한 점

## 📌중요한 점
