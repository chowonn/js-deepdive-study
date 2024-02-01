## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 15.1 var 키워드로 선언한 변수의 문제점

- 변수 중복 선언 허용
  - 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작하고 초기화문이 없는 변수 선언문은 무시

```javascript
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

- 함수 레벨 스코프

  - var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정
  - 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 됨

- 변수 호이스팅
  - var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작

```javascript
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다. (2. 초기화 단계)
console.log(foo); // undefined

// 변수에 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

## 15.2 let 키워드

- 변수 중복 선언 금지

  - let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러 발생

- 블록 레벨 스코프

  - let 키워드로 선언된 변수는 블록 레벨 스코프를 따름

    ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/befd1ffa-5737-440d-b8b0-74555ebdb1de)

- 변수 호이스팅

  - var 키워드로 선언한 변수는 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 "선언 단계"와 "초기화 단계"가 한번에 진행

    - 선언 단계에서 스코프에 변수 식별자를 등록해 자바스크립트 엔진에 변수의 존재를 알림
    - 그 즉시 초기화 단계에서 undefined로 변수를 초기화
    - 이후 변수 할당문에 도달하면 비로소 값이 할당

      ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/809c45d9-c2a3-437a-9e06-28ead4387714)

  - let 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행

    - 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행
    - 스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없음
    - 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대**라고 함

      ```javascript
      // 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화되지 않았다.
      // 초기화 이전의 일시적 사각 지대에서는 변수를 참조할 수 없다.
      console.log(foo); // ReferenceError: foo is not defined

      let foo; // 변수 선언문에서 초기화 단계가 실행된다.
      console.log(foo); // undefined

      foo = 1; // 할당문에서 할당 단계가 실행된다.
      console.log(foo); // 1
      ```

      ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/205f4e0b-4ed8-46b6-bb1a-0bd769e8eddb)

    ```javascript
    let foo = 1; // 전역 변수

    {
      console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
      let foo = 2; // 지역 변수
    }
    ```

    - let 키워드로 선언한 변수의 경우 변수 호이스팅이 발생하지 않는다면 위 예제는 전역 변수 foo의 값을 출력해야 함
    - 하지만 let 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 참조 에러가 발생
      - `let foo = 2;` 코드가 호이스팅으로 선언이 됐는데 초기화와 할당이 이루어지지 않았기 때문에 에러

- 전역 객체와 let

  ```javascript
  // 이 예제는 브라우저 환경에서 실행해야 한다.

  // 전역 변수
  var x = 1;
  // 암묵적 전역
  y = 2;
  // 전역 함수
  function foo() {}

  // var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
  console.log(window.x); // 1
  // 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
  console.log(x); // 1

  // 암묵적 전역은 전역 객체 window의 프로퍼티다.
  console.log(window.y); // 2
  console.log(y); // 2

  // 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
  console.log(window.foo); // ƒ foo() {}
  // 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
  console.log(foo); // ƒ foo() {}
  ```

  - var 키워드로 선언한 전역 변수와 전역 함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티

  ```javascript
  // 이 예제는 브라우저 환경에서 실행해야 한다.
  let x = 1;

  // let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
  console.log(window.x); // undefined
  console.log(x); // 1
  ```

  - let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님

## 15.3 const 키워드

- 선언과 초기화

  - const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 함

    ```javascript
    const foo; // SyntaxError: Missing initializer in const declaration
    ```

  - const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작

    ```javascript
    {
      // 변수 호이스팅이 발생하지 않는 것처럼 동작한다
      console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
      const foo = 1;
      console.log(foo); // 1
    }

    // 블록 레벨 스코프를 갖는다.
    console.log(foo); // ReferenceError: foo is not defined
    ```

- 재할당 금지

  - const 키워드로 선언한 변수는 재할당이 금지

- 상수

  - 변수의 상대 개념인 상수는 재할당이 금지된 변수
    - 상수도 값을 저장하기 위한 메모리 공간이 필요하므로 변수라고 할 수 있음
  - 변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있지만 상수는 재할당이 금지
  - const 키워드로 선언된 변수에 원시 값을 할당한 경우

    - 원시 값은 변경할 수 없는 값이고 const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없음

    ```javascript
    // 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
    // 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
    const TAX_RATE = 0.1;

    // 세전 가격
    let preTaxPrice = 100;

    // 세후 가격
    let afterTaxPrice = preTaxPrice + preTaxPrice * TAX_RATE;

    console.log(afterTaxPrice); // 110
    ```

- const 키워드와 객체
  - const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있음
  - 새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능
    - 이때 객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 않음

## 15.4 var vs let vs const

- ES6를 사용한다면 var 키워드는 사용하지 않음
- 재할당이 필요한 경우에 한정해 let 키워드 사용, 이때 변수의 스코프는 최대한 좁게
- 변경이 발생하지 않고 읽기 전용으로 사용하는(재할당이 필요 없는 상수) 원시 값과 객체에는 const 사용

## 🤔궁금한 점

## 📌중요한 점

변수 선언에는 기본적으로 const 사용, let은 재할당이 필요한 경우에 사용

const와 let은 변수 호이스팅이 일어나지 않는 것처럼 보이지만 실제로는 일어나고 있음
