## 목차

[📗배운 점 ](#📗배운-점)

- [📗배운 점](#배운-점)
  - [함수 선언문](#함수-선언문)
  - [함수 표현식](#함수-표현식)
  - [⭐함수 생성 시점과 함수 호이스팅](#함수-생성-시점과-함수-호이스팅)
  - [12.6 참조에 의한 전달과 외부 상태의 변경](#126-참조에-의한-전달과-외부-상태의-변경)
  - [12.7.5 순수함수와 비순수 함수](#1275-순수함수와-비순수-함수)
  - [🤔궁금한 점](#궁금한-점)
  - [📌중요한 점](#중요한-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

# 📗배운 점

함수 선언문은 문(이름 생략 불가능)
함수 표현식은 식

### 함수 선언문

foo 식별자를 선언 한적 없고 할당한적 없지만 자바스크립트 엔진이 암묵적으로 색성한 식별자.
![image](https://github.com/HYHYJ/Algorithm-solving-with-js/assets/101866872/daa74208-8424-4b7c-8d8e-0f1bfdeed787)
자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고 거기에 함수 객체를 할당한다.

함수는 함수이름을 호출하는것이 아니라 함수객체를 가리키는 식별자를 호출한다.
![image](https://github.com/HYHYJ/Algorithm-solving-with-js/assets/101866872/252c3b06-790b-4761-931a-7937768e677d)

### 함수 표현식

자바스크립트의 함수는 일급 객체

### ⭐함수 생성 시점과 함수 호이스팅

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

함수 선언문으로 정의한 함수와 함수 표현으로 정의한 함수의 생성 시점이 다르기 때문이다.

함수 선언문(함수 호이스팅): 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징이 함수 호이스탕
런타임 이전에 함수 객체가 먼저 생성된다.

함수 표현식(변수 호이스팅): 변수 할당문의 값은 할당문이 실행되는 시점, 즉 런타임에 평가되므로 함수 표현식의 함수 리터럴도 할당문이 실행되는 시점에 평가되어 함수 객체가 된다.

변수는 처음에 호이스팅 될때 undefined로 초기화 된다.
![image](https://github.com/HYHYJ/Algorithm-solving-with-js/assets/101866872/adf3fa1d-86f4-4bcc-a49d-78b63505c149)

### 12.6 참조에 의한 전달과 외부 상태의 변경

![image](https://github.com/HYHYJ/Algorithm-solving-with-js/assets/101866872/bcfac41b-b7bf-4ef3-9058-9fd18bd38ef9)

### 12.7.5 순수함수와 비순수 함수

- 순수함수: 외부 상태가 변경되지 않고 어떤 외부 상태에도 의존 하지 않는다. 동일한 인수가 전달되면 언제나 동일한 값을 반환하는 함수

```javascript
var count = 0; // 현재 카운트를 나타내는 상태

// 순수 함수 increase는 동일한 인수가 전달되면 언제나 동일한 값을 반환한다.
function increase(n) {
  return ++n;
}

// 순수 함수가 반환한 결과값을 변수에 재할당해서 상태를 변경
count = increase(count);
console.log(count); // 1

count = increase(count);
console.log(count); // 2
```

- 비순수함수: 외부상태를 변경하고 직접 참조한다. 함수의 외부 상태에 따라 반환값이 달라지는 함수.

```javascript
var count = 0; // 현재 카운트를 나타내는 상태: increase 함수에 의해 변화한다.

// 비순수 함수
function increase() {
  return ++count; // 외부 상태에 의존하며 외부 상태를 변경한다.
}

// 비순수 함수는 외부 상태(count)를 변경하므로 상태 변화를 추적하기 어려워진다.
increase();
console.log(count); // 1

increase();
console.log(count); // 2
```

## 🤔궁금한 점

## 📌중요한 점
