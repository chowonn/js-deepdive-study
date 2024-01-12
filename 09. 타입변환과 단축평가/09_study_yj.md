## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 09 타입 변환과 단축 평가

### 9.1 타입 변환이란?

명시적 타입 변환/타입캐스팅 : 개발자가 의도적으로 값의 타입을 변환하는 것

암묵적 타입 변환/타입 강제 변환 : 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것

### 9.2 암묵적 타입 변환

#### 9.2.1 문자열 타입으로 변환

자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결 연산자의 피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환함

```js
1 + "2"; // -> "12"
```

#### 9.2.2 숫자 타입으로 변환

자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 변환함

- 피연산자를 숫자 타입으로 변환할 수 없는 경우에는 평가 결과가 `NaN`

```js
// 문자열 타입
+"" + // -> 0
  "0" + // -> 0
  "1" + // -> 1
  "string" + // -> NaN
  // 불리언 타입
  true + // -> 1
  false + // -> 0
  // null 타입
  null + // -> 0
  // undefined 타입
  undefined + // -> NaN
  // 심벌 타입
  Symbol() + // -> TypeError: Cannot convert a Symbol value to a number
  // 객체 타입
  {} + // -> NaN
  [] + // -> 0
  [10, 20] + // -> NaN
  function () {}; // -> NaN
```

빈 문자열(' '). 빈 배열([ ]), `null`, `false`는 0으로 `true`는 1로 변환

객체와 빈 배열이 아닌 배열, `undefined`는 변환되지 않아 `NaN`이 됨

#### 9.2.3 불리언 타입으로 변환

자바스크립트 엔진은 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환함

- 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy 값은 true로 Falsy 값은 false로 암묵적 타입 변환됨

- Falsy 값 종류
  - `false`, `undefiend`, `null`, 0, -0, `NaN`, ' '(빈 문자열)

```js
// 아래의 조건문은 모두 코드 블록을 실행한다.
if (!false) console.log(false + " is falsy value");
if (!undefined) console.log(undefined + " is falsy value");
if (!null) console.log(null + " is falsy value");
if (!0) console.log(0 + " is falsy value");
if (!NaN) console.log(NaN + " is falsy value");
if (!"") console.log("" + " is falsy value");
```

- 그 외의 값은 모두 Truthy 값

### 9.3 명시적 타입 변환

#### 9.3.1 문자열 타입으로 변환

![](2024-01-12-05-43-22.png)

#### 9.3.2 숫자 타입으로 변환

![](2024-01-12-05-43-58.png)

#### 9.3.3 불리언 타입으로 변환

![](2024-01-12-05-44-29.png)

### 9.4 단축 평가

#### 9.4.1 논리 연산자를 사용한 단축 평가

`&&`연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환

```js
"Cat" && "Dog"; // -> "Dog"
```

첫 번째 피연산자 'Cat'은 Truthy 값이므로 true로 평가, 하지만 이 시점까지는 위 표현식을 평가할 수 없음

두 번째 피연산자까지 평가해 보아야 함, 두 번째 피연산자가 `&&`연산자 표현식의 평가 결과를 결정

이 때 `&&` 연산자는 논리 연산의 결과를 결정하는 두 번째 피연산자, 즉 문자열 'Dog'를 그대로 반환

`||`연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환

```js
"Cat" || "Dog"; // -> "Cat"
```

첫 번째 피연산자 'Cat'은 Truthy 값이므로 true로 평가

이 시점에 두 번째 피연산자까지 평가하지 않아도 위 표현식을 평가할 수 있음

이 때 `||` 연산자는 논리 연산의 결과를 결정한 첫 번째 피연산자, 즉 문자열 'Cat'을 그대로 반환

단축 평가 : 표현식을 평사하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것

단축 평가를 사용하면 if문을 대체할 수 있음

- 조건이 Truthy 값일 때는 `&&`연산자

```js
var done = true;
var message = "";

// 주어진 조건이 true일 때
if (done) message = "완료";

// if 문은 단축 평가로 대체 가능하다.
// done이 true라면 message에 '완료'를 할당
message = done && "완료";
console.log(message); // 완료
```

- 조건이 Falsy 값일 때는 `||`연산자

```js
var done = false;
var message = "";

// 주어진 조건이 false일 때
if (!done) message = "미완료";

// if 문은 단축 평가로 대체 가능하다.
// done이 false라면 message에 '미완료'를 할당
message = done || "미완료";
console.log(message); // 미완료
```

객체를 가리키기를 기대하는 변수의 값이 객체가 아니라 null 또는 undefined인 경우 객체의 프로퍼티를 참조하면 타입 에러가 발생

```js
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null
```

단축 평가를 사용하면 에러가 발생하지 않음

```js
var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // -> null
```

함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당

이 때 단축 평가를 사용해 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있음

```js
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || "";
  return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = "") {
  return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2
```

#### 9.4.2 옵셔널 체이닝 연산자

`?.` : 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어감

- 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용

```js
var elem = null;

// elem이 Falsy 값이면 elem으로 평가되고 elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value;
console.log(value); // null
```

좌항 피연산자가 false로 평가되는 Falsy 값이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어감

#### 9.4.3 null 병합 연산자

`??` : 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환

- 변수에 기본값을 설정할 때 유용

```js
// 좌항의 피연산자가 null 또는 undefined이면 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
var foo = null ?? "default string";
console.log(foo); // "default string"
```

좌항의 피연산자가 false로 평가되는 Falsy값이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환

## 🤔궁금한 점

## 📌중요한 점
