## 목차

[📗배운 점](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

### 빌트인 객체

> 표준 빌트인 객체 : ECMAScript 에 정의 된 객체
> 호스트 객체 : 브라우저 환경 또는 Node.js 에서 추가로 제공하는 객체
> 사용자 정의 객체 : 사용자가 직접 정의한 객체

### 표준 빌트인 객체

자바 스크립트는 약 40개의 표준 빌트인객체를 제공한다.
표준 빌트인 객체는 생성자 함수로 호풀하여 인스턴스를 생성할 수 있다.
표준 빌트인 객체는 인스턴스 없이도 호출 가능한 빌트인 정적 메서드를 제공한다.

```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String("Lee"); // String {"Lee"}
console.log(typeof strObj); // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj); // object

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj = new Boolean(true); // Boolean {true}
console.log(typeof boolObj); // object

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function("x", "return x * x"); // ƒ anonymous(x )
console.log(typeof func); // function

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3); // (3) [1, 2, 3]
console.log(typeof arr); // object

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i); // /ab+c/i
console.log(typeof regExp); // object

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date(); // Fri May 08 2020 10:43:25 GMT+0900 (대한민국 표준시)
console.log(typeof date); // object
```

### 원시값과 래퍼 객체

원시값이 있는데 객체를 생성하는 String,Number,Blooean이 존재하는 이유는 뭘까?
원시값은 객체가 아니지만 매소드 사용이 가능하다.

```javascript
const str = "hello";

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

원시값인 문자열, 숫자, 블리언 값의 경우 마침표 표기법으로 접근하면 일시적으로 원시값과 연관된 객체로 변환해 주고 다시 원시값으로 되돌린다.
이때 <U>잠시 생셩되는 임시 객체</U>를 **래퍼 객체**라고 한다.

객체의 처리가 종료되면 래버 객체는 가비지 컬랙션의 대상이 된다.

### 전역객체

> 전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다 먼저 생성되는 특수한 객체이며 어떤 객체에도 속하지 않은 최상위 객체이다.

이때 전역객체는 환경에 따라 이름이 제각각이다

```javascript
// 브라우저 환경
globalThis === this; // true
globalThis === window; // true
globalThis === self; // true
globalThis === frames; // true

// Node.js 환경(12.0.0 이상)
globalThis === this; // true
globalThis === global; // true
```

- 전역객체는 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- JS 실행환경에 따라 추가적으로 프로퍼티와 매서드를 가진다.
- var 키워드로 선언한 전역변수와 언언하지 않은 변수에 값을 할당한 암묵적 전역, 전역함수는 객체의 프로퍼티가 된다.
- **let 과 const는 전역객체의 프로퍼티가 아니다**

#### 빌트인 전역 프로퍼티

- Infinity - 무한대를 나타내는 숫자값
- NaN - not a number 을 나타내는 숫자값
- undefined - 원시타입 undefined를 값으로 가짐

#### 빌트인 전역 함수

- eval - JS 코드를 문자열 인수로 전달봗아 식을 평가해 값을 생성한다. 문자열 코드가 여려개의 문이라면 마지막 결과를 반환한다.
  기존의 스코프를 런타임에 동적으로 수정한다.
  strict mode 에서는 자체적인 스코프를 생성한다.
  **보안에 취약하고 최적화가 수행되지 않으므로 지양해야한다.**

- isFinite - 전달받은 인수가 정상적인 유한수인지 검사하여 유한수 이면 true를 반환, 전달받은 인수의 타입이 숫자가 아니라면 숫자로 타입을 변환한 후검사를 수행한다.

- isNaN - 전달받은 인수가 NaN 인지 검사한다. 그 결과를 불리언 타입으로 반환한다.

- parseFloat - 전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환한다.

- parseInt - 전달받은 인수를 정수로 해석하여 반환한다.
  두번째 인수에 기수를 지정하면(2~36진법) 첫인수를 해당 기수로 해석해서 반환한다.

- ※ 기수를 지정하여 10진수를 해당 기수의 문자열로 변환 하고 싶을땐
  Number.protorype.toString메서드를 사용한다.
  하지만 2진수와 8진수 리터럴은 제대로 해석하지 못한다.

- encodeURI / decodeURI
  인코딩이란 URI의 문자들을 이스케이프 처리하는것(어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는것)을 의미한다.
  디코딩이란 인코딩을 반대로 하여 이스케이프 처리 이전으로 돌리는 것을 말한다.

- encodeURIComponent / decodeURIComponent
  인코딩에서 쿼리 스트링 구분자로 사용되는 = ? & 까지 인코딩 하는을 말한다.

#### 암묵적 전역

```javascript
var x = 10; // 전역 변수

function foo() {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

y는 선언하지 않은 식별자지만 전역객체의 프로퍼티가 되어 마지 전역번수처럼 동작한다. 이러한 현상을 안묵적 전역이라 한다.
하지만 객체의 프로퍼티로 추가가 되었을뿐 변수는 아니다.
따라서 호이스팅은 되지 않는다.
또한 변수가 아니라 프로퍼티이기 때문에 delete 연산자로 삭제할 수 있다.

## 🤔궁금한 점

## 📌중요한 점
