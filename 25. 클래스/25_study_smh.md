# 목차

[📗배운 점](#📗배운-점)

[📌중요한 점](#📌중요한-점)

[🤔궁금한 점](#🤔궁금한-점)

## 📗배운 점

## 25.1 클래스는 프로토타입의 문법적 설탕인가 ?

es5에서는 클래스 없이 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다.

es6에서 클래스 도입으로 클래스 기반 객체지향 언어가 익숙한 프로그래머의 학습을 돕는다.

> 기존 프로토타입 기반 객체지향 모델을 폐지하는게 아닌 , 사실상 함수인 클래스로 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 **문법적 설탕**을 제공한 것

클래스와 생성자 함수의 차이점 
1. 클래스를 new 연산자 없이 호출하면 에러가 발행한다. 하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다. 
2. 클래스는 상속을 지원하는 extends와 super 키워드를 제공한다. 하지만 생성자 함수는 extends와 super 키워드를 지원하지 않는다. 
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동착한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스틸이 함수 표인 식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다. 
4. 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다. 하지만 생성자 함수는 암묵적으로 strict mode가 지정되지 않는다. 
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다. 다시 말해 열거되지 않는다. 

## 25.2 클래스 정의

`파스칼 케이스` 명명

```javascript
// 클래스 선언문
class Person {}
```
`표현식으로 정의`


```javascript
// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

표현식 정의가 가능하다는 것은 ? 클래스가 값으로 사용할 수 있는 일급 객체라는 뜻 

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다. 
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다. 
3. 함수의 매개변수에게 전달할 수 있다. 
4. 함수의 반환값으로 사용할 수 있다. 


= 클래스는 함수다. 따라서 클래스는 값처럼 사용할 수 있는 일급 객체다.


## 25.3 클래스 호이스팅

`클래스 = 함수`

**함수 선언문** : 소스코드 평가 과정, 런타임 이전에 먼저 평가되어 함수 객체를 생성

클래스도 함수 선언문과 같이 런타임 이전에 평가된다. 이때 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 constructor이다. 생성자 함수로 호출할 수 있기에 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다. `프로포타입과 생성자 함수는 언제나 한쌍이기 때문`


```javascript
console.log(Person);
// ReferenceError: Cannot access 'Person' before initialization
//호이스팅이 안되는 걸까 ?
// 클래스 선언문
class Person {}
```
```javascript
const Person = '';

{
  // 호이스팅이 발생하지 않는다면 ''이 출력되어야 한다.
  console.log(Person);
  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```
정답: 호이스팅 발생 !

클래스는 let,const처럼 선언문 이전에 일시적 TDZ에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 보인다.

var,let,const,function, function*,class 키워드를 사용하여 선언시 호이스팅됨 `모든 선언문은 런타임 이전에 먼저 실행되기 때문임`

## 25.4 인스턴스 생성

```javascript
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```
클래스는 생성자 함수이며, new 연산자와 함께 호출되어 인스턴스를 생성한다.

`클래스의 존재이유 = 인스턴스를 생성하는 것 = new 연산자`

```javascript
class Person {}

// 클래스를 new 연산자 없이 호출하면 타입 에러가 발생한다.
const me = Person();
// TypeError: Class constructor Person cannot be invoked without 'new'
```

new 연산 없어 ? 에러 삐삐

```javascript
const Person = class MyClass {};

// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
const me = new Person();

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자다.
console.log(MyClass); // ReferenceError: MyClass is not defined

const you = new MyClass(); // ReferenceError: MyClass is not defined
```

클래스 표현식으로 정의된 클래스의 경우 위 예제와 같이 클래스를 가리키는 식별자(person)을 사용해 인스턴스를 생성하지 않고 기명 클래스 표현식의 클래스 이름(MyClass)을 사용해 인스턴스를 생성하면 에러가 발생한다. (MyClass로는 접근 불가 !! 왜냐 스코프땜시임) 이는 기명 함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 `외부 코드에서 접근 불가능하기 때문이다`. 

## 25.5 메서드 

> 클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다. 클래스 몸체에서 정의할 수 있는 메서드는 constructor 생성자 , 프로토타입 메서드, 정적 메서드 세가지 이다

### 25.5.1 constructor 생성자

인스턴스를 생성하고 초기화하기 위한 특수 메서드이다. 이름을 변경할 수 없다 !

함수와 동일하게 프로토타입과 연결되어 자신의 스코프 체인을 구성한다.
```js
class Person {
  constructor(name) {
    this.name = name;
  }
}

const person1 = new Person('John');
```
new Person('John')은 Person의 인스턴스를 생성하며, 이 인스턴스는 Person 클래스(또는 생성자 함수)의 모든 속성과 메서드를 상속받게 된다.


new Person('John')은 Person의 인스턴스를 생성 과정

1. 새로운 객체를 생성.
2. 새로운 객체의 프로토타입을 Person.prototype으로 설정한다. 이로써 새 객체는 Person.prototype에 정의된 모든 메서드와 속성에 접근할 수 있게 됨.
Person 생성자 함수를 호출하고, 이 때 this를 새로 생성된 객체에 바인딩해준다. Person 생성자 함수 내에서 this를 사용하여 새 객체의 속성을 설정할 수 있다.
3. 생성자 함수에서 반환된 객체가 새 인스턴스가 된다. 명시적으로 다른 객체를 반환하지 않는 한, this에 바인딩된 새 객체가 반환된다.

`constructor 메서드는 클래스로부터 생성되는 인스턴스의 초기화를 담당`

생성자 함수와 마찬가지로 constructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 된다. constructor 내부의 this는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킨다. 

constructor는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다. 다시 말해, 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다. 

클래스의 constructor 메서드와 프로토타입의 constructor 프로퍼티는 이름이 같아 혼동하기 쉽지만 직접적인 관련이 없다. 표 로토타입의 constructor 프로퍼티는 모든 프로토타입이 가지고 있는 프로퍼티이며, 생성자 함수를 가리킨다. 

constructor는 생성자 함수와 유사하지만 몇 가지 차이가 있다. 

1.  constructor는 클래스 내에 최대 한 개만 존재할 수 있다. 
만약 클래스가 2개 이상의 constructor를 포함하면 문법 에러SyntaxError가 발생한다.  constructor는 생략할 수 있다.
```javascript
class Person {
  constructor() {}
  constructor() {}
}
// SyntaxError: A class may only have one constructor
```



2.  constructor를 생략한 클래스는 빈 constructor에 의해 빈 객체를 생성한다. 

```javascript
class Person {}
```

 
3. 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.

```javascript
class Person {
  // constructor를 생략하면 다음과 같이 빈 constructor가 암묵적으로 정의된다.
  constructor() {}
}

// 빈 객체가 생성된다.
const me = new Person();
console.log(me); // Person {}
```

3. 인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티의 초기값을 전달하려면 다음과 같이 constructor 에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달한다. 이때 초기값은 constructor의 매개변수에게 전달된다. 

```javascript
class Person {
  constructor() {
    // 고정값으로 인스턴스 초기화
    this.name = 'Lee';
    this.address = 'Seoul';
  }
}

// 인스턴스 프로퍼티가 추가된다.
const me = new Person();
console.log(me); // Person {name: "Lee", address: "Seoul"}
```
  
4. 이처럼 constructor 내에서는 인스턴스의 생성과 동시에 인스턴스 프로퍼티 추가를 통해 인스턴스의 초기화를 실행한다. 따라서 인스턴스를 초기화 하려면 constructor를 생략해서는 안 된다.

```javascript
class Person {
  constructor(name, address) {
    // 인수로 인스턴스 초기화
    this.name = name;
    this.address = address;
  }
}

// 인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
const me = new Person('Lee', 'Seoul');
console.log(me); // Person {name: "Lee", address: "Seoul"}
```
   

constructor는 별도의 반환문을 갖지 않아야 한다. 이는 17.2.3절 "생성자 함수의 인스턴스 생성 과정"에서 을 반환하기 때문이다 살펴보았듯이 new 연산자와 함께 클래스가 호출되면 생성자 함수와 동일하게 암묵적으로 this, 즉 인스턴스를 반환하기 때문인다.

만약 this가 아닌 객체를 명시적으로 반환하면 this, 즉 인스턴스가 반환되지 못하고 return 문에 명시한 객체가 반환된다

그럼, 명시적으로 원시값을 반환하면 ?

```javascript
class Person {
  constructor(name) {
    this.name = name;

    // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
    return {};
  }
}

// constructor에서 명시적으로 반환한 빈 객체가 반환된다.
const me = new Person('Lee');
console.log(me); // {}
```

원시값 반환 무시 - 암묵적으로 this 반환

`constructor 내부에서 return문 반드시 생략할 것.`

```javascript
class Person {
  constructor(name) {
    this.name = name;

    // 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환된다.
    return 100;
  }
}

const me = new Person('Lee');
console.log(me); // Person { name: "Lee" }
```

### 25.5.2 프로토타입 메서드

생성자 함수를 사용하여 인스턴스를 생성하는 경우 -> 프로토타입 메서드를 생성하기 위해서는 명시적으로 프로토타입에 메서드를 추가해야 한다.

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function () { //명시적으로 추가
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```
클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다. 

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```

생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.

```javascript
// me 객체의 프로토타입은 Person.prototype이다.
Object.getPrototypeOf(me) === Person.prototype; // -> true
me instanceof Person; // -> true

// Person.prototype의 프로토타입은 Object.prototype이다.
Object.getPrototypeOf(Person.prototype) === Object.prototype; // -> true
me instanceof Object; // -> true

// me 객체의 constructor는 Person 클래스다.
me.constructor === Person; // -> true
```

클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 된다. 

인스턴스는 프로토타입 메서드를 상속받아 사용할 수 있다. 

프로토타입 체인은 기존의 모든 객체 생성 방식(객체 리터럴, 생성자 함수, Object.create 메서드 등)뿐만 아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용된다. 

생성자 함수의 역할을 클래스가 할 뿐이다. 결국 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수라고 볼 수 있다. 

`다시 말해, 클래스는 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 메커니즘이다.` 

### 25.5.3 정적 메서드

정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다. 

생성자 함수의 경우 정적 메서드를 생성하기 위해서는 다음과 같이 명시적으로 생성자 함수에 메서드를 추가 해야 한다.

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 정적 메서드
Person.sayHi = function () {
  console.log('Hi!');
};

// 정적 메서드 호출
Person.sayHi(); // Hi!
```

클래스에서는 메서드에 static을 붙이면 정적 메서드(클래스 메서드)가 된다.

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log('Hi!');
  }
}
```

정적 메서드는 클래스에 바인딩된 메서드가 된다. 클래스는 함수 객체로 평가되므로 자신의 프로퍼티/메서드를 소유할 수 있다. 클래스는 클래스 정의(클래스 선언문이나 클래스 표현식)가 평가되는 시점에 함수 객체가 되므로 인스턴스와 달리 별다른 생성 과정이 필요 없다. 따라서 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출할 수 있다. 정적 메서드는 프로토타입 메서드처럼 인스턴스로 호출하지 않고 클래스로 호출한다.


```javascript
// 정적 메서드는 클래스로 호출한다.
// 정적 메서드는 인스턴스 없이도 호출할 수 있다.
Person.sayHi(); // Hi!
```
정적 메서드는 인스턴스로 호출할 수 없다. 정직 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 제인 상에 존재하지 않기 때문이다. 다시 말해, 인스턴스의 프로토타입 체인 상에는 클래스가 존재하지 않기 때문 에 인스턴스로 클래스의 메서드를 상속받을 수 없다. 

```javascript
// 인스턴스 생성
const me = new Person('Lee');
me.sayHi(); // TypeError: me.sayHi is not a function
```

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이

정적 메서드와 프로토타입 메서드는 무엇이 다르며, 무엇을 기준으로 구분하여 정의해야 할지 생각해 보자 정적 메서드와 프로토타입 메서드의 차이는 다음과 같다. 
1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다. 
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다. 
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다. 

다음 예제를 살펴보자.

```javascript
class Square {
  // 정적 메서드
  static area(width, height) {
    return width * height;
  }
}

console.log(Square.area(10, 10)); // 100
```

정적 매서드 area는 2개의 인수를 전달받아 면적을 계산한다. 이때 정적 메서드 area는 인스턴스 프로퍼티를 창조하지 않는다. 만약 인스턴스 프로퍼티를 참조해야 한다면 정적 메서드 대신 프로토타입 메서드를 사용해야한다. 


```javascript
class Square {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  // 프로토타입 메서드
  area() {
    return this.width * this.height;
  }
}

const square = new Square(10, 10);
console.log(square.area()); // 100
```

메서드 내부의 this는 메서드를 소유한 객체가 아니라 메서드를 호출한 객체, 즉 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체에 바인딩된다. 프로토타입 메서드는 인스턴스로 호출해야 하므로 프로토타입 메서드 내부의 this는 프로토타입 메서드 를 호출한 인스턴스를 가리킨다. 

위 예제의 경우 square 객체로 프로토타입 메서드 area를 호출했기 때문에 area 내부의 this는 square 객체를 가리킨다. 정적 메서드는 클래스로 호출해야 하므로 정적 메서드 내부의 this는 인스턴스가 아닌 클레스를 가리킨다. 즉, 프로토타입 메서드와 정적 메서드 내부의 this 바인딩이 다르다. 따라서 메서드 내부에서 인스턴스 프로퍼티를 참조할 필요가 있다면 this를 사용해야 하며, 이러한 경우 프 로토타입 메서드로 정의해야 한다.

하지만 메서드 내부에서 인스턴스 프로퍼티를 참조해야 할 필요가 없다면 this를 사용하지 않게 된다. 물론 메서드 내부에서 this를 사용하지 않더라도 프로토타입 메서드로 정의할 수 있다. 하시만 반드시 인스 턴스를 생성한 다음 인스턴스로 호출해야 하므로 this를 사용하지 않는 메서드는 정적 메서드로 정의하는 것 이 좋다. 표준 빌트인 객체인 Math, Number, JSON, Object, Reflect 등은 다양한 정적 메서드를 가지고 있다. 이들 정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수다. 예를 들어, 전달받은 인수 중에서 가장 큰 수를 반환하는 정적 메서드 Math.max는 인스턴스와 상관없이 애플리케이션 전역에서 사용할 유틸리티 함수다. 

```javascript
// 표준 빌트인 객체의 정적 메서드
Math.max(1, 2, 3);          // -> 3
Number.isNaN(NaN);          // -> true
JSON.stringify({ a: 1 });   // -> "{"a":1}"
Object.is({}, {});          // -> false
Reflect.has({ a: 1 }, 'a'); // -> true
```

클래스 또는 생성자 함수를 하나의 네임스페이스로 사용하여 정적 메서드를 모아 놓으면 이 를 충돌 가능성을 줄여 주고 관련 함수들을 구조화할 수 있는 효과가 있다. 이 같은 이유로 정적 메서드는 에 플리케이션 전역에서 사용할 유틸리티 함수를 전역 함수로 정의하지 않고 메서드로 구조화할 때 유용하다. 

> ES6에서는 2142절 "빌트인 전역 함수'에서 살펴본 isFinite, isNaN parseFloat, parseInt 등의 빌트인 전역 함수를 표준 빌 트인 객체 Number의 정적 메서드로 추가 구현했다. Number의 정적 메서드 isFinite, isNaN, parseFloat, parseInt는 빌트 인 전액 함수 isFinite, isNaN, parseFloat, parseInt보다 엄격하다. 이에 대해서는 28장 "Number"에서 자세히 살펴보자. 

### 25.5.5 클래스에서 정의한 메서드의 특징

클래스에서 정의한 메서드는 다음과 같은 특징을 갖는다. 
1. function 키워드를 생략한 메서드 축약 표현을 사용한다. 
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다. 
3. 암묵적으로 strict mode로 실행된다.' 
4. for... in 문이나 Object.keys 메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다." 5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.


### 25.6 클래스의 인스턴스 생성 과정

연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 클래스의 내부 메서드 [Construct]]가요 출된다. 클래스는 new 연산자 없이 호출할 수 없다. 이때 17,2.3절 "생성자 함수의 인스턴스 생성 과정에서 살펴본 바와 유사하게 다음과 같은 과정을 거쳐 인스턴스가 생성된다." 
1. 인스턴스 생성과 this 바인딩 
   연산자와 함께 클래스를 호출하면 constructor의 내부 코드가 실행되기에 앞서 암묵적으로 빈 객체가 생성된다. 이 빈 객체가 바로 (아직 완성되지는 않았지만) 클래스가 생성한 인스턴스다. 이때 클래스가 생성 한인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정된다. 그리고 암묵적으 로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다. 따라서 constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킨다. 
2. 인스턴스 초기화 
   constructor의 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다. 즉, this에 바인딩되 이 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 같을 초기화한다. 만약 constructor가 생략되었다면 이 과정도 생략된다. 
3. 인스턴스 반환 
   클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  }
}
```


## 25.7 프로퍼티

### 25.7.1 인스턴스 프로퍼티 

인스턴스 프로퍼티는 constructor 내부에서 정의한다.

```javascript
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name;
  }
}

const me = new Person('Lee');
console.log(me); // Person {name: "Lee"}
```

constructor 내부 코드가 실행되기 이전에 constructor 내부의 this에는 이미 클래스가 암묵적으로 생성한 인스턴스가 생성자 함수에서 생성자 함수가 생성할 인스턴스 프로퍼티를 정하는 것과 마찬가지로 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다. 이로써 클래스가 암묵적으로 생성한 빈 객체 즉, 인스턴스에 프로퍼티가 추가되어 인스턴스가 초기화된다. 

```javascript
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name; // name 프로퍼티는 public하다.
  }
}

const me = new Person('Lee');

// name은 public하다.
console.log(me.name); // Lee
```
 constructor 내부에서 this에 추가한 프로퍼티는 언제나 클레스가 생성한 인스턴스 프로퍼티가 된다. es6의 클래스는 다른 객체지향 언어처럼 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않는다. 따라서 인스턴스 프로처티는 언제나 public하다. 다행히도 private 프로를 정의 할 수 있는 사양이 현재 제안 중에 있다. 


### 25.7.2 접근자 프로퍼티

`접근자 프로미티`에서 살펴보았듯이 접근자 프로퍼티는 자체적으로는 값([Value] 내부 슬롯)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수 구성된 프로퍼티다.

```javascript
const person = {
  // 데이터 프로퍼티
  firstName: 'Ungmo',
  lastName: 'Lee',

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    // 배열 디스트럭처링 할당: "36.1. 배열 디스트럭처링 할당" 참고
    [this.firstName, this.lastName] = name.split(' ');
  }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(`${person.firstName} ${person.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName); // Heegun Lee

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(person, 'fullName'));
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}
```
객체 리터럴은 클래스로 표한하면 ?


```javascript
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  }
}

