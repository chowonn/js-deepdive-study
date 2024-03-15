## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 24.1 렉시컬 스코프

렉시컬 스코프 : 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정

```javascript
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // ?
bar(); // ?
```

- 함수를 어디서 호출하는지는 함수의 상위 스코프 결정에 어떠한 영향도 주지 못함
- 렉시컬 환경에서 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정

## 24.2 함수 객체의 내부 슬롯

렉시컬 스코프가 가능하려면 함수 정의가 평가되어 함수 객체를 생성할 때 자신이 정의된 환경(위치)에 의해 결정된 상위 스코프의 참조를 함수 객체 자신의 내부 슬롯에 저장

이 때 자신의 내부 슬롯에 저장된 상위 스코프의 참조는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킴

따라서 함수 객체의 내부 슬롯에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프

또한 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장될 참조값

함수 객체는 내부 슬롯에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억함

## 24.3 클로저와 렉시컬 환경

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

- outer 함수가 평가되어 함수 객체를 생성할 때(①)
  - 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 전역 렉시컬 환경을 outer 함수의 [[Environment]] 내부 슬롯에 상위 스코프로서 저장
- outer 함수를 호출
  - outer 함수의 렉시컬 환경이 생성되고 앞서 outer 함수 객체의 [[Environment]] 내부 슬롯에 저장된 전역 렉시컬 환경을 outer 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 할당
- 중첩 함수 inner 평가(② inner 함수는 함수 표현식으로 정의했기 때문에 런타임에 평가)
  - 중첩 함수 inner는 자신의 [[Environment]] 내부 슬롯에 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 outer 함수의 렉시컬 환경을 상위 스코프로서 저장
- outer 함수의 실행이 종료(③)
  - inner 함수를 반환하면서 outer 함수의 생명 주기가 종료
  - outer 함수의 실행 컨텍스트가 실행 컨텍스트 스택에서 제거, 이 때 outer 함수의 렉시컬 환경까지 소멸하진 않음
  - outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있고 inner 함수는 전역변수 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않음
- outer 함수가 반환한 inner 함수를 호출(④)
  - inner 함수의 실행 컨텍스트가 생성되고실행 컨텍스트 스택에 푸시
  - 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 [[Environment]] 내부 슬롯에 저장되어 있는 참조값이 할당

중첩 함수 inner는 외부 함수 outer보다 더 오래 생존

이때 외부 함수보다 더 오래 생존한 중첩 함수는 외부 함수의 생존 여부(실행 컨텍스트의 생존 여부)와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억

이처럼 중첩 함수 inner의 내부에서는 상위 스코프를 참조할 수 있으므로 상위 스코프의 식별자를 참조할 수 있고 식별자의 값을 변경할 수도 있음

이러한 중첩 함수를 클로저라고 부름

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

- 위 예제의 중첩 함수 bar는 외부 함수 foo보다 더 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않음
- 이처럼 상위 스코프의 어떤 식별자도 참조하지 않는 경우 대부분의 모던 브라우저는 상위 스코프를 기억하지 않음
- 따라서 bar 함수는 클로저라고 할 수 없음

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

- 중첩 함수 bar는 상위 스코프의 식별자를 참조하고 있지만 외부함수 foo의 외부로 중첩 함수 bar가 반환되지 않음
- 이런 경우 중첩 함수 bar는 클로저였지만 외부 함수보다 일찍 소멸되기 때문에 생명 주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합하지 않음

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

- 중첩 함수 bar는 상위 스코프의 식별자를 참조하고 외부 함수의 외부로 반환되어 외부 함수보다 더 오래 살아남음
- 이처럼 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명주기가 종료된 외부 함수의 변수를 참조할 수 있음
- 이러한 중첩 함수를 클로저라고 부름
- 클로저에 의해 참조되는 상위 스코프의 변수(foo 함수의 x 변수)를 자유 변수라고 부름

## 24.4 클로저의 활용

클로저는 상태를 안전하게 변경하고 유지하기 위해 사용

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

- 카운트 상태가 전역 변수를 통해 관리되고 있기 때문에 언제든지 누구나 접근할 수 있고 변경할 수 있음
- 의도치 않게 상태가 변경될 수 있다는 것(전역 변수 num의 값이 변경될 수 있음)

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

- 전역 변수 num을 increase 함수의 지역 변수로 변경하여 의도치 않은 상태 변경 방지
- 하지만 increase 함수가 호출될 때마다 지역 변수 num은 다시 선언되고 0으로 초기화되기 때문에 출력 결과는 언제나 1임

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

