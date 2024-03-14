# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

## this

- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수
- this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있음 (코드 어디서든 참조 가능)
- `this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정됨`
- 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있음 (일반 함수 내부의 this는 undefined가 바인딩 됨)

## 함수 호출 방식과 this 바인딩

> 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다. <br>
> => 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정함. 그러나 this 바인딩은 함수 호출 시점에 결정됨

### 함수 호출하는 방식

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

### 일반 함수 호출

- `기본적으로 this에는 전역 객체가 바인딩된다.`
- 그러나 this는 기본적으로 자기 참조 변수이므로 객체를 생성하지 않는 일반 함수에서는 의미가 없다.

```javascript
function foo() {
  console.log("foo's this: ", this); // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

메서드 내에 정의한 중첩함수도 일반 함수로 호출되면 전역 객체가 바인딩된다. (콜백함수도)

`어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.`

### 메서드 호출

- 메서드 내부의 this는 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다.
- 주의점은 `메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩 된다. `

```javascript
const person = {
  name: "Lee",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  },
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

### 생성자 함수 호출

- 생성자 함수 내부의 this에는 생상자 함수가 생성할 인스턴스가 바인딩된다.
- new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자가 아닌 일반 함수로 동작한다.

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

<br>

# 📗배운 점

## this 가 필요한 이유

- 생성자 함수를 정의하는 시점에는 인스턴스를 생성하기 전이므로 생성할 인스턴스를 가리키는 식별자를 알 수 없음.
- 따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요함 => `this`

## apply / call / bind 메서드에 의한 간접 호출

```javascript
function getThisBinding() {
  console.log(arguments);
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}
```

- apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것으로, 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.
- 대표적인 용도로는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우이다.

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

- apply, call 과 달리 bind는 함수를 호출하지 않는다.
- `bind는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기에 유용하다.`

<br>

| 함수 호출 방식                                             | this 바인딩                                                             |
| :--------------------------------------------------------- | :---------------------------------------------------------------------- |
| 일반 함수 호출                                             | 전역 객체                                                               |
| 메서드 호출                                                | 메서드를 호출한 객체                                                    |
| 생성자 함수 호출                                           | 생성자 함수가 (미래에) 생성할 인스턴스                                  |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에서 첫번째 인수로 전달한 객체 |

  <br>

# 🤔궁금한 점