const me = new Person('Ungmo', 'Lee');

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(`${me.firstName} ${me.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
me.fullName = 'Heegun Lee';
console.log(me); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(me.fullName); // Heegun Lee

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(Person.prototype, 'fullName'));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수, 즉 getter 함수와 setter 함수로 구성되어 있다. 

- getter는 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용한다. getter는 메서드 이름 앞에 get 키워드를 사용해 정의한다. 
- setter는 인스턴스 프로퍼티에 값을 할당할 때마 다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용한다. setter는 메서드 이름 앞에 set 키워드를 사용해 정의한다. 


getter와 setter 이름은 인스턴스 프로퍼티처럼 사용된다. 다시 말해 getter는 호출하는 것이 아니라 프로퍼티처럼 참조하는 형식으로 사용하며, 참조 시에 내부적으로 getter가 호출된다. setter도 호출하는 것이 아니라 프로퍼티처럼 값을 할당하는 형식으로 사용하며, 할당 시에 내부적으로 setter가 호출된다.

getter는 이름 그대로 무언가를 취득할 때 사용하므로 `반드시 무언가를 반환`해야 하고 setter는 무언가를 프로퍼티에 할당해야 할 때 사용하므로 `반드시 매개변수`가 있어야 한다. setter는 단 하나의 값만 할당받기 때문에 `단 하나의 매개변수만` 선언할 수 있다. 

클래스의 메서드는 기본적으로 프로토타입 메서드가 된다. 따라서 클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 된다 

```javascript
// Object.getOwnPropertyNames는 비열거형(non-enumerable)을 포함한 모든 프로퍼티의 이름을 반환한다.(상속 제외)
Object.getOwnPropertyNames(me); // -> ["firstName", "lastName"]
Object.getOwnPropertyNames(Object.getPrototypeOf(me)); // -> ["constructor", "fullName"]
```



### 25.7.3 클래스 필드 정의 제안 

**클래스 필드(필드 또는 멤버)** : 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티.

 클래스 기반 객체지향 언어인 자바의 클래스 정의를 살펴보자, 자바의 클래스 필드는 마치 클래스 내부에서 변수처럼 사용된다.

```java
// 자바의 클래스 정의
public class Person {
  // ① 클래스 필드 정의
  // 클래스 필드는 클래스 몸체에 this 없이 선언해야 한다.
  private String firstName = "";
  private String lastName = "";

  // 생성자
  Person(String firstName, String lastName) {
    // ③ this는 언제나 클래스가 생성할 인스턴스를 가리킨다.
    this.firstName = firstName;
    this.lastName = lastName;
  }

  public String getFullName() {
    // ② 클래스 필드 참조
    // this 없이도 클래스 필드를 참조할 수 있다.
    return firstName + " " + lastName;
  }
}
```

자바스크립트의 클래스에서 인스턴스 프로퍼티를 선언하고 초기화하려면 반드시 constructor 내부에서 this에 프로퍼티를 추가해야 한다. 

하지만, 자바의 클래스에서는 위 예제의 ①과 같이 클래스 필드를 마치면 수처럼 클래스 몸체에 this 없이 선언한다. 

또한 자바스크립트의 클래스에서 인스턴스 프로퍼티를 참조하려면 반드시 this를 사용하여 참조해야 한다. 

하지만 자바의 클래스에서는 위 예제의와 같이 this를 생략해도 클래스 필드를 참조할 수 있다. 클래스 기반 객체지향 언어의 this는 언제나 클래스가 생성할 인스턴스를 가리킨다. 

위 예제의 3 과 같이 this는 주로 클래스 필드가 생성자 또는 메서드의 매개변수 이름과 동일할 때 클래스 필드임을 명확히 하거 위해 사용한다. 자바스크립트의 클래스 몸체에는 메서드만 선언할 수 있다. 

따라서 클래스 몸체에 자바와 유사하게 클래스 필드를 선언하면 문법 에러SyntaxError가 발생한다 

```javascript
class Person {
  // 클래스 필드 정의
  name = 'Lee';
}

const me = new Person('Lee');
```

하지만 위 예제를 최신 브라우저(Chrome 72 이상) 또는 최신 Node.js(버전 12 이상)에서 실행하면 문법 에러가 발생하지 않고 정상 동작한다.
최신 브라우저와 최신 Node.js에서는 다음 예제와 같이 클래스 필드를 클래스 몸체에 정의할 수 있다.

클래스 몸체에서 클래스 필드를 정의하는 경우 this에 클래스 필드를 바인딩해서는 안 된다. this는 클래스의 constructor와 메서드 내에서만 유효하다

```javascript
class Person {
  // this에 클래스 필드를 바인딩해서는 안된다.
  this.name = ''; // SyntaxError: Unexpected token '.'
}
```

클래스 필드를 창조하는 경우 자바와 같은 클래스 기반 객체지향 언어에서는 this를 생략할 수 있으나 자바 스크립트에서는 this를 반드시 사용해야 한다.


```javascript
class Person {
  // 클래스 필드
  name = 'Lee';

  constructor() {
    console.log(name); // ReferenceError: name is not defined
  }
}

new Person();
```

클래스 필드에 초기값을 할당하지 않으면 undefined를 갖는다

```javascript
class Person {
  // 클래스 필드를 초기화하지 않으면 undefined를 갖는다.
  name;
}

const me = new Person();
console.log(me); // Person {name: undefined}
```

인스턴스를 생성할 때 외부의 초기값으로 클래스 필드를 초기화해야 할 필요가 있다면 constructor에서 클 래스 필드를 초기화해야 한다.

```javascript
class Person {
  name;

  constructor(name) {
    // 클래스 필드 초기화.
    this.name = name;
  }
}

const me = new Person('Lee');
console.log(me); // Person {name: "Lee"}
```

이처럼 인스턴스를 생성할 때 클래스 필드를 초기화할 필요가 있다면 constructor 밖에서 클래스 필드를 정의할 필요가 없다. 클래스 필드를 초기화할 필요가 있다면 어차피 constructor 내부에서 클래스 필드를 참조하여 초기값을 할당해야 한다. 이때 this, 즉 클래스가 생성한 인스턴스에 클래스 필드에 해당하는 프로퍼티 가 없다면 자동 추가되기 때문이다.

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
}

const me = new Person('Lee');
console.log(me); // Person {name: "Lee"}
```

함수는 일급 객체이므로 함수를 클래스 필드에 할당할 수 있다. 따라서 클래스 필드를 통해 메서드를 정의할 수도 있다


```javascript
class Person {
  // 클래스 필드에 문자열을 할당
  name = 'Lee';

  // 클래스 필드에 함수를 할당
  getName = function () {
    return this.name;
  }
  // 화살표 함수로 정의할 수도 있다.
  // getName = () => this.name;
}

const me = new Person();
console.log(me); // Person {name: "Lee", getName: ƒ}
console.log(me.getName()); // Lee
```

이처럼 클래스 필드에 함수를 할당하는 경우, 이 함수는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다. 모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문이다. 따라서 클래스 필드에 함수를 할당하는 것은 권장 하지 않는다.

클래스 필드에 화살표 향수를 할당하여 화살표 함수 내부의 this가 인스턴스를 가리키게 하는 경우도 있다.

```html
<!DOCTYPE html>
<html>
<body>
  <button class="btn">0</button>
  <script>
    class App {
      constructor() {
        this.$button = document.querySelector('.btn');
        this.count = 0;

        // increase 메서드를 이벤트 핸들러로 등록
        // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킨다.
        // 하지만 increase는 화살표 함수로 정의되어 있으므로
        // increase 내부의 this는 인스턴스를 가리킨다.
        this.$button.onclick = this.increase;

        // 만약 increase가 화살표 함수가 아니라면 bind 메서드를 사용해야 한다.
        // $button.onclick = this.increase.bind(this);
      }

      // 인스턴스 메서드
      // 화살표 함수 내부의 this는 언제나 상위 컨텍스트의 this를 가리킨다.
      increase = () => this.$button.textContent = ++this.count;
    }
    new App();
  </script>
</body>
</html>
```

인스턴스가 여러 개 생성된다면 이 방법도 메모리의 손해를 감수할 수밖에 없다. 

인스턴스를 생성 할 때 외부 초기값으로 클래스 필드를 초기화할 필요가 있다면 constructor에서 인스턴스 프로퍼티를 정의하는 기존 방식을 사용하고 인스턴스를 생성할 때 외부 초기값으로 클래스 필드를 초기화할 필요가 없다면 기존의 constructor에서 인스턴스 프로퍼티를 정의하는 방식과 클래스 필드 정의 제안 모두 사용할 수 있다.


### 25.7.4 private 필드 정의 제안 

자바스크립트는 캡슐화를 완전하게 지원하지 않는다. ES6 의 클래스도 생성자 함수와 마찬가지로 다른 클래스 기반 객체지향 언어에서는 지원하는 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않는다. 따라서 인스턴스 프로퍼티는 인스턴스를 통해 클래스 외부에서 언제나 참조할 수 있다. 즉, 언제나 public이다. 


```javascript
class Person {
  constructor(name) {
    this.name = name; // 인스턴스 프로퍼티는 기본적으로 public하다.
  }
}

// 인스턴스 생성
const me = new Person('Lee');
console.log(me.name); // Lee
```

 클래스 필드 정의 제안을 사용하더라도 클래스 필드는 기본적으로 public하기 때문에 외부에 그대로 노출 된다. 

 ```javascript
class Person {
  name = 'Lee'; // 클래스 필드도 기본적으로 public하다.
}

// 인스턴스 생성
const me = new Person();
console.log(me.name); // Lee
```
private 필드의 선두에는 #을 붙여준다. private 필드를 참조할 때도 #을 붙어주어야 한다.

```javascript
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person('Lee');

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxError: Private field '#name' must be declared in an enclosing class
```

> **타입스크립트** 
> C#의 창시자인 덴마크 출신 소프트웨어 엔지니어 아네르스 하일스베르 Anders Helsberg가 개발을 주도한 자바스크립트의 상위 확장 타입스크립트는 클래스 기반 객체지향 언어가 지원하는 접근 제한자인 public, private, protected를 모두 지원하며, 의미 또한 기본적으로 동일하다."


 public 필드는 어디서든 참조할 수 있지만 private 필드는 클래스 내부에서만 참조할 수 있다. 접근자 프로퍼티를 통해 간접적으로 접근 가능하다.

 ```javascript
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    this.#name = name;
  }

  // name은 접근자 프로퍼티다.
  get name() {
    // private 필드를 참조하여 trim한 다음 반환한다.
    return this.#name.trim();
  }
}

const me = new Person(' Lee ');
console.log(me.name); // Lee
```

private 필드는 반드시 클래스 몸체에 정의해야 한다. private 필드를 직접 constructor에 정의하면 에러가 발생한다. 

```javascript
class Person {
  constructor(name) {
    // private 필드는 클래스 몸체에서 정의해야 한다.
    this.#name = name;
    // SyntaxError: Private field '#name' must be declared in an enclosing class
  }
}
```


### 25.7.5 static 필드 정의 제안

클래스에는 static 키워드를 사용하여 정적 메서드를 정의 할 수 있다. 하지만 static 키워드를 사용하여 정적 필드를 정의할 수는 없었다. 하지만 static public 필드, static private 필드, static private 메서드를 정의할 수 있는 새로운 표준 사양인 "Static class features""가 제안되어 있다. 이 제안 중에서 static public/private 필드는 2021년 1월 현재, 최신 브라우저(Chrome 72 이상)와 최신 Node.js(버전 12 이상)에 이미 구현되어 있다.

```javascript
class MyMath {
  // static public 필드 정의
  static PI = 22 / 7;

  // static private 필드 정의
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI); // 3.142857142857143
console.log(MyMath.increment()); // 11
```

## 25.8 상속에 의한 클래스 확장

### 25.8.1 클래스 상속과 생성자 함수 상속

상속에 의한 클래스 확장은 지금까지 살펴본 프로토타입 기반 상속과는 다른 개념이다. 프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자산을 상속받는 개념이지만 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.

클래스와 생성자 함수는 인스턴스를 생성할 수 있는 함수라는 점에서 매우 유사하다. 하지만 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 기본적으로 제공되지만 생성자 함수는 그렇지 않다.

예를 들어, 동물을 추상화에래스와 새와 사자를 추상화한 Bird, Lion 클래스를 각각 정의한다고 생각해보자. 새와 사자는 동물에 속하므로 동물의 속성을 갖는다. 하지만 새와 사자는 자신만의 고유한 속성 도 갖는다. 이때 Animal 클래스는 동물의 속성을 표현하고 Bird, Lion 클래스는 상속을 통해 Animal 클래스 의 속성을 그대로 사용하면서 자신만의 고유한 속성만 추가하여 확장할 수 있다.


Bird 클래스와 Lion 클래스는 상속을 통해 Animal 클래스의 속성을 그대로 사용하고 자신만의 고유한 속성 을 추가하여 확장했다. 이처럼 상속에 의한 클래스 확장은 코드 재사용 관점에서 매우 유용하다. 



상속을 통해 Animal 클래스를 확장한 Bird 클래스


```javascript
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() { return 'eat'; }

  move() { return 'move'; }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
  fly() { return 'fly'; }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat());  // eat
console.log(bird.move()); // move
console.log(bird.fly());  // fly
```


클래스는 상속을 통해 다른 클래스를 확장할 수 있는 문법인 extends 키워드가 기본적으로 제공된다. extends 키워드를 사용한 클래스 화장은 간편하고 직관적이다. 하지만 생성자 함수는 클래스와 같이 상속을 통해 다른 생성자 함수를 확장할 수 있는 문법이 제공되지 않는다

자바스크립트는 클래스 기반 언어가 아니므로 생성자 함수를 사용하여 클래스를 흉내 내려는 시도를 권장하지는 않지만 의사 클래스 상속 패턴을 사용하여 상속에 의한 클래스 확장을 흉내 내기도 했다.
```javascript
// 의사 클래스 상속(pseudo classical inheritance) 패턴
var Animal = (function () {
  function Animal(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  Animal.prototype.eat = function () {
    return 'eat';
  };

  Animal.prototype.move = function () {
    return 'move';
  };

  return Animal;
}());

// Animal 생성자 함수를 상속하여 확장한 Bird 생성자 함수
var Bird = (function () {
  function Bird() {
    // Animal 생성자 함수에게 this와 인수를 전달하면서 호출
    Animal.apply(this, arguments);
  }

  // Bird.prototype을 Animal.prototype을 프로토타입으로 갖는 객체로 교체
  Bird.prototype = Object.create(Animal.prototype);
  // Bird.prototype.constructor을 Animal에서 Bird로 교체
  Bird.prototype.constructor = Bird;

  Bird.prototype.fly = function () {
    return 'fly';
  };

  return Bird;
}());

var bird = new Bird(1, 5);

console.log(bird); // Bird {age: 1, weight: 5}
console.log(bird.eat());  // eat
console.log(bird.move()); // move
console.log(bird.fly());  // fly
```



### 25.8.2 extends 키워드 

 상속을 통해 클래스를 확장하려면 extends 기워드를 사용하여 상속받을 클래스를 정의한다.

 ```javascript
// 수퍼(베이스/부모)클래스
class Base {}

// 서브(파생/자식)클래스
class Derived extends Base {}
```

상속을 통해 확장된 클래스를 서브클래스라 부르고, 서브클래스에게 상속된 클래스를 수퍼클래스라 부른다. 서브클래스를 파생 클래스 또는 자식 클래스, 수퍼클래스를 베이스 클래스 또는 부모 클래스라고 부르기도 한다. extends의 역할은 수퍼클래스와 서브클래스 간의 상속 관계를 설정하는 것이다.

수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인뿐 아니라 클래스 간의 프로토타입 체인도 생성한다. 이를 통해 프로토타입 메서드, 정적 메서드 모두 상속이 가능하다.


### 25.8.3 동적 상속 

EXTENDS 키워드는 클래스뿐 아니라 생성자 함수를 상속받아 클래스를 확장할 수도 있다. 단, extends 키워드 앞에는 반드시 클래스가 와야 한다. 


```javascript
// 생성자 함수
function Base(a) {
  this.a = a;
}

// 생성자 함수를 상속받는 서브클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived); // Derived {a: 1}
```

extends 키워드 다음에는 클래스뿐만이 아니라 [Construct] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를 통해 동적으로 상속받을 대상을 결정할 수 있다

```javascript
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```



### 25.8.4 서브클래스의 constructor 

클래스에서 constructor를 생략하면 클래스에 다음과 같이 비어 있는 constructor가 암묵적으로 정의된다.

```javascript
constructor() {}
```

서브클래스에서 constructor를 생략하면 클래스에 다음과 같은 constructor가 암묵적으로 정의된다. args는 new 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트다. 


```javascript
constructor(...args) { super(...args); }
```
super()는 수퍼클래스의 constructor(super-constructor)를 호출하여 인스턴스를 생성한다.

수퍼클래스와 서브클래스 모두 constructor를 생략했다.

```javascript
// 수퍼클래스
class Base {}

// 서브클래스
class Derived extends Base {}
```

위 예제의 클래스에는 다음과 같이 암묵적으로 constructor가 정의된다.

```javascript
// 수퍼클래스
class Base {
  constructor() {}
}

// 서브클래스
class Derived extends Base {
  constructor() { super(); }
}

const derived = new Derived();
console.log(derived); // Derived {}
```

위 예세와 같이 수퍼클래스와 서브클래스 모두 constructor를 생략하면 빈 객체가 생성된다. 프로퍼티를 소유하는 인스턴스를 생성하려면 constructor 내부에서 인스턴스에 프로퍼티를 추가해야 한다. 



### 25.8.5 super 키워드

super 키워드는 함수처럼 호출할 수도 있고 this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다. 

super 는 다음과 같이 동작한다. 

- super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다. 
- super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다. 

super 호출

 `super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다.` 다음 예제와 같이 수퍼클래스의 constructor 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면 서브클래스의 constructor를 생략할 수 있다. 
 이때 new 연산자와 함께 서브클래스를 호출하면서 전달한 인수는 모두 서브클래스에 암묵적으로 정의된 constructor의 super 호출을 통해 수퍼클래스의 constructor에 전달된다. 

```javascript
// 수퍼클래스
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {
  // 다음과 같이 암묵적으로 constructor가 정의된다.
  // constructor(...args) { super(...args); }
}

const derived = new Derived(1, 2);
console.log(derived); // Derived {a: 1, b: 2}
```

다음 예제와 같이 수퍼클래스에서 추가한 프로퍼티와 서브클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 constructor를 생략할 수 없다. 이때 new 연산자와 함께 서브클래스를 호출하면서 전달한 인수 중에서 수퍼클래스의 constructor에 전달할 필요가 있는 인수는 서브클래스의 constructor에 시호출하는 super를 통해 전달한다.

```javascript
// 수퍼클래스
class Base {
  constructor(a, b) { // ④
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {
  constructor(a, b, c) { // ②
    super(a, b); // ③
    this.c = c;
  }
}

const derived = new Derived(1, 2, 3); // ①
console.log(derived); // Derived {a: 1, b: 2, c: 3}
```
new 연산자와 함께 Derived 클래스를 호출(①)하면서 전달한 인수 1. 2. 3은 Derived 클래스의 constructor()에 전달되고 super 호출(3)을 통해 Base 클래스의 constructor(④)에 일부가 전달된다. 이처럼 인스턴스 초기화를 위해 전달한 인수는 수퍼클래스와 서브클래스에 배분되고 상속 관계의 두 클래스 는 서로 협력하여 인스턴스를 생성한다. super를 호출할 때 주의할 사항은 다음과 같다. 

#### 01. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super를 호출해야 한다

```javascript
class Base {}

class Derived extends Base {
  constructor() {
    // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
    console.log('constructor call');
  }
}

const derived = new Derived();
```

#### 02. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다

```javascript
class Base {}

class Derived extends Base {
  constructor() {
    // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
    this.a = 1;
    super();
  }
}

const derived = new Derived(1);
```

#### 03. super는 반드시 서브클래스의 constructor에서만 호출한다. 서브클래스가 아닌 클래스의 constructor나 함수에 서 super를 호출하면 에러가 발생한다.

```javascript
class Base {
  constructor() {
    super(); // SyntaxError: 'super' keyword unexpected here
  }
}

function Foo() {
  super(); // SyntaxError: 'super' keyword unexpected here
}
```

### super 참조 

`메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다. `

#### 01. 서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드 sayHi를 가리킨다. 

```javascript
// 수퍼클래스
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

// 서브클래스
class Derived extends Base {
  sayHi() {
    // super.sayHi는 수퍼클래스의 프로토타입 메서드를 가리킨다.
    return `${super.sayHi()}. how are you doing?`;
  }
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```


super 참조를 통해 수퍼클래스의 메서드를 참조하려면 super가 수퍼클래스의 메서드가 바인딩된 객체, 즉 수퍼클래스의 prototype 프로피티에 바인딩된 프로토타입을 참조할 수 있어야 한다. 

위 예제는 다음 예제와 동일하게 작한다. 

```javascript
// 수퍼클래스
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  sayHi() {
    // __super는 Base.prototype을 가리킨다.
    const __super = Object.getPrototypeOf(Derived.prototype);
    return `${__super.sayHi.call(this)} how are you doing?`;
  }
}
```


super는 자신을 참조하고 있는 메서드(위 예제의 경우 Derived의 sayHi)가 바인딩되어 있는 객체(위 예제의 경 우 Derived.prototype)의 프로토타입(위 예제의 경우 Base.prototype)을 가리킨다. 따라서 super-sayHi는 Base-prototype.sayHi를 가리킨다. 단, super.sayHi, 즉 Base-prototype.sayHi를 호출할 때 call 메서드를 사용해 this를 전달해야 한다. 

 call 메서드를 사용해 this를 전달하지 않고 Base.prototype.sayHi를 그대로 호출하면 Base.prototype. sayhi 메서드 내부의 this는 Base.prototype을 가리킨다. Base.prototype.sayHi 메서드는 프로토타입 메서드이기 때문에 내부의 this는 Base.prototype이 아닌 인스턴스를 가리켜야 한다. name 프로퍼티는 인스턴스에 존재하기 때문이다. 
 
 이처럼 super 참조가 동작하기 위해서는 super를 참조하고 있는 메서드(위 예제의 경우 Derived의 sayHi)가 바인딩되어 있는 객체(위 예제의 경우 Derived.prototype)의 프로토타입(위 예제의 경우 Base.prototype)을 찾을 수 있어야 한다. 이를 위해 메서드는 내부 슬롯 [HomeObject]를 가지며, 자신을 바인딩하고 있는 객체를 가리킨다. super 참조를 의사 코드로 표현하면 다음과 같다.


 ```javascript
/*
[[HomeObject]]는 메서드 자신을 바인딩하고 있는 객체를 가리킨다.
[[HomeObject]]를 통해 메서드 자신을 바인딩하고 있는 객체의 프로토타입을 찾을 수 있다.
예를 들어, Derived 클래스의 sayHi 메서드는 Derived.prototype에 바인딩되어 있다.
따라서 Derived 클래스의 sayHi 메서드의 [[HomeObject]]는 Derived.prototype이고
이를 통해 Derived 클래스의 sayHi 메서드 내부의 super 참조가 Base.prototype으로 결정된다.
따라서 super.sayHi는 Base.prototype.sayHi를 가리키게 된다.
*/
super = Object.getPrototypeOf([[HomeObject]])
```

 주의할 것은 ES6의 메서드 축약 표현으로 정의된 함수만이 [[HomeObject]]를 갖는다는 것이다. 


 
```javascript
const obj = {
  // foo는 ES6의 메서드 축약 표현으로 정의한 메서드다. 따라서 [[HomeObject]]를 갖는다.
  foo() {},
  // bar는 ES6의 메서드 축약 표현으로 정의한 메서드가 아니라 일반 함수다.
  // 따라서 [[HomeObject]]를 갖지 않는다.
  bar: function () {}
};
```

[[HomeObject]]를 가지는 함수만이 super 참조를 할 수 있다. 따라서 [[HomeObject]]를 가지는 ES6의 메서드 축약 표현으로 정의된 함수만이 super 참조를 할 수 있다. 단, super 참조는 수퍼클래스의 메서드를 참조하기 위해 사용하므로 서브클래스의 메서드에서 사용해야 한다. super 참조는 클래스의 전유물은 아니다. 객체 리터럴에서도 super 참조를 사용할 수 있다. 단, ES6의 메서드 축약 표현으로 정의된 함수만 가능하다.

```javascript
const base = {
  name: 'Lee',
  sayHi() {
    return `Hi! ${this.name}`;
  }
};

const derived = {
  __proto__: base,
  // ES6 메서드 축약 표현으로 정의한 메서드다. 따라서 [[HomeObject]]를 갖는다.
  sayHi() {
    return `${super.sayHi()}. how are you doing?`;
  }
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

#### 02. 서브클래스의 정적 메서드 내에서 super.sayHi는 수퍼클래스의 정적 메서드 sayHi를 가리킨다. 

```javascript
// 수퍼클래스
class Base {
  static sayHi() {
    return 'Hi!';
  }
}

// 서브클래스
class Derived extends Base {
  static sayHi() {
    // super.sayHi는 수퍼클래스의 정적 메서드를 가리킨다.
    return `${super.sayHi()} how are you doing?`;
  }
}

console.log(Derived.sayHi()); // Hi! how are you doing?
```


### 25.8.6 상속 클래스의 인스턴스 생성 과정

직사각형을 추상화한 Rectangle 클래스와 상속을 통해 Rectangle 클래스를 확장한 ColorRectangle 클래스를 정의한다.

```javascript
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  toString() {
    return `width = ${this.width}, height = ${this.height}`;
  }
}

