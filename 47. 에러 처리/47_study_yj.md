## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 47 에러처리

## 47.1 에러 처리의 필요성

```javascript
console.log("[Start]");

try {
  foo();
} catch (error) {
  console.error("[에러 발생]", error);
  // [에러 발생] ReferenceError: foo is not defined
}

// 발생한 에러에 적절한 대응을 하면 프로그램이 강제 종료되지 않는다.
console.log("[End]");
```

- 발생한 에러에 대해 대처하지 않고 방치하면 프로그램은 강제 종료

```javascript
const $elem = document.querySelector("#1");
// DOMException: Failed to execute 'querySelector' on 'Document': '#1' is not a valid selector.
```

- querySelector 메서드는 인수로 전달한 문자열이 CSS 선택자 문법에 맞지 않는 경우 에러를 발생

```javascript
// DOM에 button 요소가 존재하지 않으면 querySelector 메서드는 에러를 발생시키지 않고 null을 반환한다.
const $button = document.querySelector("button"); // null

$button.classList.add("disabled");
// TypeError: Cannot read property 'classList' of null
```

- 직접적으로 에러를 발생하지 않는 예외적인 상황이 발생할 수도 있음
- querySelector 메서드는 인수로 전달한 CSS 선택자 문자열로 DOM에서 요소 노드 찾을 수 없는 경우 에러를 발생시키지 않고 null을 반환

```javascript
// DOM에 button 요소가 존재하는 경우 querySelector 메서드는 에러를 발생시키지 않고 null을 반환한다.
const $button = document.querySelector("button"); // null
$button?.classList.add("disabled");
```

- 이때 if문으로 querySelector 메서드의 반환값을 확인하거나 단축 평가 또옵셔널 체이닝 연산자 ?.를 사용하지 않으면 다음 처리에서 에러로 이어질 가능성이 큼

## 47.2 try...catch...finally 문

기본적으로 에러 처리를 구현하는 방법은 크게 두 가지가 있음

1. 예외적인 상황이 발생하면 반환하는 값(null 또는 -1)을 if 문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리, 에러 처리 코드를 미리 등록
2. try...catch...finally문, 일반적으로 이 방법을 에러 처리라고 함

```javascript
console.log("[Start]");

try {
  // 실행할 코드(에러가 발생할 가능성이 있는 코드)
  foo();
} catch (err) {
  // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
  // err에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
  console.error(err); // ReferenceError: foo is not defined
} finally {
  // 에러 발생과 상관없이 반드시 한 번 실행된다.
  // 불필요하다면 생략 가능
  console.log("finally");
}

// try...catch...finally 문으로 에러를 처리하면 프로그램이 강제 종료되지 않는다.
console.log("[End]");
```

## 47.3 Error 객체

Error 생성자 함수는 에러 객체를 생성

Error 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인수로 전달할 수 있음

```javascript
const error = new Error("invalid");
```

## 47.4 throw 문

Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아님, 에러 객체 생성과 에러 발생은 의미가 다름

에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야 함

```javascript
try {
  // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작한다.
  throw new Error("something wrong");
} catch (error) {
  console.log(error);
}
```

- throw 문의 표현식은 어떤 값이라도 상관없지만 일반적으로 에러 객체를 지정

## 47.5 에러의 전파

에러는 호출자 방향으로 전파

```javascript
const foo = () => {
  throw Error("foo에서 발생한 에러"); // ④
};

const bar = () => {
  foo(); // ③
};

const baz = () => {
  bar(); // ②
};

try {
  baz(); // ①
} catch (err) {
  console.error(err);
}
```

- ①에서 baz 함수 호출
- ②에서 bar 함수 호출
- ③에서 foo 함수 호출
- foo 함수는 ④에서 에러를 throw
- 이때 이 에러는 호출자에게 전파되어 전역에서 캐치
  - ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/feae6a8e-4125-48bd-857b-63f66af9c0e4)

주의할 것은 비동기 함수인 setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다는 것

## 🤔궁금한 점

## 📌중요한 점
