# 📗챕터 정리

## ❔ 심볼 (Symbol)

심볼은 자바스크립트 ES6 버전에서 새로 추가된 데이터 유형이다.

심볼은 변경 불가능한 데이터이며, 다른 값과 중복되지 않는 유일무이한 값이다.

때문에 주로 충돌 위험 없이 고유한 프로퍼티 키를 생성할 때 사용하게 된다.

심볼은 `new` 키워드 없이 `Symbol` 함수만 사용하여 생성할 수 있다.

생성한 심볼의 값은 외부에 노출되지 않는다.

```jsx
const exSymbol = Symbol();

console.log(exSymbol); // Symbol()
console.log(typeof exSymbol); // symbol
```

심볼의 소괄호 내부에는 심볼에 대한 설명인 심볼 키를 인수로 전달할 수 있다.

심볼 키는 문자열로 작성되어야 하며, 심벌 값 자체에는 어떠한 영향도 주지 않는다.

심볼 키가 같은 두 심볼을 비교하더라도 각 심볼은 고유하기 때문에 같지 않으며, `false`가 출력된다.

```jsx
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false
```

### Symbol.for / Symbol.keyFor메서드

`Symbol.for` 메서드는 심볼이 저장된 레지스트리에서 메서드의 인수와 심볼 키가 일치하는 심벌을 검색한다.

이후, 검색에 성공하면 검색된 심볼을 반환한다.

검색에 실패하면 메서드의 인수로 전달된 값을 심볼 키로 하여 새로운 심볼을 생성한다. 

```jsx
const s1 = Symbol.for('mySymbol'); // 검색 실패 -> 새로운 심볼 생성하여 s1 변수에 할당
const s2 = Symbol.for('mySymbol'); // 검색 성공 -> 심볼을 s2 변수에 할당

console.log(s1 === s2); // true
```

`Symbol.keyFor` 메서드는 전역 심볼 레지스트리에서 심볼 키를 추출할 때 사용한다.

⚠️ 이 때, Symbol 함수로 생성된 심볼은 전역 심볼 레지스트리에 심볼 키를 저장할 수 없다.

때문에 Symbol 함수로 생성된 심볼은 `Symbol.keyFor` 메서드를 사용하여 심볼 키를 추출할 수 없다.

**반면**, `Symbol.for` 메서드로 생성된 심볼은 전역 심볼 레지스트리를 통해 심볼 키를 저장할 수 있다.

**유일무이한 심볼 값을 단 하나만 생성하고자 한다면 `Symbol.for` 메서드를 사용해야 한다.**

```jsx
const s1 = Symbol.for('mySymbol');
Symbol.keyFor(s1); // -> mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
Symbol.keyFor(s2); // -> undefined
```

---

## 심벌 사용 예시

### 1️⃣ 상수 정의

4방향, 즉 상하좌우를 나타내는 상수를 정의한다고 생각해보자.

```jsx
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4
};

// 변수에 상수를 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

Direction 객체의 프로퍼티 값인 1~4 숫자 값 자체에는 아무런 의미도 존재하지 않는다.

또한, 1~4 숫자 값은 다른 값과 중복되거나 값 자체가 변경될 가능성도 없지 않다.

이러한 경우에 값에 고유한 의미를 부여하고, 변경할 수 없게 하기 위해 심볼 값을 사용할 수 있다.

```jsx
const Direction = {
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right')
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

### 2️⃣ 프로퍼티 키 대체 / 프로퍼티 은닉

위 경우에서는 프로퍼티의 값에 심볼을 할당하였다.

심볼은 프로퍼티의 값 뿐만 아니라 프로퍼티 키에도 할당할 수 있다.

심볼 값을 프로퍼티 키로 사용하고 호출할 때는 심볼 값을 대괄호로 감싸주어야 한다.

```jsx
const obj = {
  [Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')]; // 1
```

프로퍼티 키를 심볼 값으로 지정한 프로퍼티는 `for … in`, `Object.keys`, `Object.getOwnPropertyNames` 메서드로 접근할 수 없다.

따라서 외부에 노출할 필요가 없는 프로퍼티를 은닉하기 위해 심볼을 사용할 수 있다.

```jsx
const obj = {
  [Symbol('mySymbol')]: 1
};

for (const key in obj) {
  console.log(key); // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)); // []

console.log(Object.getOwnPropertyNames(obj)); // []
```

### 3️⃣ 하위 호환성 보장 (표준 빌트인 객체의 확장 등의 경우)

개발자의 편의를 위한 메서드 추가, 새로운 라이브러리 개발 등의 이유로 빌트인 객체의 확장이 필요할 수 있다.

이러한 경우에 새로운 버전에서 등장할 메서드와 이름이 중복되어 덮어쓰게 될 우려가 있다.

~~때문에 일반적으로 빌트인 객체는 확장을 권장하지 않는다.~~

이 때 심볼 값으로 프로퍼티 키를 생성하여 빌트인 객체를 확장하면 중복 문제를 방지할 수 있다.

즉, 심볼 값으로 프로퍼티 키를 생성하면 하위 호환성이 보장된다.

```jsx
// 표준 빌트인 객체를 확장하는 것은 권장하지 않는다.
Array.prototype.sum = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2].sum(); // -> 3

// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')](); // -> 3
```

---

## Well-known Symbol

자바스크립트에는 기본적으로 제공되는 빌트인 심볼이 존재하며 이것을 *Well-known Symbol* 라고 칭한다.

*Well-known Symbol* 은 Symbol 함수의 프로퍼티에 할당되어 있다.

이것은 자바스크립트 엔진의 내부 알고리즘에 사용된다.

예를 들어, 다음 챕터에서 다룰 빌트인 이터러블(`for … of` 문으로 순회 가능한 객체)은 *Well-known Symbol* 중 `Symbol.iterator` 심볼을 키로 갖는 메서드를 가진다.

`Symbol.iterator` 메서드를 호출하면 이터레이터를 반환하도록 ECMASciprt 사양에 규정되어 있다.

만약 일반 객체를 이터러블 객체로서 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 된다.

먼저, `Symbol.iterator`을 키로 갖는 메서드를 객체에 추가하고 이터레이블을 반환하도록 구현하면 그 객체는 이터러블 객체가 된다.

이 때, 일반 객체에 추가해야 하는 메서드의 키 `Symbol.iterator`는 기존 프로퍼티 키 혹은 나중에 추가될 프로퍼티 키와 절대로 중복되지 않을 것이다.

```jsx
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      }
    };
  }
};

for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```

<br />

## 🤔궁금한 점

## 📌중요한 점