// 서브클래스
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);
    this.color = color;
  }

  // 메서드 오버라이딩
  toString() {
    return super.toString() + `, color = ${this.color}`;
  }
}

const colorRectangle = new ColorRectangle(2, 4, 'red');
console.log(colorRectangle); // ColorRectangle {width: 2, height: 4, color: "red"}

// 상속을 통해 getArea 메서드를 호출
console.log(colorRectangle.getArea()); // 8
// 오버라이딩된 toString 메서드를 호출
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red
```

서브클래스 ColorRectangle이 new 연산자와 함께 호출되면 다음 과정을 통해 인스턴스를 생성한다. 

#### 1. 서브클래스의 super 호출

 자바스크립트 엔진은 클래스를 평가할 때 수퍼클래스와 서브클래스를 구분하기 위해 "base" 또는 "derived" 를 값으로 갖는 내부 슬롯 [[ConstructorKind]]를 갖는다.  다른 클래스를 상속받지 않는 클래스(그리고 생 성자 함수)는 내부 슬롯 [[ConstructorKind]]의 값이 "base"로 설정되지만 다른 클래스를 상속받는 서브클래스는 내부 슬롯 [[ConstructorKind]]의 값이 "derived"로 설정된다. 이를 통해 수퍼클래스와 서브클래스 는 new 연산자와 함께 호출되었을 때의 동작이 구분된다. 다른 클래스를 상속받지 않는 클래스(그리고 생성자 함수)는 new 연산자와 함께 호출되었을 때 암묵적으로 빈 객체, 즉 인스턴스를 생성하고 이를 this에 바인딩한다

하지만 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위임한다. 이것이 바로 서브클래스의 constructor에서 반드시 super를 호출해야 하는 이유다. 

서브클래스가 new 연산자와 함께 호출되면 서브클래스 constructor 내부의 super 키워드가 함수처럼 호출 된다. super가 호출되면 수퍼클래스의 constructor(super-constructor)가 호출된다. 좀 더 정확히 말하자면 수퍼클래스가 평가되어 생성된 함수 객체의 코드가 실행되기 시작한다. 만약 서브클래스 constructor 내부에 super 호출이 없으면 에러가 발생한다. 실제로 인스턴스를 생성하는 주체는 수퍼클래스이므로 수퍼클래스의 constructor를 호출하는 super가 호출되지 않으면 인스턴스를 생성 할 수 없기 때문이다.

#### 02. 수퍼클래스의 인스턴스 생성과 this 바인딩 

수퍼클래스의 constructor 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성한다. 이 빈 객체가 바로 (아직 완성되지는 않았지만) 클래스가 생성한 인스턴스다. 그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다. 따라서 수퍼클래스의 constructor 내부의 this는 생성된 인스턴스를 가리킨다. 

```javascript
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // ColorRectangle {}
    // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
    console.log(new.target); // ColorRectangle
