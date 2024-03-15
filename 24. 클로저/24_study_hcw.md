# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

# 렉시컬 스코프

**자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수가 정의된 위치에 따라 상위 스코프를 결정한다.**

**이를 렉시컬 스코프(정적 스코프) 라고 한다.**

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

- foo 함수와 bar 함수는 모두 전역 함수이므로 이 둘의 상위 스코프는 전역임.
- 함수를 어디서 호출하는지는 상위 스코프를 결정하는데 아무런 영향을 끼치지 못한다.

<br>

> '함수의 상위 스코프를 결정한다' 는 것은 '렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다' 는 말과 같다. <br>
> 따라서 렉시컬 스코프란, 렉시컬 환경의 '외부 렉시컬 환경에 대한 참조”에 저장할 참조값 즉, 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경에 의해 결정된다.

<br>

# 함수 객체의 내부 슬롯 [[Environment]]

함수 정의가 평가되어 함수 객체를 생성할 때 자신이 정의된 환경에 의해 결정된 상위 스코프의 참조를 함수 객체 자신의 내부 슬롯에 저장한다.

이때 자신의 내부 슬롯 [[Environment]]에 저장된 상위 스코프의 참조는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킨다.

> 함수 객체의 내부 슬롯 [[Environment]]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프다. <br> 또한 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장될 참조값이다. <br> 함수 객체는 내부 슬롯 [[Environment]]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.

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

위 예제의 foo 함수 내부에서 bar 함수가 호출되어 실행 중인 시점의 실행 컨텍스트는 아래와 같다.

![image](https://github.com/chowonn/js-deepdive-study/assets/70478015/d16785bc-552e-499b-9184-578490c365f7)

- foo 함수, bar 함수 전역에서 함수 선언문으로 정의되고 전역 객체 window의 메서드가 된다.
- [[Environment]] 에는 함수 정의가 평가된 시점, 즉 전역 코드 평가 시점에 실행 중인 실행 컨텍스트의 렉시컬 환경인 전역 렉시컬 환경의 참조가 저장된다. (①)
- 함수가 호출되면 함수 내부로 코드의 제어권이 이동하고 함수 코드를 평가한다

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성<br>
   2-1. 함수 환경 레코드 생성<br>
   2-2. this 바인딩<br>
   2-3. 외부 렉시컬 환경에 대한 참조 결정

- 외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조가 할당된다. (②, ③)

  <br>

# 클로저와 렉시컬 환경

- `외부 함수보다 중첩 함수가 더 오래 유지되는 경우, 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저(closure)라고 부른다.`

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

위 코드의 실행 결과(④)는 outer 함수의 지역 변수 x의 값인 10이다. 이미 생명 주기가 종료되어 실행 컨텍스트 스택에서 제거되어도 outer 함수의 지역 변수 x는 동작하고 있다.

- inner 함수는 자신이 평가될 때 자신이 정의된 위치에 의해 결정된 상위 스코프를 [[Environment]] 내부 슬롯에 저장한다.
- outer 함수를 호출하면 outer 함수의 렉시컬 환경이 생성되고 앞서 outer 함수 객체의 [[Environment]]에 저장된 전역 렉시컬 환경을 outer 함수 렉시컬 환경의 '외부 렉시컬 환경에 대한 참조' 에 할당한다.
- 중첩함수 inner 가 평가되고 자신의 [[Environment]] 내부 슬롯에 현재 실행중인 실행 컨텍스트의 렉시컬 환경, 즉 outer 함수의 렉시컬 환경을 상위 스코프로서 저장한다.
- **이때 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다.**

<br>

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저이다.

그러나 상위 스코프의 어떤 식별자도 참조하지 않는다면 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않는다.

> 클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다 !

<br>

# 클로저의 활용

- `클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.`

**즉, 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위함이다.**

<br>

# 캡슐화와 정보 은닉

**캡슐화**는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.

캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 **정보 은닉**이라 한다.

자바스크립트에서는 public, protected, private 같은 접근 제한자를 제공하지 않으므로 `기본적으로 public하다.`

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

- `name` 프로퍼티는 외부로 공개되어 있어 자유롭게 참조하거나 변경할 수 있다. 즉, `public` 하다.
- `_age` 변수는 Person 생성자 함ㅁ수의 지역 변수이므로 Person 생성자 함수 외부에서 참조하거나 변경할 수 없다. 즉, `private` 하다.

<br>

위 예제의 sayHi 메서드는 인스턴스 메서드이므로 Person 객체가 생성될 때마다 중복 생성된다.

따라서 즉시 실행 함수를 사용하여 아래의 예시처럼 Person 생성자 함수와 Person.prototype.sayHi 메서드를 하나의 함수로 모을 수 있다.

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

- 위와 같이 작성하면 접근 제한자를 사용하지 않아도 정보 은닉이 가능한 것처럼 보인다
- 그러나 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 다음과 같이 \_age 변수의 상태가 유지되지 않는다는 것이다.

<br>

```javascript
const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person("Kim", 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.
```

- 이는 Person.prototype.sayHi 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 형상이다. (즉시 실행 함수가 호출될 때 생성됨)
- 이처럼 자바스크립트는 정보 은닉을 완전하게 지원하지는 않는다 ..

# 📗배운 점

## 렉시컬 스코프가 가능하려면?

자신이 호출되는 환경과는 상관없이 **자신이 정의된 환경**, 즉 상위 스코프를 기억해야 한다.

이를 위해 `함수는 자신의 내부 슬롯 [[Environment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.`

<br>

# 🤔궁금한 점
