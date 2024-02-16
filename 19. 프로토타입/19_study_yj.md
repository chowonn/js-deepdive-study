## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 19.1 객체지향 프로그래밍

객체지향 프로그래밍은 **속성**을 가지고 있고, 이를 통해 실체를 인식하거나 구별할 수 있음

추상화 : 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것

```javascript
const circle = {
  radius: 5, // 반지름

  // 원의 지름: 2r
  getDiameter() {
    return 2 * this.radius;
  },

  // 원의 둘레: 2πr
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  // 원의 넓이: πrr
  getArea() {
    return Math.PI * this.radius ** 2;
  },
};

console.log(circle);
// {radius: 5, getDiameter: ƒ, getPerimeter: ƒ, getArea: ƒ}

console.log(circle.getDiameter()); // 10
console.log(circle.getPerimeter()); // 31.41592653589793
console.log(circle.getArea()); // 78.53981633974483
```

- 객체지향 프로그래밍은 객체의 상태를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작을 하나의 논리적인 단위로 묶어 생각
- 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조
- 객체의 상태 데이터를 프로퍼티, 동작을 메서드라 함

## 19.2 상속과 프로토타입

상속 : 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것

자바스크립트는 프로토타입을 기반으로 상속을 구현

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

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/dcd6b552-b303-4e55-9cff-1666fe914ceb)

## 19.3 프로토타입 객체

모든 객체는 하나의 프로토타입을 가지고 모든 프로토타입은 생성자 함수와 연결되어 있음

객체와 프로토타입과 생성자 함수는 서로 연결되어 있음

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/c7cb2db1-b9bb-4c54-8f06-0343ae6a33ec)

### 19.3.1 \_\_proto\_\_접근자 프로퍼티

모든 객체는 \_\_proto\_\_ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근 가능

\_\_proto\_\_는 접근자 프로퍼티다 → 내부 슬롯에 직접적으로 호출할 수 없고 \_\_proto\_\_ 접근자 프로퍼티를 통해 간접적으로 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근할 수 있음

\_\_proto\_\_ 접근자 프로퍼티는 상속을 통해 사용 → \_\_proto\_\_ 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티

\_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유 → 프로토타입 체인이 생성되는 것을 방지하기 위해

\_\_proto\_\_ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않음 → Object.getPrototypeOf 메서드나 Object.setPrototypeOf 메서드 사용을 권장

### 19.3.2 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴

```javascript
// 화살표 함수는 non-constructor다.
const Person = (name) => {
  this.name = name;
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(Person.hasOwnProperty("prototype")); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(Person.prototype); // undefined

// ES6의 메서드 축약 표현으로 정의한 메서드는 non-constructor다.
const obj = {
  foo() {},
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(obj.foo.hasOwnProperty("prototype")); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(obj.foo.prototype); // undefined
```

모든 객체가 가지고 있는 \_\_proto\_\_접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킴

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/65366514-a1d2-4a34-9237-c7597f2d8a35)

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__); // true
```

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/2d63cb2b-a4bb-414e-a48d-35e20805b798)

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

모든 프로토타입은 constructor 프로퍼티를 가짐

이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킴

이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이뤄짐

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true
```

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/bed1e4c0-dfeb-4586-a341-36781806c71c)

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

```javascript
// obj 객체를 생성한 생성자 함수는 Object다.
const obj = new Object();
console.log(obj.constructor === Object); // true

// add 함수 객체를 생성한 생성자 함수는 Function이다.
const add = new Function("a", "b", "return a + b");
console.log(add.constructor === Function); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// me 객체를 생성한 생성자 함수는 Person이다.
const me = new Person("Lee");
console.log(me.constructor === Person); // true
```

- constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수

```javascript
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) {
  return a + b;
};

// 배열 리터럴
const arr = [1, 2, 3];

// 정규표현식 리터럴
const regexp = /is/gi;
```

- 리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있음
- 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없음

