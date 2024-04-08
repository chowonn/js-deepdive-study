# 📗챕터 정리

## 개요

ES6 이전의 모든 함수는 **일반 함수로서 호출**할 수 있으면서 **동시에 생성자 함수로서 호출**할 수 있다.

이 때, 객체의 메서드, 콜백 함수처럼 생성자 함수로서 호출할 필요가 없는 함수도 프로토타입 객체를 생성하였다.

이것은 불필요한 프로토타입 객체를 생성하게 되어 성능을 저하시키는 문제를 야기했다.

이러한 문제를 해결하기 위해 ES6 버전에서 현재 사용하고 있는 메서드와 화살표 함수가 등장하였다.

메서드와 화살표 함수는 생성자 함수로서 사용할 수 없고, 인스턴스 생성이 불가능한 ***non-constructor*** 함수이다.

---

## 메서드

메서드란 ES6 버전 이후, 객체 내에서 메서드 표현 문법으로 정의된 함수를 의미한다.

```jsx
const obj = {
  foo() {
	  return "hello, world"
  }
};
```

메서드 내에는 자신을 바인딩한 객체를 가리키는 [[*HomeObject*]] 내부 슬롯을 갖는다.

[[*HomeObject*]] 내부 슬롯을 가진 함수는 super 참조를 위해 `super` 키워드를 사용할 수 있다.

(클래스 챕터에서 다뤘던 `super` 키워드를 사용한 슈퍼클래스 메서드 참조)

---

## 화살표 함수

화살표 함수란 함수 표현식으로 정의한 함수이다.

개요에서 설명하였듯 ***non-constructor*** 함수이다.

```jsx
const arrow = (x, y) => { ... };
```

화살표 함수의 매개변수가 딱 한 개라면 소괄호(`()`)를 생략할 수 있다.

```jsx
const arrow = x => { ... };
```

함수 몸체를 감싸는 중괄호(`{}`)를 생략할 수 있다.

이 때, 내부의 문이 값으로 평가될 수 있는 표현식이라면 암묵적으로 반환된다.

```jsx
const arrow = (x, y) => x * y;

multiply(2, 3); // 6
```

중괄호를 생략했을 때, 내부의 문이 값으로 평가될 수 있는 표현식이 아니라면 에러가 발생한다.

```jsx
const arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'

// 위 표현은 다음과 같이 해석된다.
const arrow = () => { return const x = 1; };
```

화살표 함수가 객체 리터럴을 반환하는 경우, 객체 리터럴을 소괄호로 감싸주어야 한다.

```jsx
const create = (id, content) => ({ id, content });
create(1, 'JavaScript'); // -> {id: 1, content: "JavaScript"}

// 위 표현은 다음과 동일하다.
const create = (id, content) => { return { id, content }; };
```

소괄호로 감싸지 않을 경우 객체 리터럴의 중괄호를 함수 몸체로 감싸는 중괄호로 잘못 인식하게 된다.

```jsx
// { id, content }를 함수 몸체 내의 쉼표 연산자문으로 해석한다.
const create = (id, content) => { id, content };
create(1, 'JavaScript'); // -> undefined
```

화살표 함수는 `this`, `arguments`, `super`, `new.target` 바인딩을 갖지 않는다.

만약 화살표 함수가 중첩 함수일 때 위의 바인딩 키워드를 참조하면, 스코프 체인 상에서 가장 가까운 상위 함수의 바인딩을 참조한다.

<br />

## 🤔궁금한 점

## 📌중요한 점