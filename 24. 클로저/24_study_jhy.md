## 클로저

- 클로저는 자바스크립트 고유의 개념이 아니다
- 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

### 24.1 랙시컬 스코프

렉시컬 스코프(정적 스코프)란?

- 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다.

함수는 어디서 호출하는지는 함수의 상위 스코프 결정에 어떠한 영향도 주지 못한다.
즉, 함수의 상위 스코프는 함수를 전의한 위치에 의해 정적으로 결정되고 변하지 않는다.

스코프는 실행 컨텍스트의 렉시컬 환경

스코프 체인이란?

- 렉시컬 환경은 자신의 "외부 렉시컬 환경에 대한 참조"를 통해 상위 렉시컬 환경과 연결

함수의 상위 스코프를 결정한다 == 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다.

-> 렉시컬 스코프 : 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다.

### 24.2 함수 객체의 내부 슬롯 [[Environment]]

함수는 자신의 내부 슬롯[[Environment]]에 자신이 정의 된 환경, 상위 스코프의 참조를 저장한다.
상위 스코프의 참조는 현재 실행중인 실행 컨텍스트의 렉시컬 환결을 가리킨다.

함수 객체는 내부 슬롯[[Environment]]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.

```javascript
const x = 1;

function foo() {
  const x = 10;

  // 상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
  // 함수 호출 위치와 상위 스코프는 아무런 관계가 없다.
  bar();
}

// 함수 bar는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Environment]]에 저장하여 기억한다.
function bar() {
  console.log(x);
}

foo(); // ?
bar(); // ?
```

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/63f5acc3-1b82-4608-8048-bd21b738cede)

### 24.3 클로저와 렉시컬 환경

```javascript
const x = 1;

// ①
function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  }; // ②
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```

외부함수 보다 중첩함수가 더 오래 유지되는 경우 중첩함수는 이미 생명주기가 종료한 외부함수의 변수를 참조 할 수 있다.
이러한 중첩 함수를 클로저 라고 부른다.

클로저 = 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

그 함수가 선언된 렉시컬 환경 = 상위 스코프를 의미하는 실행 컨텍스트의 렉시컬 환경
![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/2eb6604a-d1b5-4336-a3d5-ac2273751829)

inner
![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/fc0d25c0-9aa8-4a26-996f-fd62c1cbee5f)

outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer함수의 렉시컬 환경까지 소멸하는 것은 아니다.

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/bc5bac0b-356a-460f-b674-a4f04eaea54d)

서로 참조되는 상황이라서 가비지 컬렉터의 대상이 되지 않는다.

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/bc88c481-e4dc-4739-bf05-bc7993e841e7)
자신이 정의된 위치에 의해 결정된 상위 스코프를 기억한다.

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로는 모든 함수는 클로저다.

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      function foo() {
        const x = 1;
        const y = 2;

        // 일반적으로 클로저라고 하지 않는다.
        function bar() {
          const z = 3;

          debugger;
          // 상위 스코프의 식별자를 참조하지 않는다.
          console.log(z);
        }

        return bar;
      }

      const bar = foo();
      bar();
    </script>
  </body>
</html>
```

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/096f4263-e716-49a2-a6f2-9fd4fb1f823b)

중첩함수 bar는 외부함수 foo보다 더 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않는다.
따라서 bar함수는 클로저라고 할 수 없다.

다른 예제를 브라우저에서 디버깅 모드로 실행해보자.

#### 24-07

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      function foo() {
        const x = 1;

        // 일반적으로 클로저라고 하지 않는다.
        // bar 함수는 클로저였지만 곧바로 소멸한다.
        function bar() {
          debugger;
          // 상위 스코프의 식별자를 참조한다.
          console.log(x);
        }
        bar();
      }

      foo();
    </script>
  </body>
</html>
```