```javascript
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

- obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴에 의해 생성된 객체
- obj 객체는 Object 생성자 함수와 constructor 프로퍼티로 연결되어 있음

프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재

## 19.5 프로토타입의 생성 시점

프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성 → 생성된 프로토타입의 프로토타입은 언제나 Object.prototype

```javascript
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

- non-constructor는 프로토타입이 생성되지 않음

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

Object, String, Number, Function, Array 등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성 → 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩

생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당

## 19.6 객체 생성 방식과 프로토타입의 결정

추상 연산 OrdinaryObjectCreate는 필수적으로 자신이 생성할 객체의 프로토타입을 인수로 전달받음

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때 추상 연산 OrdinaryObjectCreate를 호출

이 때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype임

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

Object 생성자 함수를 인수 없이 호출하면 빈 객체가 생성

Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상 연산 OrdinaryObjectCreate가 호출

이 때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype임

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체 생성 방식과 마찬가지로 추상 연산 OrdinaryObjectCreate가 호출

이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체

## 19.7 프로토타입 체인

프로토타입 체인 : 자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색

프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/aa5dcc1c-7bb8-42f5-95c3-4a1d13550880)

프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype → Object.prototype을 프로토타입 체인의 종점이라 함

종점에서도 프로퍼티를 검색할 수 없는 경우 undefined 반환

## 19.8 오버라이딩 프로퍼티 섀도잉

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

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/440c8414-a101-46ac-b2bf-9a7884c5b257)

- 프로토타입이 소유한 프로퍼티(메서드 포함)를 프로토타입 프로퍼티, 인스턴스가 소유한 프로퍼티를 인스턴스 프로퍼티라고 함
- 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가
- 이때 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩 했고 프로토타입 메서드 sayHello는 가려짐
- 이처럼 상속 관계에 의해 프로퍼티가 가려지는 현상을 프로퍼티 섀도잉이라 함

```javascript
// 인스턴스 메서드를 삭제한다.
delete me.sayHello;
// 인스턴스에는 sayHello 메서드가 없으므로 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

- 프로토타입 메서드가 아닌 인스턴스 메서드 sayHello 삭제

```javascript
// 프로토타입 체인을 통해 프로토타입 메서드가 삭제되지 않는다.
delete me.sayHello;
// 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

- 하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능
- 하위 객체를 통해 프로토타입에 get 엑세스는 허용되나 set 엑세스는 허용되지 않음

## 19.9 프로토타입의 교체

### 19.9.1 생성자 함수에 의한 프로토타입의 교체

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

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/aaca01b8-d172-40c0-b968-6e238cf30d52)

- 1에서 Person.prototype에 객체 리터럴 할당
  - Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체한 것
- 프로토타입으로 교체한 객체 리터럴에는 constructor가 없음
  - me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나옴

```javascript
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

- 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결 파괴

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

- 프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하여 프로토타입의 constructor 프로퍼티를 되살림

### 19.9.2 인스턴스에 의한 프로토타입의 교체

프로토타입은 생성자 함수의 prototype 프로퍼티뿐만 아니라 인스턴스의 \_\_proto\_\_ 접근자 프로퍼티(또는 Object.getPrototypeOf 메서드)를 통해 접근할 수 있음

\_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것

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

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/1d4ccdb2-6990-492e-b9cb-85a2ab55d0bb)

- 1에서 me 객체의 프로토타입을 parent 객체로 교체

```javascript
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

- 프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없으므로 constructor 프로퍼티와 생성자 함수 간의 연결 파괴
  - 프로토타입의 constructor 프로퍼티로 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나옴

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/d584e53c-6c1e-4818-b461-7baf569ad19a)

- 생성자 함수에 의한 프로토타입 교체와 인스턴스에 의한 프로토타입 교체의 미묘한 차이

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
  constructor: Person,
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  },
};

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결을 설정
Person.prototype = parent;

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false

