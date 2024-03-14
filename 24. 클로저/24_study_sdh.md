## 목차

[📗배운 점](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

### 클로져

> **클로저** : 함수와 함수가 선언됐을 때의 렉시컬 환경(Lexical Environment) 사이의 조합을 나타냅니다.
> 클로저는 함수가 다른 함수 안에서 정의되었을 때 발생합니다. (외계인이 납치한 호랑이)

#### 24.1 렉시컬 스코프

자바스크립트는 상위스코프를 함수가 정의된 곳에서 결정한다. 이를 정적 스코프 라고 한다.

```javascript
const x = 1;

function outerFunc() {
  const x = 10;

  function innerFunc() {
    console.log(x); // 10
  }

  innerFunc();
}

outerFunc();
```

이곳에서는 `outerFunc`안에 `innerFunc`가 정의 되어 있어 상위 스코프의 x의 값은 10이 된다.
하지만

```javascript
const x = 1;

function outerFunc() {
  const x = 10;
  innerFunc();
}

function innerFunc() {
  console.log(x); // 1
}

outerFunc();
```

이곳의 `innerFunc`가 정의 된 곳은 전역이기 때문에 x의 값은 1이 나온다.

#### 24.2 함수 객체의 내부 슬롯

함수 객체의 내부 슬롯에 저장된 현재 실행중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프다.
외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯에 저장된 렉시컬 환경의 참조가 할당된다.

#### 24.3 클로저와 렉시컬 환경

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

`outer`함수를 호출(3) 하면 `outer`함수는 `inner`를 반환하고 생명주기를 마감한다.
하지만 (4)의 실행 결과는 `outer`의 지역변수값인 10이다

이처럼 외부 함수보다 중첩함수가 더 오래 유지되는 경우 중첩함수는 이미 생명주기가 종료된 외부 함수의 변수를 다시 참조할 수 있다.
이러한 중첩 함수를 클로저 라고 부른다.

위 예제에서 inner 함수는 자신이 평가될 때 자신이 정의된 위치에 의해 결정된 상위 스코프를 `[[Environment]]` 내부 슬롯에 저장한다. 이때 저장된 상위 스코프는함수가 존재하는 한 유지된다.

위 예제에서 **`outer`함수가 평가**되어 함수 객체를 생성할 때(1) 전역 렉시컬 환경을 `outer` 함수 객체의 `[[Environment]]`내부 슬롯에 상위 스코프로서 저장한다.

**`outer`함수를 호출**하면 `outer`함수의 렉시컬 환경이 생성되고 앞서 `outer`함수 객체의 `[[Environment]]`내부 슬롯에 저장된 전역 렉시컬 환경을 `outer` 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 할당한다.

그리고 중첩 함수 **`inner` 함수가 평가**된다. 아땨 중첩함수 `inner`는 자신의 `[[Environment]]`내부 슬롯에 `outer` 함수의 렉시컬 환경을 상위 스코프로서 저장한다.

**`outer` 함수의 실행이 종료**되면 `inner`함수를 반환하면서 `outer`함수의 생명주기가 종료된다(3)
이때 `outer`함수의 실행 컨텍스트는 실행 컨텍스트 스텍에서 제거되지만 <U>`outer`함수의 렉시컬 환경까지 소멸 하는것은 아니다.</U>

**`outer`함수의 렉시컬 환경은 inner함수의 `[[Environment]]` 내부 슬롯에 의해 참조되고 있고 `inner`함수는 전역변수 `innerFunc에 의해 참조되고 있으므로 사비지 컬렉션의 대상이 되지 않기 때문이다.**

`outer`가 반환한 **`inner`함수를 호출**(4)하면 `inner`의 실행 컨덱스트가 생성되고 실행 컨텍스트에 푸시된다.렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 `inner` 함수 객체의 `[[Enironment]]` 내부 슬롯에 저장되어 있는 참조 값이 할당된다.

\* 상위 스코프의 어떠한 식별자도 참조하지 않는 함수는 클로저가 아니다. \* 외부함수보다 중첩함수가 일찍 소멸 되는것은 클로저 본질에 부합하지 않는다. \* **클로저는 중첩함수가 상위 스코프의 식별자를 참조하고 있고 중첩함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는것이 일반적이다.**

#### 24.4 클로저의 활용

> 클로저 : 클로져는 상태를 안전하게 변경하고 유지하기 위해 사용한다. 다시 말해 상태를 안전하게 은닉하고 특정 함수에게만 상태변경을 허용하기 위해 사용된다.

함수가 호출 될때마다 횟수를 누적하는 카운트의 예시이다.

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

위 코드는 잘 작동하지만 오류를 발생시킬 수 있다.

1. num은 함수가 호출 되기 전까지 변경되지 않고 유지 되어야 한다.
2. 이를 위해 num의 값은 함수만이 변경할 수 있어야 한다.

위 내용을 위해 num을 함수 안쪽으로 넣어 보았다.

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

num이 지역변수가 되어 버려 상태를 유지 하지 못한다.

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

위와 같이 변경 하면 위 코드가 실행될때 즉시 실행함수가 호출되고 즉시 실행함수가 반환한 함수가 `increase`변수에 할당된다. `increase` 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 즉시 실행 함수의 렉시컬 환경을 기억하는 클로져이다.

#### 24.5 캡슐화와 정보 은닉.

캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 종작인 메서드를 하나로 묶는것을 말한다.

자바스크립트는 `public`,`privete`,`protected`와 같은 접근 제한자를 제공하지 않고 기본적으로 `public`하다.

```javascript
function Person(name, age) {
  this.name = name; // public
  let _age = age; // private

  // 인스턴스 메서드
  this.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };
}
```

여기서 `name` 프로퍼티는 외부로 공개 되어 있어 자유롭게 참조하거나 변경할 수 있다.
즉 public하다 하지만 `_age` 변수는 person 생성자 함수의 지역변수 이므로 생성자 함수 외부에서 참조하거나 변경할 수 없다. 즉 `_age`는 `private`하다.

하지만 위 예제의 `sayHi`메서드는 인스턴스 메서드 이므로 `Person` 객체가 생성될 때마다 중복 생성된다.

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

이렇게 하면 프로토타입 메서드가 `_age`를 참조 할 수 없게 된다.

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
```

이렇게 하면 접근 제한자 없이도 정보 은닉이 가능한 것 처럼 보인다.
하지만 person 생성자 함수가 여러개의 인스턴스를 생성할 경우 `_age`변수 상태가 유지되지 않는다는 것이다.
이는 Person.prototype.sayHi가 단 한번 생성되는 클로저 이기 때문에 발생하는 현상이다.

## 🤔궁금한 점

## 📌중요한 점
