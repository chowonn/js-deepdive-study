# 📗챕터 정리

## 이터레이션 프로토콜

순회 가능한 데이터 컬렉션을 만들기 위해 ECMAScirpt 사양에 정의한 규정이다.
이터레이션 프로토콜에는 1️⃣ 이터러블 프로토콜, 2️⃣ 이터레이터 프로토콜이 존재한다.

### 1️⃣ 이터러블 프로토콜 (Iterable Protocol)

`Symbol.iterator` 심볼을 프로퍼티 키로 사용한 메서드를 구현하거나, 프로토타입 체인으로 상속받은 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.
**이터러블은 `for ... of` 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.**

객체가 이터러블인지 확인하는 코드는 다음과 같다.

```jsx
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';
```

배열, 문자열, Map, Set 은 이터러블 프로토콜을 준수하는 이터러블 객체이다.
위 객체들은 후술할 빌트인 이터러블로 구분하기도 한다.

```jsx
isIterable([]);        // -> true
isIterable('');        // -> true
isIterable(new Map()); // -> true
isIterable(new Set()); // -> true
isIterable({});        // -> false
```

위에서 언급했듯이 이 객체들은 **`for ... of` 문으로 순회가 가능하다.**
또한 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.

```jsx
const array = [1, 2, 3];

// 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in array); // true

// 이터러블인 배열은 for...of 문으로 순회 가능하다.
for (const item of array) {
  console.log(item); // 1 2 3
}

// 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있다.
console.log([...array]); // [1, 2, 3]

// 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```

### 2️⃣ 이터레이터 프로토콜 (Iterator Protocol)

이터러블의 `Symbol.iterator` 메서드를 호출하면 이터레이터 프로토콜을 준수한 **이터레이터**를 반환한다.
이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.

이터레이터는 `next` 메서드를 소유한다.
`next` 메서드는 이터러블을 순회하며 `value`와 `done` 프로퍼티를 갖는 **이터레이터 리절트 객체**를 반환한다.
이터레이터 리절트 객체 내의 `value`와 `done` 프로퍼티는 각각 이터러블의 값과 순환 완료 여부를 의미한다.

```jsx
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환한다. 이터레이터는 next 메서드를 갖는다.
const iterator = array[Symbol.iterator]();

// next 메서드를 호출하면 이터러블을 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를
// 반환한다. 이터레이터 리절트 객체는 value와 done 프로퍼티를 갖는 객체다.
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

---

## ❔ for … of 문

for ... of 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다. 
사용법은 for ... in 문의 형식과 매우 유사하다.

```jsx
for (const item of [1, 2, 3]) {
  // item 변수에 순차적으로 1, 2, 3이 할당된다.
  console.log(item); // 1 2 3
}
```

### for … in / for … of 비교

`for ... in` 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티를 확인한다.
확인한 프로퍼티 중, 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 `true`인 프로퍼티를 순회하며 열거한다.
(이 때, 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.)

`for ... of` 문은 내부적으로 이터레이터의 `next` 메서드를 호출하여 이터러블을 순회한다.
`next` 메서드가 반환한 이터레이터 리절트 객체의 `value` 프로퍼티 값을 `for ... of` 문의 변수에 할당한다.
이후 이터레이터 리절드 객체의 `done` 프로퍼티 값이 `false` 이면 이터러블의 순회를 계속하고 `true`이면 이터러블의 순회를 중단한다.

---

## 이터러블과 유사 배열 객체

유사 배열 객체는 마치 배열처럼 인덱스 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 뜻한다.
유사 배열 객체는 `length` 프로퍼티를 갖기 때문에 `for` 문으로 순회할 수 있다.
인덱스(Index)를 나타내는 숫자 형식의 문자열 프로퍼티 키로 가지므로 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.

```jsx
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3
};

