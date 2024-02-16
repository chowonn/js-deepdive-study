# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

**객체지향 프로그래밍** : 여러 개의 독립적 단위, 즉 객체의 집합으로 프로프램을 표현하려는 프로그래밍 패러다임

**추상화** : 다양한 속성 중에서 프로그램에 필요한 속성한 간추려 내어 표현하는 것

**객체** : 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조

<br>

# 상속과 프로토타입

자바스크립는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다.

### 19-04

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

- Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받음
- 즉, 자신의 상태를 나타내는 radius 프로퍼티만 개별적으로 소유하고 내용이 동일한 메서드는 상속을 통해 공유
- `상속은 코드의 재사용이란 관점에서 매우 유용하다 `

<br>

# 프로토타입

- 프로토타입 객체란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다.
- 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.
- 모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 가지며, 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]] 에 저장된다

<br>

## \_ _ proto _ \_ 접근자 프로퍼티

**1. \_ _ proto _ \_ 는 접근자 프로퍼티다.**

- [[Prototype]] 내부 슬롯에 직접 접근할 수 없으며 \_ _ proto _ \_접근자 프로퍼티를 통해 간접적으로 프로토타입에 접근할 수 있다.

### 19-06

```javascript
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

**2. 모든 객체는 상속을 통해 Object.prototype.\_ _ proto _ \_ 접근자 프로퍼티를 사용할 수 있다.**

### 19-07

```javascript
const person = { name: "Lee" };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty("__proto__")); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

**3. \_ _ proto _ \_ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.**

- 모든 객체가 \_ _ proto _ \_ 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문이다
- \_ _ proto _ \_ 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우에는 Object.getPrototypeOf 메서드를 사용하고, 프로토타입을 교체하고 싶은 경우에는 Object.setPrototypeOf 메서드를 사용할 것을 권장한다.

### 19-10

```javascript
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

# 함수 객체의 prototype 프로퍼티

`함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.`

### 19-11

```javascript
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty("prototype"); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty("prototype"); // -> false
```

**즉 non-constructor인 화살표 함수, ES6 메서드 축약 표현으로 정의한 메서드는 prototype프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.**
| 구분 | 소유 | 값 | 사용 주체 | 사용 목적 |
| ---- | --------------------- |------------ | ------------ | ------------ |
| \_ _ proto _ \_ 접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 |
| prototype 프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생설할 객체의 프로포타입을 할당하기 위해 사용 |

<br>

# 프로토타입의 생성 시점

> **프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.**

## 사용자 정의 생성자 함수와 프로토타입 생성 시점

**`생성자 함수로서 호출이 가능한 함수. 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.`**

### 19-20

```javascript
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

> `사용자 정의 생성자 함수로 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다`

![image](https://github.com/chowonn/js-homework/assets/70478015/0c118a88-7899-4a73-8b1a-2124c686b749)

<br>

## 빌트인 생성자 함수와 프로토타입 생성 시점

- `Object`, `Striong`, `Number`, `Function`, `Array`, `RegExp`, `Date`, `Promise`등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.
- **모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다**
- 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화 되어 존재한다. **이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[prototype]] 내부 슬롯에 할당된다.**

<br>

# 객체 생성 방식과 프로토타입의 결정

- 객체 리터럴
- Object생성자 함수
- 생성자 함수
- Object.create메서드
- 클래스(ES6)

=> 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 모두 추상 연산 `OrdinaryObjectCreate`에 생성된다.

<br>

## 객체 리터럴에 의해 생성된 객체의 프로토타입

객체 리터럴에 의해 객체를 생성할 때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.

즉, 객체 리터럴로 생성된 객체의 프로토타입은 Object.prototype 이다.

### 19-24

```javascript
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

## Object 생성자 함수에 의해 생성된 객체의 프로토타입

- Object 생성자 함수를 인수없이 호출하면 빈 객체가 생성된다.
- OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.
- 객체 리터럴과 Object생성자 함수에 의한 객체 생성 결과는 동일하나, 프로퍼티를 추가하는 방식이 다르다.
- 객체 리터럴은 리터럴 내부에 프로퍼티를 추가하지만, Object생성자 함수 방식은 일단 빈 객체를 생성한 이후 프로퍼티를 추가해야 한다.

### 19-26

```javascript
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

## 생성자 함수에 의해 생성된 객체의 프로토타입