- 코드가 실행되면 즉시 실행 함수가 호출되고 즉시 실행 함수가 반환한 함수가 increase 변수에 할당
- 즉시 실행 함수가 반환한 클로저는 카운트 상태를 유지하기 위한 자유 변수 num을 언제 어디서 호출하든지 참조하고 변경할 수 있음
- 즉시 실행 함수는 한 번만 실행되므로 increase가 호출될 때마다 num 변수가 재차 초기회되지 않음
- 또한 num 변수는 외부에서 직접 접근할 수 없는 은닉된 private 변수이므로 전역 변수를 사용했을 때와 같이 의도되지 않은 변경을 걱정할 필요가 없음

이처럼 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 혀용하여 상태를 안전하게 변경하고 유지하기 위해 사용

## 24.5 캡슐화와 정보 은닉

```javascript
function Person(name, age) {
  this.name = name; // public
  let _age = age; // private

  // 인스턴스 메서드
  this.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };
}

const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person("Kim", 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

- name 프로퍼티는 외부로 공개되어 있어서 자유롭게 참조하거나 변경할 수 있음(public)
- \_age 변수는 Person 생성자 함수의 지역 변수이므로 Person 생성자 함수 외부에서 참조하거나 변경할 수 없음(private)
- 하지만 sayHi 메서드는 인스턴스 메서드이므로 Person 객체가 생성될 때마다 중복 생성됨

```javascript
function Person(name, age) {
  this.name = name; // public
  let _age = age; // private
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  // Person 생성자 함수의 지역 변수 _age를 참조할 수 없다
  console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
};
```

- sayHi 메서드를 프로토타입 메서드로 변경하여 sayHi 메서드의 중복 생성을 방지
- 이때 `Person.prototype.sayHi` 메서드 내에서 Person 생성자 함수의 지역 변수 \_age를 참조할 수 없는 문제 발생

```javascript
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person("Kim", 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

- Person 생성자 함수와 `Person.prototype.sayHi` 메서드를 하나의 함수에 모음
- 즉시 실행 함수가 반환하는 Person 생성자 함수와 Person 생성자 함수의 인스턴스가 상속받아 호출할 `Person.prototype.sayHi` 메서드는 즉시 실행 함수가 종료된 이후 호출
- 하지만 Person 생성자 함수와 sayHi 메서드는 이미 종료되어 소멸한 즉시 실행 함수의 지역 변수 \_age를 참조할 수 있는 클로저
- 이렇게 하면 public, private, protected 같은 접근 제한자를 제공하지 않는 자바스크립트에서도 정보 은닉이 가능한 것처럼 보임
- 하지만 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 다음과 같이 \_age 변수의 상태가 유지되지 않는다는 문제가 있음

```javascript
const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person("Kim", 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.
```

- 이는 `Person.prototype.sayHi` 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상
- `Person.prototype.sayHi` 메서드는 즉시 실행 함수가 호출될 때 생성되고 자신의 상위 스코프인 즉시 실행 함수의 실행 컨텍스트의 렉시컬 환경 참조를 [[Environment]]에 저장하며 기억
- 따라서 Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 `Person.prototype.sayHi` 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 동일한 상위 스코프를 사용
- 이러한 이유로 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 위와 같이 \_age 변수의 상태가 유지되지 않음

최신 브라우저에는 private 필드를 정의할 수 있음

## 24.6 자주 발생하는 실수

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

- func 배열의 요소로 추가된 3개의 함수가 0, 1, 2를 반환하지 않고 3만 세 번 반환
- for 문의 변수 선언문에서 var 키워드로 선언한 i 변수는 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수
- 전역 변수 i에 0, 1, 2가 순차적으로 할당되고 func 배열의 요소로 추가한 함수를 호출하면 전역 변수 i를 참조하여 i의 값 3이 출력

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

- 클로저를 사용해 올바르게 작동하는 코드로 변환
- ①에서 즉시 실행 함수는 전역 변수 i에 할당되어 있는 값을 인수로 전달받아 매개변수 id에 할당한 후 중첩 함수를 반환하고 종료
- 즉시 실행 함수가 반환한 함수는 funcs 배열에 순차적으로 저장
- 즉시 실행 함수가 반환한 중첩 함수는 자신의 상위 스코프를 기억하는 클로저이고, 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수에 묶여있는 자유 변수가 되어 그 값이 유지

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

- let 키워드로 선언한 변수를 사용하면 for 문의 코드 블록이 반복 실행될 때마다 for 문 코드 블록의 새로운 렉시컬 환경이 생성되기 때문에 이 같은 번거로움이 해결
- 함수의 상위 스코프는 for 문의 코드 블록이 반복 실행될 때마다 식별자(for 문의 변수 선언문에서 선언한 초기화 변수 및 for 문의 코드 블록 내에서 선언한 지역 변수 등)의 값을 유지해야 함
- 이를 위해 for 문이 반복될 때마다 독립적인 렉시컬 환경을 생성하여 식별자의 값을 유지

## 🤔궁금한 점

## 📌중요한 점