// 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.
console.log(Person.prototype === Object.getPrototypeOf(me)); // true
```

- 프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하고 생성자 함수의 prototype 프로퍼티를 재설정하여 파괴된 생성자 함수와 프로토타입 간의 연결 되살리기

## 19.10 instanceof 연산자

`객체 instanceof 생성자 함수`

instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받음

우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입의 교체
Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결되어 있지 않다.
console.log(Person.prototype === parent); // false
console.log(parent.constructor === Person); // false

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않기 때문에 false로 평가된다.
console.log(me instanceof Person); // false

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

- Person.prototype이 me 객체의 프로토타입 체인상에 존재하지 않음
- 프로토타입으로 교체한 parant 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩하면 me instanceof Person은 true로 평가될 것

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입의 교체
Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결되어 있지 않다.
console.log(Person.prototype === parent); // false
console.log(parent.constructor === Person); // false

// parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩한다.
Person.prototype = parent;

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

- instanceof 연산자는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/e0d327f9-7c50-4ff1-b429-0e310c3ad3c9)

```javascript
function isInstanceof(instance, constructor) {
  // 프로토타입 취득
  const prototype = Object.getPrototypeOf(instance);

  // 재귀 탈출 조건
  // prototype이 null이면 프로토타입 체인의 종점에 다다른 것이다.
  if (prototype === null) return false;

  // 프로토타입이 생성자 함수의 prototype 프로퍼티에 바인딩된 객체라면 true를 반환한다.
  // 그렇지 않다면 재귀 호출로 프로토타입 체인 상의 상위 프로토타입으로 이동하여 확인한다.
  return (
    prototype === constructor.prototype || isInstanceof(prototype, constructor)
  );
}

console.log(isInstanceof(me, Person)); // true
console.log(isInstanceof(me, Object)); // true
console.log(isInstanceof(me, Array)); // false
```

- 생성자 함수에 의해 프로토타입이 교체되어 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴되어도 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로 instanceof는 아무런 영향을 받지 않음

## 19.11 직접 상속

### 19.11.1 Object.create에 의한 직접 상속

Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/9b113959-a53b-41de-a10d-399f7d9763ce)

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

- Object.create 메서드는 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성
  - 객체를 생성하면서 직접적으로 상속을 구현
- 장점
  - new 연산자 없이도 객체를 생성
  - 프로토타입을 지정하면서 객체를 생성
  - 객체 리터럴에 의해 생성된 객체도 상속 가능

```javascript
// 프로토타입이 null인 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

// console.log(obj.hasOwnProperty('a')); // TypeError: obj.hasOwnProperty is not a function

// Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않는다.
console.log(Object.prototype.hasOwnProperty.call(obj, "a")); // true
```

- Object.create 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문에 권장하지 않음

### 19.11.2 객체 리터럴 내부에서 \_\_prototype\_\_에 의한 직접 상속

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

- 객체 리터럴 내부에서 \_\_proto\_\_접근자 프로퍼티를 사용하여 직접 상속 구현

## 19.12 정적 프로퍼티/메서드

정적 프로퍼티/메서드 : 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = "static prop";

// 정적 메서드
Person.staticMethod = function () {
  console.log("staticMethod");
};

const me = new Person("Lee");

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

- Person 생성자 함수는 객체이므로 자신의 프로퍼티/메서드를 소유할 수 있음
- 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없음

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/f24c66a1-3b67-4c33-b66f-0de1b5139d09)

정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 인스턴스로 접근할 수 없음

```javascript
// Object.create는 정적 메서드다.
const obj = Object.create({ name: "Lee" });

// Object.prototype.hasOwnProperty는 프로토타입 메서드다.
obj.hasOwnProperty("name"); // -> false
```

- Object.create 메서드는 Object 생성자 함수의 정적 메서드
  → Object 생성자 함수가 생성한 객체로 호출할 수 없음

- Object.prototype.hasOwnProperty 메서드는 Object.prototype의 메서드
  → 모든 객체의 프로토타입 체인의 종점, Object.prototype의 메서드이므로 모든 객체가 호출 가능

## 19.13 프로퍼티 존재 확인

### 19.13.1 in 연산자

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/b9b6452d-133b-4f5d-b8df-44204cafb922)

객체 내에 특정 프로퍼티가 존재하는지 여부 확인

```javascript
const person = {
  name: "Lee",
  address: "Seoul",
};

