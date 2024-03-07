# 목차

[📗배운 점](#📗배운-점)

[📌중요한 점](#📌중요한-점)

[🤔궁금한 점](#🤔궁금한-점)



## 📗배운 점

### 19.1 객제치향 프로그래밍

자바스크립트를 이루고 있는 **거의 모든 것**이 객체이다. (원시 타입의 값 제외)

*속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체라 한다.*

> 자바스크립트는 실세계의 실체(사물이나 개념)를 인식해 실체가 갖는 속성(attribute/property)들 중 필요한 속성(특징 등)을 추려 **추상화**하고 독립적인 객체의 집합으로 프로그램을 표현한다.

**객체지향 프로그래밍** : `상태`를 나타내는 데이터(property)와 상태 데이터를 조작할 수 있는 `동작(method)`을 하나의 논리적인 단위로 묶어 생각한다. 

### 19.2 상속과 프로토타입

! 속성이란, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.


**불필요한 중복 제거** : 프로토타입 기반으로 상속을 구현해 기존의 코드를 적극 재사용해 불필요한 중복을 제거한다. 

`생성자함수(17.2절)`는 동일 프로퍼티 (메서드 포함) 구조를 갖는 객체를 여러 개 생성시 유용
동일 생성자 함수에 의해 생성된 모든 인스턴스가 동일한 메서드를 중복 소유하는 것은 10개의 인스턴스 생성시 내용이 동일한 메서드도 10개 생성 ! -**메모리 낭비**


### 19.3 프로토타입 객체

**프로토타입 객체**: 객체간 상속을 구현. 프로토타입은 어떤 객체의 **상위 부모 객체**의 역할을 하는 객체 ! 상속을 받은 객체는 상위 객체로부터 공유 프로퍼티(메서드 포함)을 제공 받아 자기 것처럼 자유롭게 사용한다.

모든 객체는 하나의 프로토타입을 갖는다. 모든 프로토타입은 생성자 함수와 연결되어 있다.

모든 객체는 __proto__접근자 프로퍼티를 통해 자신의 [[prototype]] 내부 슬롯이 가리키는 트로토타입에 간접적으로 접근 가능.

프로토타입 - 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근

생성자 함수 - 자신의 prototype 프로퍼티를 통해 프로토타입에 접근

#### 19.3.1 __proto__ 접근자 프로퍼티  


> 모든 객체는 `__proto__`접근자 프로퍼티를 통해 자신의 [[prototype]] 내부 슬롯이 가리키는 트로토타입에 간접적으로 내부 슬롯의 값. 즉, 프로퍼티에 접근 가능.(직접적 아님 주의)
> getter/sette 접근자 방식([get] [set] 프로퍼티 어트리뷰트에 할당된 함수를 이용하는 방식)
> `__proto__ `접근자 프로퍼티를 통해 접근시 내부적으로 [get] getter함수가 호출
> `__proto__`접근자 프로퍼티를 통해 프로토타입을 할당하면 `__proto__`접근자 프로퍼티의 [set] setter함수 호출



```javascript
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent; //원본은 파괴되지 않음 참조만 한거

console.log(obj.x); // 1
```

`__proto__` 접근자를 통해 프로퍼티에 접근하는 이유가 뭐냐 ?
프로토타입은 무조건 [[단반향 리스트]]로 구현되어야만 한다 !
트로토타입은 계층 구조인 체인으로 묶여 있고 접근시 프로퍼티가 없을 시 `__proto__`가 가리키는 참조를 띠리 자신의 부모 역할을 쫓아 순차 검색함
이때 서로를 겨냥해 참조하는 순환 참조 체인으로 만들면 종점이 없어 무한 루프에 빠진다.
아무런 체크없이 무조건적으로 프로토타입을 교체할 수 없도록 `__proto__` 접근자 프로퍼티를 통해 접근하고 교체하도록 구현하였다.


```javascript
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefined

// 따라서 __proto__보다 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```
**직접상속**을 통해 Object.prototype을 상속받지 않는  객체를 생성하게되면 `__proto__`접근자 프로퍼티를 사용하지 못할 수 있음.
`__proto__`접근자 프로퍼티 대신에참조취득을 원한다면 **Object.getPrototypeOf** 메서드를 사용하고 프로토타입을 교체하고 싶다면**Object.setPrototypeOf** 메서드를 사용한다. (권장)

#### 19.3.2 함수 객체의 prototype 프로퍼티

**함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**

즉, 생성자 함수로서 호출이 불가한 non-constructor인 화살표 함수와 메서드 축약 표현 메서드는 prototype 프로퍼티는 소유하지 않고 프로토타입도 생성하지 않음. (일반 함수형태는 가능)

> 모든 객체가 가지고 있는 Object.prototype으로부터 *상속*받은 `__proto__` 접근자 프로퍼티와 함수 객체만이 소유하는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.(주체는 다름 p.270)



#### 19.3.3 프로토타입의 CONSTRUCTOR 프로퍼티와 생성자 함수

모든 프로토타입 constructor를 가진다. 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다 
> constructor 프로퍼티는 생성자 함수를 가리킨다. 함수 객체가 생성될때 연결이 이뤄짐.

```javascript
// 생성자 함수
function Person(name) { // 생성자함수
  this.name = name;
}

const me = new Person("Lee"); // me 객체 생성 이때 me 객체가 constructor랑 연결되버림
//그래서 me 객체에는 constructor 프로퍼티가 없는데도 상속받아서 있는거마냥 true가 되버림.

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true 
//일반함수거 빌려쓰는 처지라서 결국 얘네는 같음. 
//주체는 다르나 동일한 프로토타입을 가리킨다. ! 
```



### 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

> 생성자함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결 ! 이때, constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수이다.
> 결국, constructor 프로퍼티는 생성자 함수를 가리킨다.

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

But, 리터럴 표기법에 의한 객체 생성 방식처럼 명시적으로 new 연산자없이 객체 생성하는 경우도 있다. ↓

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
리터럴 객체도 물론 프로토타입이 존재함. 하지만 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없음.

생성자로 빈 객체(undfined , null) 생성시 내부에 암시적으로 추상 연산인 ordinaryObjectCreate를 호출해서 빈객체로 생성해줘서 빈 객체도 Object.prototype을 가져버린다.

즉, 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체는 아님.

> 리터럴 객체도 결국 상속을 위해서 프로토타입이 필요함. 그래서 가상의 생성자 함수를 같는다. 다시 말해서, 프로토타입과 생성자함수는 단독으로 존재할 수 없고 언제나 짝꿍임.




### 19.5 프로토타입의 생성 시점

> 모든 객체는 생성자함수와 연결되어 있다. 생성자 함수 생성 시점에 프로토타입이 생성되는 데 이는 둘이 짝꿍인걸 증명한다.

#### 19.5.1 사용자 정의 함수와 프로토타입 생성 시점

1. 생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

2. 함수 선언문은 호이스팅에 의해 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행되니까 결국 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 된다. 이때 프로토타입도 더불어 생성됨.

3. 프로토타입 = 객체, 객체는 프로토타입을 가지므로 프로토타입도 자신의 프로토타입을 가짐. 생성된 프로토타입의 프로토타입은 Object.prototype이다.  

4. 이처럼 빌트인이 아니라 사용자 정의 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 생성함. 그리고 이는 언제나 Object.prototype이다.

#### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

> 빌트인도 일반 함수랑 마찬가지로 생성자 함수가 생성되는 시점에 프로토타입이 생성된다. 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다. 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다. 

**전역 객체** : 코드 실행 이전에 자바스크립트 엔진에 의해 생성되는 특수한 객체. 브라우저에서는 window, 서버 사이드 환경에서는 global 객체를 의미

> 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다. 이후 생성된 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [prototype] 내부 슬롯에 할당된다. 이로써 생성된 객체는 프로토타입을 상속받는다. 

### 19.6 객체 생성 방식과 프로토타입의 결정

객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점을 가짐.

객체 생성 시점에 객체 생성 방식에 따라 결정되는 인수를 추상연산 OrdinaryObjectCreate이 전달 받고 그리고 이를 자신이 생성할 객체에 추가할 프로퍼티 목록을 옵션으로 전달할 수 있다.


객체의 다양한 생성 방식

1. 객체 리터럴
2. Object 생성자 함수
3. 생성자 함수
4. Object.create 메서드
5. 클래스


#### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입


자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할때 추상 연산 OrdinaryObjectCreate를 호출해서 전달되는 프로토타입이 Object.prototype이다. 
> 즉, 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다. 

객체 리터럴에 의해 생성된 객체는 Object.prototype를 프로토타입으로 갖게 되며, 이로써 Object.prototype를 상속받는다. 자기 자신의 것을 갖는게 아니라 상속받은 Object.prototype의 constructor랑 hasOwnProperty 메서드를 자기 것 마냥 쓸 수 있음. 근데 본인거 아님

객체 리터럴에 의해 생성된 객체와 Object 생성자 함수에 의해 생성된 객체는 동일한 프로토타입을 가진다.
#### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

생성자 함수를 인수없이 호출 시 빈 객체가 생성되어 호출시 추상 연산  OrdinaryObjectCreate가 호출됨. 이때 추상 연산에 전달되는 프로토타입은 Object.Prototype임. 

즉, 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype임. 빈 객체로 생성자 함수 생성시 추상연산에 의해 생성자 함수와 Object.prototype과 생성된 객체 사이에 연결이 만들어짐. 객체리터럴에 의해 생성된 객체와 동일한 구조를 갖는다. 이처럼 생성자 함수에 의해 생성된 객체는 Object.prototype을 프로토타입으로 갖게 되며, 이로써 Object.prototype을 상속받는다.

객체리터럴 방식은 객체 리터럴 내부에 프로퍼티를 추가.
생성자 함수 방식은 빈객체 생성 후 프로퍼티 추가.



#### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

new로 생성자 함수를 호출해서 인스턴스 생성시 다른 객체 생성 방식과 마찬가지로 추상 연상 OrdinaryObjectCreate가 호출됨. 

이때 OrdinaryObjectCreate에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체임. 즉, 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인되어 있는 객체이다. 

생성시 추상연산에 의해 생성자 함수와 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체와 생성된 객체 사이에 연결이 만들어짐. Object.prototype은 다양한 빌트인 메서드를 갖고 있지만 사용자 정의 함수로 생성된 프로토타입은 constructor 뿐이다. 


### 19.7 프로토타입 체인

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
자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 내부슬롯의 참조를 따라 자신의 부모역할을 하는 프로토타입의 프로퍼티를 순차 검색한다. 이를 프로토타입 체인이라 한다.

> 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍에 상속을 구현하는 메커니즘이다.

찾고 찾고 찾아서 상위를 찾아낸다.

체인의 최상위 객체는 언제나 Object.prototype이다. 따라서 모든 객체는  Object.prototype를 상속받는다.  Object.prototype가 체인의 종점임.  Object.prototype 내부 슬롯은 null임.
근데 종점인  Object.prototype에서도 프로퍼티를 못 찾으면 undefined 반환.

자바스크립트 엔진은 객체 간 상속 관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티를 검색한다. 즉, 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘이라고 할 수 있다.

프로퍼티가 아닌 것은 스코프 체인에서 검색한다. 스코프 체인은 식별자 검색을 위한 메커니즘임.

> 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아닌 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용된다. 

### 19.8 오버라이딩과 프로퍼티 섀도잉

프로토토입 프로퍼티와 `같은` 이름으로 인스턴스에 추가하면 체인을 따라 프로토타입 프로퍼티를 검색해서 덮어쓰는 것이 아닌 인스턴스에 `추가`해준다. 인스턴스 메서드는 같은 이름의 프로토타입 메서드를 오버라이딩한 것. 그러면 프로토타입 메서드는 가려진다. 이를 섀도잉이라 한다.

**오버라이딩** : (override  - 덮어 쓰다.) 상위 클래스가 가지고 있는 매서드를 하위 클래스가 재정의 하여 사용하는 방식이다. 

**오버 로딩** : 함수 이름은 동일하나 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식. 자바스크립트는 오버로딩을 지원하지 않지만 arguments 객체를 사용하며 구현 가능.

**섀도잉**: 이로써 상의 클래스가 가려지는 현상을 섀도잉이라 한다.

삭제하는 경우에도 인스턴스 메서드 삭제시 프로토타입의 프로퍼티를 삭제 또는 변경할 수 없다.
원본을 변경한게 아니니까 다시 말해 get은 되는데 set은 안됨.

무조건 직접 접근 해야함.

### 19.9 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경 할 수 있다. 이것은 부모 객체인 프로토타입을 동적으로 변경 할 수 있다는 것을 의미함. 프로토타입은 생성자 함수 또는 인스턴스에 의해 교체가 가능함.

#### 19.9.1 생성자 함수에 의한 교체

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // ① 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = { //객체 리터럴 할당. person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체한 것.
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");
```

프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없다. constructor 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할때 암묵적으로 추가한 것. 따라서 me 객체의 생성자 함수를 검색하면 person이 아닌 Object가 나온다.

프로토타입 교체시 constructor 프로퍼티와 생성자 함수 간 연결이 파괴된다.


```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체 즉, 함수 연결 파괴
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정 즉, constructor 프로퍼티가 다시 살아남.
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

#### 19.9.2 인스턴스에 의한 프로토타입의 교체

프로포타입 접근 방법 두가지
1. 생성자 함수의 prototype 프로퍼티
2. 인스턴스의 `__proto__`접근자 프로퍼티(Object.getPrototypeOf)

프로포타입 교체 방법 두가지
1. 생성자 함수의 prototype 프로퍼티
2. 인스턴스의 `__proto__`접근자 프로퍼티(Object.setPrototypeOf)

차이
- 생성자 함수의 prototype 프로퍼티에 다른 임의의 객체를 바인딩하는 것은 - 미래에 생성할 인스턴스의 프로토타입을 교체하는 것.

- `__proto__` 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 - 이미 생성된 객체의 프로토타입을 교체하는 것

> `__proto__` 프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없음. 즉, constructor 프로퍼티와 함수 간 연결이 파괴된다.

> 교체한 객체 리터럴에 constructor 프로퍼티를 추가하고 생성자 함수의 prototype 프로퍼티를 재성정하여 파괴된 생성자 함수와 프로토타입 간의 연결을 되살린다.


### 19.10 instanceof 연산자

`객체 instanceof 생성자 함수`

> 이항 연상자로서 왼쪽 객체를 가르키는 식별자로, 오른쪽 생성자 함수를 가리키는 식별자를 피연자로 받는다. (오른쪽이 함수가 아니면 TypeError)

오른쪽 생성자 함수의 prototype에 바인딩된 객체가 좌측 객체의 프로토타입 체인 상에 존재하면 true, 아니면 false.



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

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입의 교체
Object.setPrototypeOf(me, parent); // 교체되면서 me랑 연결 파괴
//하지만 여전히 person 생성자함수에 의해 생성된 인스턴스임

// Person 생성자 함수와 parent 객체는 연결되어 있지 않다.
console.log(Person.prototype === parent); // false
console.log(parent.constructor === Person); // false

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않기 때문에 false로 평가된다.
console.log(me instanceof Person); // false  // person이랑은 이별했지만

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true // 아직 객체랑은 아님
```

instanceof는 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

생성자 함수에 의해 프로토타입이 교체되어 constructor 프로퍼티와 생성자 함수 간 연결이 파괴되어도 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로 instanceof는 아무런 영향도 받지 않는다. 

### 19.11 직접상속

#### 19.11.1 Object.create에 의 한 직접 상속

> Object.create메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다. 다른 객체 생성 방식처럼 추상연산OrdinaryObjectCreate를 호출하여 객체를 생성한다.
> 첫번째 매개변수, 생성할 객체의 프로토타입으로 지정할 객체
> 두번째 매개변수, 생성할 객체의 프로퍼티 키, 디스크립터 객체로 이뤄진 객체 (option 생략가능)


Object.create 객체 생성시 이점

1. new연산자 없이도 객체 생성
2. 프로토타입을 지정하면서 객체 생성
3. 객체 리터럴에 의해 생성된 객체도 상속 가능


Object.prototype의 빌트인 메서드를 객체가 직접 호출하는 것은 Object.create메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있어서 간접적으로 사용할 것.

직접호출
```javascript
//직접호출
// 프로토타입이 null인 객체, 즉 프로토타입 체인의 종점에 위치하는 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

console.log(Object.getPrototypeOf(obj) === null); // true

// obj는 Object.prototype의 빌트인 메서드를 사용할 수 없다.
console.log(obj.hasOwnProperty('a')); // TypeError: obj.hasOwnProperty is not a function
```
간접호출 
```javascript
// 프로토타입이 null인 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

// console.log(obj.hasOwnProperty('a')); // TypeError: obj.hasOwnProperty is not a function

// Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않는다.
console.log(Object.prototype.hasOwnProperty.call(obj, 'a')); // true
```

#### 19.11.2 객체 리터럴 내부에서 `__proto__`에 의한 직접 상속

객체 리터럴 내부에서 `__proto__`접근자 프로퍼티를 사용하여 직접 상속하는 방법

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

### 19.12 정적 프로퍼티 메서드

> 정적프로퍼티/메서드 생성자 함수로 인스턴스를 생성하지 않아도 참조 또는 호출 할 수 있는 프로퍼티 메서드를 말한다.
> 일반적으로 생성자 함수가 생성한 인스턴스는 자신의 프로토타입 체인에 속한 객체의 프로퍼티/메서드에 접근 가능 
> but, 정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 인스턴스로 접근 불가 

1. Object.create 메서드는 Object 생성자 함수의 정적 메서드 Object.prototype.hasOwnProperty 메서드는 Object.prototype의 메서드다. 따라서 Object.create 메서드는 인스턴스, 즉 Object 생성자 함수가 생성한 객체로 호출 불가
2. Object.prototype.hasOwnProperty 메서드는 모든 객체의 프로토타입 종점, 즉 Object.prototype의 메서드로 모든 객체가 호출 가능하다.

### 19.13 프로퍼티 존재 확인

#### 19.13.1 in 연산자

객체 내 특정 프로퍼티 존재 여부를 확인 (거기 있니 ~?)

```
// key: 프로퍼티 키를 나타내는 문자열
// object: 객체로 평가되는 표현식
key in object
```

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
in 연산자는 확인 대상 객체의 프로퍼티뿐만 아니라 상속받는 모든 프로토타입의 프로퍼티를 확인하므로 주의가 필요하다. 

person 객체에는 toString이 없지만 

밑에 처럼 true를 보냄

이는 in 연산자가 person 객체가 속한 프로토타입 체인 상에 존재하는 모든 프로토타입에서 toString을 검색해서임. 
toString은 Object.prototype의 메서드

```
console.log("toString" in person); // true

```

Reflect.has 메서드는 in 연산자와 동일하게 작동한다.

```javascript
const person = { name: "Lee" };

console.log(Reflect.has(person, "name")); // true
console.log(Reflect.has(person, "toString")); // true
```

#### 19.13.2 Object.prototype.hasOwnProperty 메서드

Object.prototype.hasOwnProperty 객체에 특정 프로퍼티가 존재 여부를 확인한다.

```javascript
console.log(person.hasOwnProperty("name")); // true
//전달받은 key가 객체 고유의 프로퍼티 키여야만 true 상속받은 프로토타입의 프로퍼티 키인 경우 false
console.log(person.hasOwnProperty("age")); // false
```

### 19.14 프로퍼티 열거

#### for…in 문

`for (변수선언문 in 객체){...}`

객체의 모든 프로퍼티를 순회-열거하려면 for...in문을 사용한다.

객체의 프로퍼티 수만큼 순회함. 

```javascript
const person = {
  name: 'Lee',
  address: 'Seoul'
};

// in 연산자는 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
console.log('toString' in person); // true

// for...in 문도 객체가 상속받은 모든 프로토타입의 프로퍼티를 열거한다.
// 하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.
for (const key in person) {
  console.log(key + ': ' + person[key]);
}

// name: Lee
// address: Seoul
```


1. 프로퍼티 어트리뷰트 [Enumerable]은 프로퍼티의 열거 가능 여부를 true false로 나타냄

2. for...in문은 객체의 프로토타입 체인 상에 존재하는 모든 프로퍼티 중에서 프로퍼티 어트리뷰트[Enumerable]의 값이 true인 프로퍼티를 순회하며 열거한다. (false, 심볼 열거 안함)
3. 프로퍼티를 열거할때 순서를 보장하지 않는다.

#### 19.14.2 Object.keys/values/entries 메서드

for...in문은 객체 자신의 고유 프로퍼티 뿐만 아니라 상속받은 프로퍼티도 열거한다.

Object.prototype.hasOwnProperty 메서드를 사용해 객체 자신의 프로퍼티인지 확인해
객체 자신의 고유 프로퍼티만을 열거하기 위해 for...in문 보다 Object.keys, Object.valeus, Object.entries 메서드 사용을 권장한다.

1. Object.keys
     객체 자신의 열거가능한 프로퍼티 키를 배열로 반환한다.
2. Object.values
     객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다.
3. Object.entries
     객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환한다.

## 📌중요한 점

## 🤔궁금한 점