...
```

 이때 인스턴스는 수퍼클래스가 생성한 것이다. 하지만 new 연산자와 함께 호출된 클래스가 서브클래스라는 것이 중요하다. 즉. new 연산자와 함께 호출된 함수를 가리키는 new.target은 서브클래스를 가리킨다. 따라서 인스턴스는 new. target이 가리키는 서브클래스가 생성한 것으로 처리된다. 따라서 생성된 인스턴스의 프로토타입은 수퍼클래스의 prototype 프로퍼티가 가리키는 객체(Rectangle prototype)가 아니라 new.target, 즉 서브클래스의 prototype 프로퍼티가 가리키는 객체(ColorRectangle. prototype)다

```javascript
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // ColorRectangle {}
    // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
    console.log(new.target); // ColorRectangle

    // 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정된다.
    console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype); // true
    console.log(this instanceof ColorRectangle); // true
    console.log(this instanceof Rectangle); // true
...
```

#### 3. 수퍼클래스의 인스턴스 초기화 
수퍼클래스의 constructor가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다. 즉, this에 바인딩 되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화한다.

```javascript
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // ColorRectangle {}
    // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
    console.log(new.target); // ColorRectangle

    // 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정된다.
    console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype); // true
    console.log(this instanceof ColorRectangle); // true
    console.log(this instanceof Rectangle); // true

    // 인스턴스 초기화
    this.width = width;
    this.height = height;

    console.log(this); // ColorRectangle {width: 2, height: 4}
  }
