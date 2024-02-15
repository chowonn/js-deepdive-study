# 목차

[📌중요한 점](#📌중요한-점)

---

<br>

# 📌중요한 점

**객체 리터럴로 객체를 생성하는 방식의 문제점**
=> 단 하나의 객체만 생성하므로 만일 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 생성해야 해서 비효율적임.

<br>

> 프로퍼티 구조가 동일한 경우, 객체를 여러 개 생성해야 한다면 `생성자 함수`를 사용하자!

#### 17-04

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

## this

this 는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. this가 가리키는 값으로 this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
| 함수 호출 방식 | this가 가리키는 값 (this 바인딩) |
| ---- | --------------------- |
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로서 호출 | 메서드를 호춯한 객체(마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가 미래에 생성할 인스턴스 |

#### 17-05

```javascript
// 함수는 다양한 방식으로 호출될 수 있다.
function foo() {
  console.log(this);
}

// 일반적인 함수로서 호출
// 전역 객체는 브라우저 환경에서는 window, Node.js 환경에서는 global을 가리킨다.
foo(); // window

// 메서드로서 호출
const obj = { foo }; // ES6 프로퍼티 축약 표현
obj.foo(); // obj

// 생성자 함수로서 호출
const inst = new foo(); // inst
```

## 생성자 함수의 인스턴스 생성 과정

**생성자 함수**

- 객체를 생성하는 함수로 인스턴스를 생성하고 프로퍼티를 초기화 한다
- new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작함

`자바스크립트 엔진은 암묵적으로 인스턴스를 생성하고 반환함` (따로 생성 및 반환 코드가 없음)

**1. 인스턴스 생성과 this 바인딩**

- `인스턴스는 this에 바인딩 됨` => 생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 가리키는 이유
- 위의 처리는 런타임 전에 실행된다

#### 17-08

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

**2. 인스턴스 초기화**

- this에 바인딩되어 있는 인스턴스를 초기화한다
- 바인딩 된 인스턴스에 프로퍼티나 메서드를 추가해 초기 값을 인스턴스 프로퍼티에 할당함

#### 17-09

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

3. 인스턴스 반환

- 생성자 함수 내부에서 모든 처리가 끝나면 인스턴스가 바인딩 된 `this를 암묵적으로 반환한다`

#### 17-10

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

**생성자 함수에서 return 은 생략하자. 생성자 함수의 기본 동작을 훼손한다 !**

<br>