for (let i = 0; i < arrayLike.length; i++) {
  console.log(arrayLike[i]); // 1 2 3
}
```

그런데 유사 배열은 실제로 배열이 아닌 객체이며 이터러블이 아니다.
이터러블이 아니기에 내부에 `Symbol.iterator` 메서드가 존재하지 않으며, `for … of` 문도 사용할 수 없다.

```jsx
for (const item of arrayLike) {
  console.log(item); // -> TypeError: arrayLike is not iterable
}
```

단, arguments, NodeList, HTMLCollection은 유사 배열 객체이면서 이터러블이다.
정확히는 ES6에서 이터러블이 도입되면서 유사 배열 객체인 arguments, NodeList, HTMLCollection 객체에 `Symbol.iterator` 메서드를 구현하여 이터러블이 되었다.
하지만 이터러블이 된 이후에도 `length` 프로퍼티를 가지며 인덱스로 접근할 수 있는 것에는 변함이 없으므로 유사 배열 객체이면서 이터러블인 것이다.

배열도 마찬가지로 ES6에서 이터러블이 도입되면서 `Symbol.iterator` 메서드를 구현하여 이터러블이 되었다.
하지만 모든 유사 배열 객체가 이터러블인 것은 아니다.
위 예제의 arrayLike 객체는 유사 배열 객체지만 이터러블이 아니다.
다만 ES6에서 도입된 `Array.from` 메서드를 사용하여 배열로 간단히 변환할 수 있다.
`Array.from` 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달 받아 배열로 변환하여 반환한다.

---

## 이터레이션 프로토콜의 필요성

`for ... of` 문, 스프레드 문법, 배열 디스트럭처링 할당은 모두 이터레이션 프로토콜을 준수하는 이터러블만 사용할 수 있다.
즉, 이터러블은 `for ... of` 문, 스프레드 문법, 배열 디스트럭처링 할당과 같은 데이터 소비자에 의해 사용되므로 데이터 공급자의 역할을 한다고 할 수 있다.

만약 다양한 데이터 공급자가 각자의 순회 방식을 갖는다면 데이터 소비자는 다양한 데이터 공급자의 순회 방식을 모두 지원해야 하는데, 이는 비효율적이다.
다양한 데이터 공급자가 이터레이션 프로토콜을 준수하도록 규정하면 데이터 소비자는 이터레이션 프로토콜만 지원하도록 구현하면 되기 때문이다.

이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정한다.
그리고 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 **데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할을 한다.**

---

## 사용자 정의 이터러블 구현

일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다.

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수한다.
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1];
    const max = 10; // 수열의 최대값

    // next 메서드는 이터레이터 리절트 객체를 반환한다.
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        // 이터레이터 리절트 객체를 반환한다.
        return { value: cur, done: cur >= max };
      },
    }
  },
};

// 이터러블인 fibonacci 객체를 순회할 때마다 next 메서드가 호출한다.
for (const num of fibonacci) {
  console.log(num); // 1 2 3 5 8
}
```

## 이터러블을 생성하는 함수

수열의 최댓값을 외부에서 전달할 수 있도록 수정해보자.
수열의 최댓값을 인수로 전달 받아 이터러블을 반환하는 함수를 만들면 된다.

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블을 반환하는 함수.
// 수열의 최대값을 인수로 전달받는다.
const fibonacciFunc = function(max) {
  let [pre, cur] = [0, 1];

  // Symbol.iterator 메서드를 구현한 이터러블을 반환한다.
  return {
    [Symbol.iterator]() {
      return {
        next() {
          [pre, cur] = [cur, pre + cur];
          return { value: cur, done: cur >= max };
        },
      }
    },
  }
};