- new연산자와 함께 생성자 함수를 호출하여 객체를 생성하는 방식도 마찬가지로 추상연산OrdinaryObjectCreate가 호출된다.
- 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

### 19-28

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
const you = new Person("Kim");

me.sayHello(); // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

![image](https://github.com/chowonn/js-homework/assets/70478015/f3d31996-09ef-46ff-88ad-d8bce3ae3092)

<br>

# 프로토타입 체인

자바스크립트는 어떤 객체의 프로퍼티 또는 메서드에 접근할 때, 해당 객체에 존재하지 않는다면 [[Prototype]] 의 참조를 따라 프로토타입의 프로퍼티 또는 메서드를 순차적으로 찾아 올라간다. => **`프로토타입 체인`**

프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘

- 프로토타입 최상위 객체 : Object.prototype (= 프로토타입 체인의 종점)

<br>

# 오버라이딩과 프로퍼티 섀도잉

- **`오버라이딩`** : 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식
- **`오버로딩`** : 함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고, 매개변수에 의해 메서드를 구별하여 호출하는 방식이다.
  자바스크립트는 오버로딩을 지원하지 않지만 arguments객체를 사용하여 구현할 수는 있다.
- **`프로퍼티 섀도잉`** : 상속 관계에 의해 프로퍼티가 가려지는 현상

### 19-36

```javascript
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("Lee");

// 인스턴스 메서드
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello(); // Hey! My name is Lee
```

하위 객체에서 프로토타입 프로퍼티를 변경하거나 삭제하는 것은 불가능

### 19-38

```javascript
// 프로토타입 체인을 통해 프로토타입 메서드가 삭제되지 않는다.
delete me.sayHello;
// 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

# 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경할 수 있다 => 부모 객체인 프로토타입을 동적으로 변경 가능

<br>

## 생성자 함수에 의한 프로토타입의 교체

프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // ① 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");
```

<br>

## 인스턴스에 의한 프로토타입의 교체

- 인스턴스의 \_ _ proto _ \_접근자 프로퍼티를 통해 프로토타입을 교체할 수 있다
- 생성자 함수의 prototype 프로퍼티에 다른 임의의 객체를 바인딩하는 것은 미래에 생성할 인스턴스의 프로토타입을 교체하는 것으로, 이미 생성된 객체의 프로토타입을 교체하는 것이다.

### 19-43

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  },
};

// ① me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee
```

```javascript
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

# 직접 상속

## Object.create에 의한 직접 상속

Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다.

- 첫 번째 매개변수 : 생성할 객체의 프로토타입으로 지정할 객체
- 두 번째 매개변수 : 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달함 (생략 가능)

### 19-51

```javascript
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj → Object.prototype → null
// obj = {};와 동일하다.
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj → Object.prototype → null
// obj = { x: 1 };와 동일하다.
obj = Object.create(Object.prototype, {
  x: { value: 1, writable: true, enumerable: true, configurable: true },
});
// 위 코드는 다음과 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj → myProto → Object.prototype → null
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// obj → Person.prototype → Object.prototype → null
// obj = new Person('Lee')와 동일하다.
obj = Object.create(Person.prototype);
obj.name = "Lee";
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

> Object.create 메서드의 장점

1. new 연산자가 없이도 객체를 생성할 수 있다.
2. 프로토타입을 지정하면서 객체를 생성할 수 있다.
3. 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

<br>

## 객체 리터럴 내부에서 \_ _ proto _ \_에 의한 직접 상속

Object.create 메서드에 의한 직접 상속은 여러 장점이 있으나 두 번째 인자로 프로퍼티를 정의하는 것이 번거로움.

그래서 ES6에서는 객체 리터럴 내부에서 \_ _ proto _ \_접근자 프로퍼티를 사용해서 `직접 상속`을 구현할 수 있다.

### 19-55

```javascript
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto,
};
/* 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

# 프로퍼티 열거

## for ...in 문

- 객체의 모든 프로퍼티를 순회하며 열거하려면 for...in문 사용 => **`for (변수 선언문 in 객체) {...}`**
- 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거한다
- 심벌은 열거하지 않음
- 배열에는 for...in문 말고 일반적인 for문, for...of문, Array.prototype.forEach 권장함

<br>

# 📗배운 점

> \_ _ proto _ \_ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유는?

- 상호 참조에 의해 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 생성되는 것을 방지하기 위해서

### 19-08

```javascript
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

=> **`프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다.`**

<br>

# 🤔궁금한 점
