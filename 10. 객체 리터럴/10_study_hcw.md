# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

## 자바스크립트의 객체 생성 방법

1. `객체 리터럴` (일반적이고 간단한 방법)
2. Object 생성자 함수
3. 생성자 함수
4. Object.create 메서드
5. 클래스 (ES6)

## 프로퍼티 접근 방법

1. 마침표 프로퍼티 접근 연산자(.)를 사용하는 **마침표 표기법 (dot notation)**
2. 대괄호 프로퍼티 접근 연산자([ ])를 사용하는 **대괄호 표기법(bracket notation)**

- 프로퍼티 키가 식별자 네이밍 규칙을 준수하는 이름, 즉 자바스크립트에서 사용 가능한 유효한 이름이면 마침표 표기법과 대괄호 표기법을 모두 사용할 수 있다.
- 대괄호 표기법의 경우 `[ ]의 내부에 지정하는 키는 반드시 따옴표로 감싼 문자열`이어야 한다. 그렇지 않으면 자바스크립트 엔진은 식별자로 해석함

### 10-13

```javascript
var person = {
  name: "Lee",
};

console.log(person[name]); // ReferenceError: name is not defined
```

- `ReferenceError가 발생한 이유` => 따옴표로 감싸지 않은 이름, 즉 식별자 name을 평가하기 위해 선언된 name을 찾았지만 찾지 못했기 때문에

<br>

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

### 프로퍼티 축약 표현

- 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다.

![image](https://github.com/chowonn/Algorithm-js/assets/70478015/5d93371c-6b04-4dfb-93cd-0e6b54160401)

### 10-20

```javascript
// ES6
let x = 1,
  y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

### 메서드 축약 표현

- 메서드를 정의할 때 `function 키워드를 생략`할 수 있다.
- 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와는 다르게 동작하므로 주의 ! => **this의 동작이 달라진다**

### 10-23

```javascript
// ES5
var obj = {
  name: "Lee",
  sayHi: function () {
    console.log("Hi! " + this.name);
  },
};

obj.sayHi(); // Hi! Lee
```

### 10-24

```javascript
// ES6
const obj = {
  name: "Lee",
  // 메서드 축약 표현
  sayHi() {
    console.log("Hi! " + this.name);
  },
};

obj.sayHi(); // Hi! Lee
```

<br>

# 📗배운 점

## 프로퍼티에 대한 다양한 접근 방법

### 🧐 person.last-name의 실행 결과가 환경에 따라 다른 이유는?

### 10-15

```javascript
var person = {
  'last-name': 'Lee',
  1: 10
};

person.'last-name';  // -> SyntaxError: Unexpected string
person.last-name;    // -> 브라우저 환경: NaN
                     // -> Node.js 환경: ReferenceError: name is not defined
person[last-name];   // -> ReferenceError: last is not defined
person['last-name']; // -> Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.
person.1;     // -> SyntaxError: Unexpected number
person.'1';   // -> SyntaxError: Unexpected string
person[1];    // -> 10 : person[1] -> person['1']
person['1'];  // -> 10
```

- person.last-name 실행할 때 자바스크립트 엔진은 먼저 person.last를 평가하게 되는데 last 키가 없으므로 `person.last가 undefined 로 평가된다`. 결과적으로 `person.last-name  ➡ undefined-name` 과 같다.
- 이후 `name이라는 식별자`를 찾는다. (name은 키가 아님에 주의하자! 식별자로 해석된다.)
- Node.js 환경에서는 name 이라는 식별자 선언이 없으므로 `ReferenceError: last is not defined` 에러를 발생시킨다.
- 브라우저 환경에서는 name 이라는 전역변수가 존재한다.(전역 객체 window의 프로퍼티) 기본값은 빈 문자열 이므로 `person.last-name  ➡ undefined-' '`이므로 NaN이 된다.

<br>

# 🤔궁금한 점

## 🧐 왜 ReferenceError가 아닌 undefined를 반환할까?

### 10-14

```javascript
var person = {
  name: "Lee",
};

console.log(person.age); // undefined
```

👩‍🏫 **`동적 타입 언어`, `느슨한 타입 체크`, `호이스팅`** 개념과 관련이 있다 !

1. **동적 타입 언어**

- 자바스크립트는 동적 타입 언어로, 변수의 타입이 런타임에 결정된다. (정적 타입 언어는 컴파일 때 타입을 확인함)

2. **느슨한 타입 체크**

- 자바스크립트는 느슨한 타입 체크를 하므로 변수의 타입이 런타임에 동적으로 결정되는데 변수에 해당 값이 없는 경우 undefined가 할당된다. person 이라는 객체를 참조는 하지만 age 프로퍼티가 없으므로 undefined를 반환한다.

3. **호이스팅**

- 코드 실행 전에 변수와 함수 선언을 끌어올리는 호이스팅 특성이 있는데, 따라서 코드가 실행되기 전에 변수 person이 선언되고 초기화되므로 console.log(person.age) 에서 ReferenceError가 아닌 undefined를 반환한다.