...
```

#### 4. 서브클래스 com의 복귀와 this 바인딩

 super의 호출이 종료되고 제어 흐름이 서브클래스 constructor로 돌아온다. 이때 super가 반환한 인스턴스가 this에 바인딩된다. 서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다.

```javascript
// 서브클래스
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);

    // super가 반환한 인스턴스가 this에 바인딩된다.
    console.log(this); // ColorRectangle {width: 2, height: 4}
...
```

이처럼 super가 호출되지 않으면 인스턴스가 생성되지 않으며, this 바인딩도 할 수 없다. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없는 이유가 바로 이 때문이다. 따라서 서브클래스 constructor 내부의 인스턴스 초기화는 반드시 super 호출 이후에 처리되어야 한다. 

 #### 5. 서브클래스의 인스턴스 초기화 

 super 호출 이후, 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행된다. 즉, this에 바인 딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로 퍼티를 초기화한다. 

 #### 6. 인스턴스 반환 
 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

 ```javascript
// 서브클래스
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);

    // super가 반환한 인스턴스가 this에 바인딩된다.
    console.log(this); // ColorRectangle {width: 2, height: 4}

    // 인스턴스 초기화
    this.color = color;

    // 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    console.log(this); // ColorRectangle {width: 2, height: 4, color: "red"}
  }
