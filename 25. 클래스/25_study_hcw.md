# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

# 클래스와 생성자 함수 차이점

1. 클래스는 new 연산자 없이 호출하면 에러가 발생함. 생성자 함수는 new 연산자 없이 호출하면 일반 함수로서 호출됨.
2. 클래스는 extends와 super 키워드로 상속 가능. 생성자 함수는 지원하지 않음.
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작함. 생성자 함수는 함수 호이스팅이 발생하고, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생함
4. 클래스 내부에는 자동으로 strict mode가 지정되며 해제할 수 없다. 생성자 함수는 그렇지 않다.
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 열거 불가능. [[Enumerable]] 값이 false 다.

   <br>

# 클래스의 특징

클래스는 **class** 키워드를 사용하여 정의할 수 있으며, 클래스는 값으로 사용될 수 있으므로 **일급 객체**라는 것을 의미한다.

- 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능함
- 변수나 자료구조에 저장 가능
- 함수의 매개변수에게 전달할 수 있음
- 함수의 반환값으로 사용할 수 있음

`클래스 => 함수` (값처럼 사용할 수 있는 일급 객체)

   <br>

# 클래스 호이스팅

클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다.

```javascript
const Person = "";

{
  // 호이스팅이 발생하지 않는다면 ''이 출력되어야 한다.
  console.log(Person);
  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```

- 클래스는 let ,const 로 선언한 변수처럼 호이스팅 된다 => TDZ에 빠지므로 호이스팅 되지 않는 것처럼 보임.
- var, let, const function, class 키워드를 사용하여 선언된 모든 식별자는 호이스팅 됨. 모든 선언문은 런타임 이전에 먼저 실행되기 때문.

<br>

# 메서드

클래스의 몸체에는 0개 이상의 메서드만 선언 가능함.

클래스의 몸체에서 정의할 수 있는 메서드 => constructor, 프로토타입 메서드, 정적 메서드

## constructor

`인스턴스를 생성하고 초기화하기 위한 특수한 메서드. (이름 변경 불가)`

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

```javascript
// 클래스는 함수다.
console.log(typeof Person); // function
console.dir(Person);
```

- 클래스는 평가되어 함수 객체가 됨.
- 모든 함수 객체가 갖고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리킨다.

```javascript
class Person {
  constructor(name, address) {
    // 인수로 인스턴스 초기화
    this.name = name;
    this.address = address;
  }
}

// 인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
const me = new Person("Lee", "Seoul");
console.log(me); // Person {name: "Lee", address: "Seoul"}
```

- 인스턴스를 초기화하려면 constructor를 생략해서는 안된다.
- this가 아닌 다른 객체를 명시적으로 반환하는 것은 클래스의 기본 동작을 훼손하는 것 => `return 문 생략`

## 프로토타입 메서드

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

- 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.

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

- 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.
- 결국 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수라고 볼 수 있다.

<br>

## 정적 메서드

`인스턴스를 생성하지 않아도 호출할 수 있는 메서드`

```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log("Hi!");
  }
}
```

- 클래스에서는 `static` 키워드를 붙이면 정적 메서드가 된다.
- 정적 메서드는 클래스로 호출한다. (인스턴스로 호출 불가 => 프로토타입 체인에 존재하지 않음)

<br>

# 프로퍼티

## 인스턴스 프로퍼티

인스턴스 프로퍼티는 constructor 내부에서 정의함.

```javascript
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name;
  }
}

const me = new Person("Lee");
console.log(me); // Person {name: "Lee"}
```

- constructor 코드가 실행되기 전에 constructor 내부의 this에는 이미 클래스가 암묵적으로 생성한 빈 객체가 바인딩되어 있음.
- 인스턴스에 프로퍼티가 추가되어 인스턴스가 초기화 됨.

<br>

```javascript
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name; // name 프로퍼티는 public하다.
  }
}

const me = new Person("Lee");

// name은 public하다.
console;
```

- 클래스는 접근 제한자를 지원하지 않음 => `인스턴스 프로퍼티는 public 함`

<br>

## 접근자 프로퍼티

접근자 프로퍼티는 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수인 `getter 함수`와 `setter 함수`로 구성되어 있다.

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
    [this.firstName, this.lastName] = name.split(" ");
  }
}

