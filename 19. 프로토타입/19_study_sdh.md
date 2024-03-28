## 목차

[📗배운 점](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

### **객제치향 프로그래밍**

> 프로그램을 명령어 또는 함수의 목록으로 보는 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나 여러개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다.

**속성** : 실체가 가지고 있는 특징이나 성질.

**추상화** : 다양한 속성중 프필요한 속성만 간추려 표현하는것.

```js
javascript;
// 이름과 주소 속성을 갖는 객체
const person = {
  name: "Lee",
  address: "Seoul",
};

console.log(person); // {name: "Lee", address: "Seoul"}
```

**객체** : 속성을 통해 여러개의 값을 하나의 단위로 구성한 복합적인 자료구조.

**객체지향 프로그래밍** : **상태**를 나타내는 데이터와 데이터를 조작할수 있는**동작**을 하나의 논리적인 단위로 묶어 생각한다.

### 상속과 프로토타입

**상속** : 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것.

생성자 함수로 생성된 객체는 동일한 프로퍼티(매서드 포함) 구조를 생성해낸다. 그래서 내용이 동일한 메서드가 중복 생성된다.

이때 프로토타입을 기반으로 상속을 구현하면 중복을 제거할 수 있다.

### 프로토타입 객체

**proto** 는 접근자 프로퍼티다, 객체가 직접 소유 하지 않으며 프로토타입에 귀속된다. 모든 객체는 상속을 통해 Object.prototype.**proto**접근자 프로퍼티를 사용할 수 있다.

```javascript
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

![image](https://github.com/chowonn/js-deepdive-study/assets/74224516/4413e1a1-6579-4d61-bfc7-f301832c9227)

```javascript
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefined

// 따라서 __proto__보다 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

> Object.create(null)을 사용하여 객체를 생성하면 해당 객체의 프로토타입 체인의 종점이 됩니다. 기본적으로 JavaScript의 모든 객체는 Object.prototype을 상속합니다. 그러나 Object.create(null)을 사용하면, 새로운 객체는 아무런 프로토타입을 갖지 않게 됩니다. 이것은 '순수한' 객체를 생성하는 것이며, JavaScript의 기본 객체 메서드나 속성을 상속받지 않습니다.

> **constructor** 프로퍼티는 JavaScript 객체가 자신을 생성한 생성자 함수를 참조하는 프로퍼티입니다. 모든 JavaScript 객체는 constructor 프로퍼티를 가지며, 이를 통해 객체가 어떤 생성자 함수를 사용하여 만들어졌는지 알 수 있습니다.constructor 프로퍼티와 생성자 함수

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true
```

### 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

**constructor** 는 생성자 함수를 찾아가는겁니다..!

아래는 생성자 함수로 생성한것.

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

아래는 리터럴 표기법으로 생성한것.

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

리터럴 표기법으로 생성한 객체는 생성자 함수가 없지만
생성자 함수를 찾아간다.

### 프로토타입의 생성 시점

프로토타입은 생성자 삼수가 생성되는 시점에 더불어 생성된다.
생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

화살표 함수는 constructor가 생성되지 않는다.

프로토타입 또한 객체 이기 때문에 자신의 프로토타입을 갖는다.
생성된 프로토타입의 프로토타입은 Object.prototype이다.

### 객체 생성 방식과 프로토타입의 결정

객체는 다음과 같이 다양한 생성 방법이 있다.

> - 객체리터럴
> - Object 생성자 함수
> - 생성자 함수
> - Object.create 메서드
> - 클래스

다양한 방식으로 생성되지만 모두 OrdinaryObjectCreate에 의해 생성된다.
즉 프로토타입은 추상연상OrdinaryObjectCreate 에 전달되는 인수에 의해 결정된다.

객체 리터럴에 의해 생성된 객체와 Object 생성자 함수에 의해 생성된 객체는 동일한 프로토타입을 가진다.
![image](https://github.com/chowonn/js-deepdive-study/assets/74224516/466b2e6e-9108-4063-b2f7-1edfae5eb43a)

![image](https://github.com/chowonn/js-deepdive-study/assets/74224516/e89a1e34-d396-42e7-bdc3-216853e1a3dd)

new 연산자와 함께 생성자 함수를 호출하여 객체를 생성하면 OrdinaryObjectCreate가 똑같이 호출되지만 전달되는 내용은 prototype 프로퍼티에 바인딩 되어있는 객체이다. 따라서 prototype 는 달라진다.

![image](https://github.com/chowonn/js-deepdive-study/assets/74224516/32f3b767-7353-43ed-b29c-eb25f7fb51ce)

### 프로토타입 체인

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty("name")); // true
```

위에 예제를 보면 me 객체는 person 뿐만 아니라 Object도 상속 받은걸 알 수 있다.

그림으로 표현하면 다음과 같다.
![image](https://github.com/chowonn/js-deepdive-study/assets/74224516/e8d5acc5-cd9b-4862-8591-1771d60c5fd9)

하위 객체에서 어떠한 메서드를 불러올 때 만약, 해당 객체에 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. (예시 속 hasOwnProperty를 불러온 것 처럼)
프로토타입 체인은 자바스크립트가 객체지향 프로그래밍에 상속을 구현하는 메커니즘이다.

예시 상황) me.hasOwnProperty('name');

> 1.  me 객체 속엔 hasOwnPropery 메서드가 없으므로 [[Prototype]] 내부 슬롯에 바인딩된 걸 따라 Person.prototype로 이동하여 메서드가 있는지 확인한다.
>     Person.prototype에도 없으니 그 상위인 Object.prototype으로 이동하여 메서드를 검색한다.
> 2.  Object.prototype 에는 hasOwnProperty가 존재함으로 이를 호출하고 이때, Object.prototype.hasOwnProrperty 메서드의 this에는 me 객체가 바인딩 된다.
> 3.  Object.prototype은 프로토타입 체인의 종점으로 이에 [[Prototype]] 슬롯은 null이다. 프로토타입 체인에 종점에서도 메서드를 찾을 수 없는 경우 error가 발생하지 않는다. (undefine 반환)

이에 반해 프로퍼티가 아닌 식별자는 스코프 체인에서 검색하는데, 이는 함수의 중첩관계로 이루어진 구조이다. me 식별자가 어디서 선언되었는지를 보고 해당 스코프에서 식별자를 검색한다. 식별자를 찾으면 해당 메서드를 프로토타입 체인을 통해 검색한다.

스코프 체인과 프로토타입 체인은 서로 연관없이 별도 동작이 아닌 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용된다.

### 오버라이딩과 프로퍼티 섀도잉

![image](https://github.com/chowonn/js-deepdive-study/assets/74224516/becaa996-ca00-401b-9486-b632ea8ec595)

> **오버라이딩** : 상위 클래스가 가지고 있는 매서드를 하위 클래스가 재정의 하여 사용하는 방식이다.

> **섀도잉**: 이로써 상의 클래스가 가려지는 현상을 섀도잉이라 한다.

위 그림에서 인스턴스 메서드를 삭제 한 후 호출하게 되면 프로퍼티 메서드가 호출이 된다.
프로토타입 페인을 통해서는 프로포타입 메서드를 삭제할 수 없다. 변경 또는 삭제는 직접 접근하여 삭제해야한다.

### 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경 할 수 있다.
이것은 부모객체인 프로토타입을 동적으로 변경 할 수 있다는 것을 의미한다.
프로토타입은 생성자 함수 또는 인스턴스에 의해 교체 할 수 있다.

#### 생성자 함수에 의한 교체

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

이렇게 교체된 프로토타입은 construct가 파괴된다.
따라서 constructor 프로퍼티를 추가 하여 프로토타입의 constructor를 되살릴 수 있다.

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

#### 인스턴스에 의한 프로토타입의 교체

이는 인스턴스에 **proto** (또는 setPrototypeOf)접근자 프로퍼티를 통해 프로토타입을 교체하는 방법이다.

이때도 컨스트럭터는 파괴된다.
따라서 새롭게 교체한 객체 리터럴에 constructor 프로퍼티를 추가하고 저설정해줘야 한다.

### instanceof 연산자

> **instanceof** 연산자는 이항 여난자로서 좌변의 객체를 가르키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피 연산자로 받는다

`객체 instanceof 생성자 함수`
만약 우변의 피연산자가 함수가 아니면 TypeError가 발생한다.
우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true, 존재하지 않으면 false가 평가된다.

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

constructor 프로퍼티와 생성자 함수간의 연결이 파괴되어도 instanceof연산에는 아무런 영향을 받지 않는다. (단지 프로토타입 체인만 보기 때문에)

### 직접상속

#### Object.create에 의 한 직접 상속

> **Object.create메서드** : 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다.
> 마찬가지로 추상연산OrdinaryObjectCreate를 호출하여 객체를 생성한다.

Object.create메서드로 객체를 생성할 때 장점

new연산자 없이도 객체 생성
프로토타입을 지정하면서 객체 생성
객체 리터럴에 의해 생성된 객체도 상속 가능

```js
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
console.log(Object.getPrototypeOf(obj) === Person.prototype); //
```

Object.prototype의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않는다.
Object.create메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문이다.(Obejct.create(null);로 생성하는 경우)

#### 객체 리터럴 내부에서 **proto**에 의한 직접 상속

ES6에서는 객체 리터럴 내부에서 **proto**접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

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

### 정적 프로퍼티 메서드

> **정적프로퍼티/메서드** 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출 할 수 있는 프로퍼티 메서드를 말한다.

Object.create 같은 메서드는 정적 메서드이고,
Object.prototype.hasOwnProperty같은 메서드는 프로토타입 메서드이다.![image](https://github.com/chowonn/js-deepdive-study/assets/74224516/31b2a6c9-fe99-4f46-a7ea-e41e59618fda)

### 프로퍼티 존재 확인

#### in 연산자

in 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.

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

// 대상 객제체가 상속받는 모든 프로토타입의 프로퍼티를 확인하므로 주의가 필요하다.
console.log("toString" in person); // true
```

새롭게 도입된것이다. 동일하게 작한다.

```javascript
const person = { name: "Lee" };

console.log(Reflect.has(person, "name")); // true
console.log(Reflect.has(person, "toString")); // true
```

#### Object.prototype.hasOwnProperty 메서드Permalink

Object.prototype.hasOwnProperty메서드를 사용해 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.
또한 동일하게 작동한다.

```javascript
console.log(person.hasOwnProperty("name")); // true
console.log(person.hasOwnProperty("age")); // false
```

### 프로퍼티 열거

#### for…in 문Permalink

객체의 모든 프로퍼티를 순회하며 열거(enumeration)하려면 for...in문을 사용한다.
`for (변수선언문 in 객체){...}`

for...in문은 객체의 프로토타입 체인 상에 존재하는 모든 프로퍼티 중에서 프로퍼티 어트리뷰트[[Enumerable]]값이 true인 프로퍼티만 열거(enumeration)한다.

toString 같은 경우 프로토타입 체인 상에 존재하지만 [[Enumerable]]값이 false이기 때문에 for...in문에서 열거되지 않는다.

또한 for...in문은 프로퍼티가 심벌인 경우에도 열거하지 않는다.

#### Object.keys/values/entries 메서드Permalink

for...in문은 객체 자신의 고유 프로퍼티 뿐만 아니라 상속받은 프로퍼티도 열거한다.

그렇기 때문에 고유의 프로퍼티만 열거하기 위해서는
Object.keys, Object.valeus, Object.entries 메서드 사용을 권장한다.

1. - Object.keys
     객체 자신의 열거가능한(enumerable)프로퍼티 키를 배열로 반환한다.
2. - Object.values
     ES8에서 도입되었다.
     객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다.
3. - Object.entries
     ES8에서 도입되었다.
     객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환한다.

## 🤔궁금한 점

## 📌중요한 점