...
```

### 25.8.7 표준 빌트인 생성자 함수 확장

extends 키워드 다음에는 클래스뿐만이 아니라 [Construct] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. String, Number, Array 같은 표준 빌트인 객체도 [Construct] 내부 메서드를 갖는 생성자 함수이므로 extends 키워드를 사용하여 확장 할 수 있다. 

```javascript
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
  // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

// MyArray.prototype.uniq 호출
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
// MyArray.prototype.average 호출
console.log(myArray.average()); // 1.75
```

Array 생성자 함수를 상속받아 확장한 MyArray 클래스가 생성한 인스턴스는 Array.prototype과 MyArray. prototype의 모든 메서드를 사용할 수 있다.

이때 주의할 것은 Array-prototype의 메서드 중에서 map, filter와 같이 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환한다는 것이다. 


```javascript
console.log(myArray.filter(v => v % 2) instanceof MyArray); // true
```


만약 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환하지 않고 Array의 인스턴스를 반 환하면 MyArray 클래스의 메서드와 메서드 체이닝method chaining이 불가능하다.

```javascript
// 메서드 체이닝
// [1, 1, 2, 3] => [ 1, 1, 3 ] => [ 1, 3 ] => 2
console.log(myArray.filter(v => v % 2).uniq().average()); // 2
```

myArray.filter가 반환하는 인스턴스는 MyArray 클래스가 생성한 인스턴스, 즉 MyArray 타입이다. 따라서 myArray.filter가 반환하는 인스턴스로 uniq 메서드를 연이어 호출(메서드 체이닝)할 수 있다. uniq 메서드가 반환하는 인스턴스는 Array.prototype.filter에 의해 생성되었기 때문에 Array 생성자 함수가 생성한 인스턴스로 생각할 수도 있겠다. 하지만 uniq 메서드가 반환하는 인스턴스도 MyArray 타입이다. 따라서 uniq 메서드가 반환하는 인스턴스로 average 메서드를 연이어 호출(메서드 체이닝)할 수 있다. 

만약 MyArray 클래스의 uniq 메서드가 MyArray 클래스가 생성한 인스턴스가 아닌 Array가 생성한 인스턴스 를 반환하게 하려면 다음과 같이 Symbol.species를 사용하여 정적 접근자 프로퍼티를 추가한다.

```javascript
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
  // 모든 메서드가 Array 타입의 인스턴스를 반환하도록 한다.
  static get [Symbol.species]() { return Array; }

  // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);

console.log(myArray.uniq() instanceof MyArray); // false
console.log(myArray.uniq() instanceof Array); // true

// 메서드 체이닝
// uniq 메서드는 Array 인스턴스를 반환하므로 average 메서드를 호출할 수 없다.
console.log(myArray.uniq().average());
// TypeError: myArray.uniq(...).average is not a function
```




## 📌중요한 점


## 🤔궁금한 점