![image](https://github.com/HYHYJ/mts-study/assets/101866872/6bf1e1a4-a16d-4e33-94a8-17b203e40bce)

중첩함수 bar는 클로저였지만 외부함수보다 일찍 소멸되기 때문에 생명주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합하지 않는다. 그래서 일반적으로 클로저라고 하지않는다.

[다른예제]

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      function foo() {
        const x = 1;
        const y = 2;

        // 클로저
        // 중첩 함수 bar는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자를 참조한다.
        function bar() {
          debugger;
          console.log(x);
        }
        return bar;
      }

      const bar = foo();
      bar();
    </script>
  </body>
</html>
```

클러저는 중첩함수가 상휘 스코프의 식별자를 참조하고 있고 중첩함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.

자유변수: 클로저에 의해 참조되는 상위 스코프의 변수
클로저: 자유 변수에 묶여있는 함수

- 클로저는 상위 스코프의 식별자중에서 기억해야할 식별자만 기억한다.

### 24.4 클로저의 활용

클로저는 상태를 안전하게 변경하고 유지하기 위해 사용

- 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용

#### 24-09 변수가 전역이라서 좋지 않다.

```javascript
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태를 1만큼 증가 시킨다.
  return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

#### 24-10 계속 변수가 재선언되어 0으로 초기화된다.

```javascript
// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태 변수
  let num = 0;

  // 카운트 상태를 1만큼 증가 시킨다.
  return ++num;
};

// 이전 상태를 유지하지 못한다.
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```

#### 24-11 이전 상태를 유지할수 있는 클로저 사용!!

```javascript
// 카운트 상태 변경 함수
const increase = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저
  return function () {
    // 카운트 상태를 1만큼 증가 시킨다.
    return ++num;
  };
})();

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

클로저 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.

[함수형 프로그래밍에서 클로저를 활용하는 예제]

#### 24-14

```javascript
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(aux) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 클로저를 반환
  return function () {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다
const increaser = makeCounter(increase); // ①
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease); // ②
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

makeCounter함수를 호출해 함수를 반환할때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다.
![image](https://github.com/HYHYJ/mts-study/assets/101866872/1b2540d9-bfcc-427d-b0cf-efb027139a07)
![image](https://github.com/HYHYJ/mts-study/assets/101866872/e3ba9fc6-28c1-4e43-800e-72bb51c2de06)

전역 함수는 독립된 렉시컬 환경을 갖지 때문에 카운트를 유지하기위한 자유변수를 공유하지 않는다.
렉시컬 환경을 공유하는 클로저 만들기!

#### 24-15

⭐위에 코드는 전역 변수 increaser와 decreaser에 할당된 함수는 각각 독립된 렉시컬 환경을 갖기 때문에 카운트를 유지하기 위한 자유변수 counter를 공유하지 않아 카운터 증감이 연동 xx

⏩따라서 렉시컬 환경을 공유하는 클로저를 만들어야한다.
MakeCounter함수를 두번 호출하지 말아야한다.

```javascript
// 함수를 반환하는 고차 함수(즉시실행함수를 변수에 담는다)
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
const counter = (function () {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 함수를 인수로 전달받는 클로저를 반환
  return function (aux) {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
})();

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 보조 함수를 전달하여 호출
console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유 변수를 공유한다.
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```

### 24.5 캡슐화와 정보 은닉

캡슐화

- 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.

정보은닉

- 특정 프로퍼티나 메서드를 감추도록 사용하는 캡슐화

⭐
자바스크립트는 정보 은닉을 완전하게 지원하지 않는다.

에서드를 클로저안에 Peson.prototype.sayHi 메서드는 한번만 생성되서 여러개의 인스턴스를 생성할 경우 변수의 상태가 유지 되지않는다.

### 24.6 자주 발생하는 실수

클로저를 사용할 때 자주 발생할 수 있는 실수

#### 24-20

i변수는 함수 레벨 스코프를 갖기때문에 전역변수다. 전역 변수 i에는 0,1,2가 순차적으로 할당된다. 따라서 funcs 배열의 요소로 추가한 함수를 호출하면 전역 변수 i 를 참조하여 i 값 3이 출력된다.

```javascript
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i;
  }; // ①
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // ②
}
```

#### 24-21

⭐⭐⭐⭐

```javascript
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = (function (id) {
    // ①
    return function () {
      return id;
    };
  })(i);
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
```

#### 24-22

let 키워드로 선언한 변수를 사용하면 for문의 코드 블록이 반복 실행될 때마다 for문 코드 블록의 새로운 렉시컬 환경이 생성된다.

이때, 함수의 상위 스코프는 for문의 코드 블록이 반복 실행될 때마다 식별자(for의 변수 선언문에서 선언한 초기화 변수 및 for문의 코드블록 내에서 선언한 지역변수) 값을 유지해야한다.
for문이 반복될때마다 독립적인 렉시컬 환경을 생성하여 식별자의 값을 유지한다.

```javascript
const funcs = [];

for (let i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i;
  };
}

for (let i = 0; i < funcs.length; i++) {
  console.log(funcs[i]()); // 0 1 2
}
```

![image](https://github.com/HYHYJ/Algorithm-solving-with-js/assets/101866872/cc849a32-a03d-4fb7-b63c-961f110917b9)

- 새로운 렉시컬 환경을 생성하고 for문 코드 블록 내의 식별자와 값을 등록한다.
- 새롭게 생성된 렉시컬 환경을 현재 실행중인 실행 컨텍스트의 렉시컬 환경으로 교체한다.
- for문의 코드 블록의 반복 실행이 끝나면 for문이 실행되기 이전의 렉시컬 환경을 실행 중인 실행 컨텍스트이 렉시컬 환겨으로 되돌린다.

#### 24-23

-> 함수형 프로그래밍 기법인 고차함수를 사용하는 방법

```javascript
// 요소가 3개인 배열을 생성하고 배열의 인덱스를 반환하는 함수를 요소로 추가한다.
// 배열의 요소로 추가된 함수들은 모두 클로저다.
const funcs = Array.from(new Array(3), (_, i) => () => i); // (3) [ƒ, ƒ, ƒ]

// 배열의 요소로 추가된 함수 들을 순차적으로 호출한다.
funcs.forEach((f) => console.log(f())); // 0 1 2
```
