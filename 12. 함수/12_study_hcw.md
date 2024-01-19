# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

## 함수란?

- **`일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것`**
- 자바스크립트의 함수는 객체 타입의 값

![image](https://github.com/chowonn/Algorithm-js/assets/70478015/ad6672cb-314f-4b72-bc04-1a776b5f3aca)

## 함수 정의 방법

![image](https://github.com/chowonn/Algorithm-js/assets/70478015/9333ceb3-2efd-4a1b-ad39-818baf7d64aa)

### 함수 선언문

- 함수 선언문은 함수 리터럴과 형태가 동일하다.
- `함수 선언문은 표현식이 아닌 문` => 변수에 할당 불가

🙄 차이점은?

- 함수 리터럴은 함수 이름을 생략할 수 있으나, **`함수 선언문은 함수 이름을 생략할 수 없다!`**

### 12-06

```javascript
// 함수 선언문은 함수 이름을 생략할 수 없다.
function (x, y) {
  return x + y;
}
// SyntaxError: Function statements require a function name
```

### 함수 표현식

- 함수는 일급 객체이므로 함수 리터럴로 생성한 함수 객체를 변수에 할당하는 방식 => `함수 표현식`
- 함수 표현식의 함수 리터럴은 함수 이름을 생략하는 것이 일반적이다 => `익명 함수`
- `함수 표현식은 표현식인 문` => 변수에 할당 가능

<br>

**일급객체가 되기 위한 조건**

1. 값을 변수나 데이터에 할당할 수 있어야 한다 => `함수 표현식으로 자유롭게 대입 가능`
2. 파라미터로 넘길 수 있어야 한다 => `콜백 함수 형태로 전달 가능`
3. 함수의 리턴 값으로 사용할 수 있어야 한다 => `클로저 통해서 구성 가능 `

**`즉, 자바스크립트의 함수는 일급객체다.`**

### 12-11

```javascript
// 기명 함수 표현식
var add = function foo(x, y) {
  return x + y;
};

// 함수 객체를 가리키는 식별자로 호출
console.log(add(2, 5)); // 7

// 함수 이름으로 호출하면 ReferenceError가 발생한다.
// 함수 이름은 함수 몸체 내부에서만 유효한 식별자다.
console.log(foo(2, 5)); // ReferenceError: foo is not defined
```

### 화살표 함수

- function 키워드 대신 `화살표 =>` 를 사용해 간략하게 표현할 수 있다. (항상 익명함수로 정의)

### 12-15

```javascript
// 화살표 함수
const add = (x, y) => x + y;
console.log(add(2, 5)); // 7
```

```javascript
// 함수 선언문
function add(x, y) {
  return x + y;
}

console.log(add(2, 5)); // 7
```

```javascript
// 함수 표현식
const add = function (x, y) {
  return x + y;
};

console.log(add(2, 5)); // 7
```

## 다양한 함수의 형태

### 즉시 실행 함수 (IIFE - Immediately Invoked Function Expression)

- **함수의 정의와 동시에 즉시 호출되는 함수**
- 익명 함수를 사용하는 것이 일반적이다
- IIFE는 함수를 실행한 뒤 바로 소멸한다. (스코프 제한) => `전역 스코프의 오염을 막기 위해`
- 단 한 번만 호출되고 다시 호출할 수 없다. (최초 한 번만 실행하는 것이 목적)

### 12-42

```javascript
// 즉시 실행 함수도 일반 함수처럼 값을 반환할 수 있다.
var res = (function () {
  var a = 3;
  var b = 5;
  return a * b;
})();

console.log(res); // 15

// 즉시 실행 함수에도 일반 함수처럼 인수를 전달할 수 있다.
res = (function (a, b) {
  return a * b;
})(3, 5);

console.log(res); // 15
```

### 재귀 함수

- **함수가 자기 자신을 호출하는 행위를 하는 함수**
- 보통 반복 처리를 위해 사용함
- 자신을 무한 호출하므로 반드시 `탈출 조건`을 만들어야 한다.

### 12-45

```javascript
// 팩토리얼(계승)은 1부터 자신까지의 모든 양의 정수의 곱이다.
// n! = 1 * 2 * ... * (n-1) * n
function factorial(n) {
  // 탈출 조건: n이 1 이하일 때 재귀 호출을 멈춘다.
  if (n <= 1) return 1;
  // 재귀 호출
  return n * factorial(n - 1);
}

console.log(factorial(0)); // 0! = 1
console.log(factorial(1)); // 1! = 1
console.log(factorial(2)); // 2! = 2 * 1 = 2
console.log(factorial(3)); // 3! = 3 * 2 * 1 = 6
console.log(factorial(4)); // 4! = 4 * 3 * 2 * 1 = 24
console.log(factorial(5)); // 5! = 5 * 4 * 3 * 2 * 1 = 120
```

![image](https://github.com/chowonn/Algorithm-js/assets/70478015/1a7fb9b0-5ce5-4723-95ed-906d356a47a7)

---

`단, 함수 외부에서 함수를 호출할 때는 반드시 함수를 가리키는 식별자로 해야한다`

### 12-46

```javascript
// 함수 표현식
// 식별자 => factorial
// 함수 이름 => foo
var factorial = function foo(n) {
  // 탈출 조건: n이 1 이하일 때 재귀 호출을 멈춘다.
  if (n <= 1) return 1;
  // 함수를 가리키는 식별자로 자기 자신을 재귀 호출
  return n * factorial(n - 1);

  // 함수 이름으로 자기 자신을 재귀 호출할 수도 있다.
  // console.log(factorial === foo); // true
  // return n * foo(n - 1);
};

console.log(factorial(5)); // 5! = 5 * 4 * 3 * 2 * 1 = 120
```

### 콜백 함수

- **함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수** (다른 함수에 인자로 전달외어 나중에 실행되는 함수)
- 변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수 => **고차 함수**
- 콜백 함수도 고차 함수에 전달되어 헬퍼 함수 역할을 함.
- 콜백 함수는 고차 함수에 의해 호출되고, 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달할 수 있다.
- 비동기 처리(이벤트, ajax, 타이머 함수) 에 많이 활용된다.

### 12-51

```javascript
// 외부에서 전달받은 f를 n만큼 반복 호출한다.
function repeat(n, f) {
  for (var i = 0; i < n; i++) {
    f(i); // i를 전달하면서 f를 호출
  }
}

var logAll = function (i) {
  console.log(i);
};

// 반복 호출할 함수를 인수로 전달한다.
repeat(5, logAll); // 0 1 2 3 4

var logOdds = function (i) {
  if (i % 2) console.log(i);
};

// 반복 호출할 함수를 인수로 전달한다.
repeat(5, logOdds); // 1 3
```

- 변하지 않는 공통 로직은 미리 정의하고, 경우에 따라 변경되는 로직은 함수f 로 추상화함.

<br>

# 📗배운 점

## 함수 선언문 VS 함수 표현식

- 선언문과 표현식 각각으로 정의한 `함수의 생성 시점`이 다르다.
- 선언문으로 선언할 시 런타임 이전에 함수 객체가 먼저 생성됨. `즉, 함수 선언문 이전에 함수를 참조하고 호출할 수 있다(호이스팅)`
- 표현식으로 정의하면 런타임에 평가되므로 할당문이 실행되는 시점에 평가되어 함수 객체가 된다. => `함수 호이스팅x, 변수 호이스팅 발생o`

<br>

**`함수 선언문은 호이스팅에 영향을 받지만, 함수 표현식을 호이스팅에 영향을 받지 않는다.`**
[참고](https://joshua1988.github.io/web-development/javascript/function-expressions-vs-declarations/)

### 12-12

```javascript
// 함수 참조
console.dir(add); // ƒ add(x, y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x - y;
};
```

<br>

# 🤔궁금한 점

### 매개변수보다 인수가 더 많은 경우 초과된 인수는 어떻게 될까?

=> `arguments 객체의 프로퍼티로 저장된다.`

arguments 객체는 함수 내부에서 사용할 수 있는 유사 배열 객체로, 함수에 전달된 모든 인수를 포함하고 있다.

### 12-19

```javascript
function add(x, y) {
  return x + y;
}

console.log(add(2, 5, 10)); // 7
```

### 12-20

```javascript
function add(x, y) {
  console.log(arguments);
  // Arguments(3) [2, 5, 10, callee: ƒ, Symbol(Symbol.iterator): ƒ]

  return x + y;
}

add(2, 5, 10);
```

**그럼 arguments 객체는 어디서 사용될까? 🤔**

1. `가변 인자 함수 구현 할 때`

- 함수가 정해진 개수의 매개변수로 호출되지 않고 다양한 개수의 인수를 받아서 처리할 때 arguments 객체 를 사용할 수 있다.

2. `인수의 개수에 따라 동작을 다르게 구현할 때`

- arguments.length를 통해 전달된 인수의 개수에 따라 각각 다른 동작을 하도록 구현할 수 있다.

<br>

> ES6에서 도입된 rest parameters나 화살표 함수에서 사용되지 않는 추세.. 앞의 방법을 사용하는게 더 간결하고 코드를 명확하게 작성할 수 있음.