// person 객체에 name 프로퍼티가 존재한다.
console.log("name" in person); // true
// person 객체에 address 프로퍼티가 존재한다.
console.log("address" in person); // true
// person 객체에 age 프로퍼티가 존재하지 않는다.
console.log("age" in person); // false
```

- in 연산자는 확인 대상 객체의 프로퍼티뿐만 아니라 확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인

```javascript
console.log("toString" in person); // true
```

- person 객체에는 toString이라는 프로퍼티가 없지만 true
  - toString은 Object.prototype의 메서드
  - 프로토타입 체인 상에 존재하는 모든 toString을 검색한 것

### 19.13.2 Object.prototype.hasOwnProperty 메서드

```javascript
console.log(person.hasOwnProperty("name")); // true
console.log(person.hasOwnProperty("age")); // false
```

```javascript
console.log(person.hasOwnProperty("toString")); // false
```

- 인수로 전달받은 프로퍼티 키가 객체의 고유의 프로퍼티 키인 경우에만 true 반환
- 상속받은 프로토타입의 프로퍼티 키인 경우 false 반환

## 19.14 프로퍼티 열거

### 19.14.1 for...in 문

`for (변수선언문 in 객체) {...}`

for...in 문은 객체의 프로퍼티 개수만큼 순회하여 for...in 문의 변수 선언문에서 선언한 변수에 프로퍼티 키를 할당

순회 대상 객체의 프로퍼티뿐만 아니라 상속받은 프로토타입 프로퍼티까지 열거

```javascript
const person = {
  name: "Lee",
  address: "Seoul",
};

// in 연산자는 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
console.log("toString" in person); // true

// for...in 문도 객체가 상속받은 모든 프로토타입의 프로퍼티를 열거한다.
// 하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.
for (const key in person) {
  console.log(key + ": " + person[key]);
}

// name: Lee
// address: Seoul
```

- toString과 같은 Object.prototype의 프로퍼티가 열거되지 않음
- toString 메서드가 열거할 수 없도록 정의되어 있는 프로퍼티

```javascript
// Object.getOwnPropertyDescriptor 메서드는 프로퍼티 디스크립터 객체를 반환한다.
// 프로퍼티 디스크립터 객체는 프로퍼티 어트리뷰트 정보를 담고 있는 객체다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "toString"));
// {value: ƒ, writable: true, enumerable: false, configurable: true}
```

- 프로퍼티 어트리뷰터 [[Enumerable]]은 프로퍼티 열거 가능 여부를 나타내며 불리언 값을 가짐
- Object.prototype.toString 프로퍼티의 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false

for...in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거

```javascript
const sym = Symbol();
const obj = {
  a: 1,
  [sym]: 10,
};

for (const key in obj) {
  console.log(key + ": " + obj[key]);
}
// a: 1
```

- 상속받은 프로퍼티를 제외하고 객체 자신의 프로퍼티만 열거하려면 Object.prototype.hasOwnPrototype 메서드를 사용하여 객체 자신의 프로퍼티인지 확인

### 19.14.2 Object.keys/values/entries 메서드

객체 자신의 고유 프로퍼티만 열거하기 위해서는 for...in 문을 사용하는 것보다 Object.keys/values/entries 메서드를 사용하는 것을 권장

```javascript
const person = {
  name: "Lee",
  address: "Seoul",
  __proto__: { age: 20 },
};

console.log(Object.keys(person)); // ["name", "address"]
```

- Object.keys 메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환

```javascript
console.log(Object.values(person)); // ["Lee", "Seoul"]
```

- Object.values 메서드는 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환

```javascript
console.log(Object.entries(person)); // [["name", "Lee"], ["address", "Seoul"]]

Object.entries(person).forEach(([key, value]) => console.log(key, value));
/*
name Lee
address Seoul
*/
```

- Object.entries 메서드는 객체 자신의 열거 가능한 프로퍼티 키와 쌍의 배열을 배열에 담아 반환

## 🤔궁금한 점

## 📌중요한 점