const me = new Person("Ungmo", "Lee");

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(`${me.firstName} ${me.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
me.fullName = "Heegun Lee";
console.log(me); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(me.fullName); // Heegun Lee

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(Person.prototype, "fullName"));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

- `getter는 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용하므로 반드시 무언가를 반환해야 함.`
- `setter는 프로퍼티에 값을 할당할 때마다 값을 조작할 때 사용하므로 반드시 매개변수가 있어야 한다.` 단 하나의 값만 할당받기 때문에 하나의 매개변수만 선언 가능.

<br>

## 클래스 필드 정의 제안

- **클래스 필드** : 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티.

```javascript
class Person {
  // this에 클래스 필드를 바인딩해서는 안된다.
  this.name = ''; // SyntaxError: Unexpected token '.'
}
```

- 클래스의 몸체에서 클래스 필드를 정의하려면 this에 바인딩하면 안된다. this는 클래스의 constructor와 메서드 내에서만 유효함.
- 클래스 필드를 초기화 해야한다면 constructor 에서 초기화해야 한다.

<br>

아래 예시와 같이 클래스 필드에 초기값을 할당하지 않으면 undefined.

```javascript
class Person {
  // 클래스 필드를 초기화하지 않으면 undefined를 갖는다.
  name;
}

const me = new Person();
console.log(me); // Person {name: undefined}
```

<br>

# 상속에 의한 클래스 확장

## 클래스 상속과 생성자 함수 상속

> **상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것.**

<br>

## extand 키워드

- **서브 클래스** : 상속을 통해 확장된 클래스 ( 파생 클래스 or 자식 클래스)
- **수퍼 클래스** : 서브 클래스에게 상속된 클래스 ( 베이스 클래스 or 부모 클래스)

<br>

## super 키워드

함수처럼 호출할 수도 있고 this와 같이 식별자차럼 참조할 수 있는 키워드

- super를 호출하면 수퍼 클래스의 constructor(super-constructor)를 호출.
- super를 참조하면 수퍼 클래스의 메서드를 호출할 수 있음.

<br>

### super 호출 시 주의사항

1. 서브 클래스에서 constructor를 생략하지 않는 경우 서브 클래스의 constructor에서는 반드시 super를 호출해야 한다.

2. 서브 클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.

3. super는 반드시 서브 클래스의 constructor에서만 호출한다.

<br>

## 상속 클래스의 인스턴스 생성 과정

**1. 서브 클래스의 super 호출**

- 자바스크립트 엔진은 수퍼 클래스와 서브 클래스 구분을 위해 내부 슬롯 [[ConstructorKind]]를 갖는다.
- 상속받지 않는 클래스는 **base**로, 상속받는 서브 클래스는 **derived**로 설정된다.

<br>

> 서브 클래스의 constructor에서 반드시 super를 호출해야 하는 이유
> <br> => 서브 클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼 클래스에게 인스턴스 생성을 위임하기 때문에

<br>

**2. 수퍼 클래스의 인스턴스 생성과 this 바인딩**

- new 연산자와 함께 호출된 클래스는 서브 클래스이다.
- 즉, 인스턴스는 new 연산자와 함께 호출된 함수가 가리키는 서브 클래스가 생성한 것으로 처리된다.

<br>

**3. 수퍼 클래스의 인스턴스 초기화**

- 수퍼 클래스의 constructor가 실행되어 this에 바인딩되어 있는 인스턴스 프로퍼티를 초기화한다.

<br>

**4. 서브 클래스 constructor로의 복귀와 this 바인딩**

- super 호출이 종료되고 서브 클래스 constructor로 흐름이 돌아온다. 이때 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다.
- 즉, super 가 호출되지 않으면 this 바인딩이 불가능하다.
  <br>

**5. 서브 클래스의 인스턴스 초기화**

- super 호출 이후 서브 클래스의 constructor의 인스턴스가 초기화된다.

<br>

**6. 인스턴스 반환**

- 모든 처리가 끝나면 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

# 📗배운 점

# 클래스와 생성자 함수의 정의 방식 비교

![image](https://github.com/chowonn/js-deepdive-study/assets/70478015/7fe42eaf-9aaa-4096-9dd2-140afb7af4ce)

<br>

# constructor 특징

1. constructor 는 클래스 내에 최대 한 개만 존재할 수 있다. 2개 이상이면 문법 에러 발생.

```javascript
class Person {
  constructor() {}
  constructor() {}
}
// SyntaxError: A class may only have one constructor
```

2. constructor 는 생략할 수 있다. 생략하면 빈 constructor가 암묵적으로 정의된다.

```javascript
class Person {
  // constructor를 생략하면 다음과 같이 빈 constructor가 암묵적으로 정의된다.
  constructor() {}
}

// 빈 객체가 생성된다.
const me = new Person();
console.log(me); // Person {}
```

<br>

# 정적 메서드와 프로토타입 메서드의 차이

1. 자신이 속해있는 프로토타입 체인이 다르다.

2. 정적 메서드는 클래스로 호출하고, 프로토타입 메서드는 인스턴스로 호출한다.

3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

<br>

# 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용함.

2. 클래스에서 메서드를 정의할 때는 콤마가 필요하지 않음.

3. 암묵적으로 strict mode로 실행.

4. for … in 또는 Object.keys 로 열거 불가능.

5. 내부 메서드 [[Contructor]]를 갖지 않는 non-constructor 이므로 new 연산자와 함께 호출 불가능.

<br>

# public / private

| 접근 가능성                 | public | private |
| --------------------------- | ------ | ------- |
| 클래스 내부                 | O      | O       |
| 자식 클래스 내부            | O      | X       |
| 클래스 인스턴스를 통한 접근 | O      | X       |

- private에 접근하려면 접근자 프로퍼티를 통해서 간접적으로 접근해야함.
- private 필드는 반드시 클래스 몸체에 정의해야 한다. ( construcor X )
  <br>

# 🤔궁금한 점