// 이터러블을 반환하는 함수에 수열의 최대값을 인수로 전달하면서 호출한다.
// fibonacciFunc(10)은 이터러블을 반환한다.
for (const num of fibonacciFunc(10)) {
  console.log(num); // 1 2 3 5 8
}
```

## 이터러블이면서 이터레이션 객체를 생성하는 함수

위 예제에서 `fibonacciFunc` 함수는 이터러블을 반환한다.
만약 이터레이터를 생성하려면 이터러블의 `Symbol.iterator` 메서드를 호출해야 한다.

```jsx
const iterable = fibonacciFunc(5);
// 이터러블의 Symbol.iterator 메서드는 이터레이터를 반환한다.
const iterator = iterable[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 5, done: true }
```

이터러블이면서 동시에 이터레이터인 객체를 생성하면 `Symbol.iterator` 메서드를 호출하지 않아도 된다.

```jsx
//이터러블이면서 이터레이터인 객체를  반환하는 함수
const fibonacci = function (max) {
    let [pre, cur] = [0, 1];

    // Symbol.iterator 메서드와 next 메서드를 소유한 이터러블이면서 이터레이터인 객체를 반환
    return {
        [Symbol.iterator]() {
            return this;
        },
        // next 메서드는 이터레이터 리절트 객체를 반환
        next() {
            [pre, cur] = [cur, pre + cur];
            return {value: cur, done: cur >= max};
        }

    }
};

// iter는 이터러블이면서 이터레이터디.
let iter = fibonacci(10);

// iter는 이터러블이므로 for...of 문으로 순회할 수 있다.
for (const num of iter) {
    console.log(num); // 1 2 3 5 8
}

// iter는 이터러블이면서 이터레이터다.
iter = fibonacci(10);

// iter는 이터레이터이므로 이터레이션 리절트 객체를 반환하는 next 메서드르 소유한다.
console.log(iter.next()); // { value: 1, done: false }
console.log(iter.next()); // { value: 2, done: false }
console.log(iter.next()); // { value: 3, done: false }
console.log(iter.next()); // { value: 5, done: false }
console.log(iter.next()); // { value: 8, done: false }
console.log(iter.next()); // { value: 13, done: true }
```

## 무한 이터러블과 지연 평가

무한 이터러블을 생성하는 함수를 정의하며 무한 수열을 간단히 구현할 수 있다.

```jsx
// 무한 이터러블을 생성하는 함수
const fibonacciFunc = function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() { return this; },
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한을 구현해야 하므로 done 프로퍼티를 생략한다.
      return { value: cur };
    }
  };
};

// fibonacciFunc 함수는 무한 이터러블을 생성한다.
for (const num of fibonacciFunc()) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...4181 6765
}

// 배열 디스트럭처링 할당을 통해 무한 이터러블에서 3개의 요소만 취득한다.
const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3); // 1 2 3
```

이터러블은 데이터 공급자의 역할을 한다.
원래 배열이나 문자열 등은 모든 데이터를 메모리에 미리 확보한 다음 데이터를 공급한다.

하지만 위 예제의 이터러블은 **지연 평가**를 통해 데이터를 생성한다.

위 예제의 `fibonacciFunc` 함수는 무한 이터러블을 생성한다.
`for ... of` 문의 경우 **이터러블을 순회할 때 내부에서 이터레이터의 `next` 메서드를 호출하는데 바로 이때 데이터가 생성**된다.
`next` 메서드가 호출되기 이전까지는 데이터를 생성하지 않는다.

지연 평가는 데이터가 필요할 때까지 데이터의 생성을 지연하다가 데이터가 필요한 순간 데이터를 생성한다.
지연 평가를 사용하면 불필요한 데이터를 미리 생성하지 않으며, 필요한 데이터를 필요한 순간에 생성하므로 빠른 실행 속도를 기대할 수 있다.
또한 불필요한 메모리를 소비하지 않으며 무한도 표현할 수 있다는 장점도 존재한다.

<aside>
❓ **지연 평가**

---

### 지연 평가
**지연 평가**는 데이터가 필요한 시점 이전 까지는 미리 데이터를 생성하지 않는다.
이후에 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법이다.
즉, **평가 결과가 필요할 때까지 평가를 늦추는 기법**이 지연 평가다.

</aside>


<br/>

## 🤔궁금한 점

## 📌중요한 점
