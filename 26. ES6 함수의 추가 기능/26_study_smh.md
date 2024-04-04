# 목차

[📗배운 점](#📗배운-점)

[📌중요한 점](#📌중요한-점)

[🤔궁금한 점](#🤔궁금한-점)

## 📗배운 점

## 26.1 함수의 정의 
ES6 이전 함수의 역할 - 일반 함수, 생성자 함수, 메서드
문제점 - callable 하면서 constructor의 역할을 해 편리하나 성능 면에서 문제

> [[callable과 constructor/ non-constructor ]]
> 17.2.4절 "내부 메서드 [[Call]]과 [[Construct]]"에서 살펴보았듯이 호출할 수 있는 함수 객체를 callable이라 하며, 인스턴스 를 생성할 수 있는 함수 객체를 constructor, 인스턴스를 생성할 수 없는 함수 객체를 non-constructor라고 부른다

*ES6에서의 함수*

|ES6 함수의 구분|constructor|prototype|super|arguments|
|:---:|:---:|:---:|:---:|:---:|
|일반함수(Normal)|O|O|X|O|
|메서드(Method)|X|X|O|O|
|화살표함수(Arrow)|X|X|X|X|

**일반함수:** 함수 선언문 또는 함수 표현식으로 정의한 함수를 말한다. 

ES6에서 메서드와 화살표 함수는 non-constructor(인스턴스 생성 불가한 함수 객체)이다.

## 26.2 메서드

일반적 의미 - 객체에 바인딩된 함수

ES6에서의 의미 - 메서드 축약표현으로 정의된 함수만을 의미

```javascript
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() { return this.x; },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function() { return this.x; }
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```


**ES6**사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor임 즉, `생성자 함수로 호출 불가`


```javascript
new obj.foo(); // -> TypeError: obj.foo is not a constructor
new obj.bar(); // -> bar {}
```

ES6 메서드는 인스턴스를 생성항수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않음.

```javascript
// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty('prototype'); // -> false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty('prototype'); // -> true
```

표준빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor이다.


```javascript
String.prototype.toUpperCase.prototype; // -> undefined
String.fromCharCode.prototype           // -> undefined

Number.prototype.toFixed.prototype; // -> undefined
Number.isFinite.prototype;          // -> undefined

Array.prototype.map.prototype; // -> undefined
Array.from.prototype;          // -> undefined
```

ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.

super 참조는 내부슬롯 [[HomeObject]]를 사용하여 수퍼클래스의 메서드를 참조하므로 내부 슬롯 [[HomeObject]]를 갖는 ES6 메서드는 super 키워드를 사용할 수 있다.

```javascript
const base = {
  name: 'Lee',
  sayHi() {
    return `Hi! ${this.name}`;
  }
};

const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드다. ES6 메서드는 [[HomeObject]]를 갖는다.
  // sayHi의 [[HomeObject]]는 sayHi가 바인딩된 객체인 derived를 가리키고
  // super는 sayHi의 [[HomeObject]]의 프로토타입인 base를 가리킨다.
  sayHi() {
    return `${super.sayHi()}. how are you doing?`;
  }
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```


only ES6메서드인 함수만 super 키워드 사용 가능 ES6 메서드가 아닌 함수는 내부 슬롯 [[HomeObject]] 없음.


```javascript
const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드가 아니다.
  // 따라서 sayHi는 [[HomeObject]]를 갖지 않으므로 super 키워드를 사용할 수 없다.
  sayHi: function () {
    // SyntaxError: 'super' keyword unexpected here
    return `${super.sayHi()}. how are you doing?`;
  }
};
```

## 26.3 화살표 함수

화살표 함수는 function 키워드 대신 화살표 ( => )를 사용 


### 26.3.1 화살표 함수 정의

#### 함수 정의

함수 선언문 x
함수 표현식 o

```javascript
const multiply = (x, y) => x * y;
multiply(2, 3); // -> 6
```

#### 매개변수 선언

매개변수가 여러개면 '()' 안에 매개변수 선언하기 

```javascript
const arrow = (x, y) => { ... };
```

매개변수 한개면 소괄호 생략 가능

```javascript
const arrow = x => { ... };
```

매개변수가 하나도 없으면 () 소괄호라도 써야한다.


```javascript
const arrow = () => { ... };
```

#### 함수 몸체 정의

몸체가 하나의 `문`이라면 함수 몸체를 감싸는 { } 생략 가능

내부 문이 값으로 평가되는 표현식인 문이라면 암묵적으로 반환됨.

```javascript
// concise body
const power = x => x ** 2;
power(2); // -> 4

// 위 표현은 다음과 동일하다.
// block body
const power = x => { return x ** 2; };
```

{} 중괄호 생략시 몸체 내부의 문이 표현식(값으로 평가된다.)이 아닌 문이라면 에러가 발생함 값으로 평가되지 않아 반환할 수 없으니까

```javascript
const arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'

// 위 표현은 다음과 같이 해석된다.
const arrow = () => { return const x = 1; };
```

따라서, 중괄호 생략불가

```javascript
const arrow = () => { const x = 1; };
```


객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸 주기

```javascript
const create = (id, content) => ({ id, content });
create(1, 'JavaScript'); // -> {id: 1, content: "JavaScript"}

// 위 표현은 다음과 동일하다.
const create = (id, content) => { return { id, content }; };
```

객체 리터럴을 () 감싸지 않으면 객체 리터럴의 중괄호 {}를 함수 몸체를 감싸는 중괄호로 인식함.


```javascript
// { id, content }를 함수 몸체 내의 쉼표 연산자문으로 해석한다.
const create = (id, content) => { id, content };
create(1, 'JavaScript'); // -> undefined
```

함수 몸체가 여러개의 문이라면 ? { } 중괄호 생략 불가.
반환값또한 명시적으로 반환해야 한다. (return 써서)

```javascript
const sum = (a, b) => {
  const result = a + b;
  return result;
};
```

화살표 함수로 즉시 실행 함수 IIFE 사용 가능

```javascript
const person = (name => ({
  sayHi() { return `Hi? My name is ${name}.`; }
}))('Lee');

console.log(person.sayHi()); // Hi? My name is Lee.
```

화살표 함수도 일급 객체이므로 고차함수에 인수로 전달 가능 

```javascript
// ES5
[1, 2, 3].map(function (v) {
  return v * 2;
});

// ES6
[1, 2, 3].map(v => v * 2); // -> [ 2, 4, 6 ]
```

화살표 함수의 콜백으로서의 유용함.


### 26.3.2 화살표 함수와 일반 함수의 차이

01. 화살표 함수는 인스턴스 생성 불가한 non-constructor
02. 중복된 매개변수 이름을 선언할 수 없다.
03. 화살표 함수는 함수 자체의 this,arguments,super, new.target 바인딩을 갖지 않는다. `따라서 화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조한다. 만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this, arguments, super, new.target 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this, arguments, super, new.target을 참조한다`   

### 26.3.3 this

화살표 함수는 다른 함수의 인수로 전달되어 콜백으로 사용되는 경우가 많음.
콜백 함수 내부의 this가 외부 함수의 this와 다르기 때문에 발생하는 문제를 해결하기 위해 화살표 함수의 this를 일반 함수의 this와 다르게 동작하게 만들었다. this 바인딩은 함수의 호출 방식에 따라 동적으로 결정된다. 정의했을때가 아니라 호툴했을때에 따라 this에 바인딩할 객체가 동적 결정되는 것이다.

주의할 것은 일반 함수로서 호출되는 콜백 함수의 경우다. 고차 함수 Higher-Order Function, HOF의 인수로 전달 되어 고차 함수 내부에서 호출되는 콜백 함수도 중첩 함수라고 할 수 있다. 

```javascript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    // add 메서드는 인수로 전달된 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가한다.
    // ①
    return arr.map(function (item) {
      return this.prefix + item; // ②
      // -> TypeError: Cannot read property 'prefix' of undefined
    });
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```

기대결과 : ['-webkit-transition','-webkit-user-select', TypeError]

> 자바스크립트에서 this 키워드는 함수가 호출되는 방식에 따라 그 의미가 달라진다. 클래스의 메서드 내부에서 this는 해당 메서드를 호출한 객체를 가리키는데, 예제에서는 Prefixer 클래스의 인스턴스를 의미합니다. 하지만, 배열의 map 메서드 같은 고차 함수 안에서 사용되는 콜백 함수의 경우, this의 값이 기본적으로 전역 객체(브라우저에서는 window, Node.js에서는 global)를 가리키거나 undefined가 됩니다(엄격 모드에서는 undefined).

이 문제를 해결하는 방법은 여러 가지가 있습니다. 하나의 방법은 콜백 함수를 화살표 함수로 변경하는 것입니다. 화살표 함수는 자신을 둘러싼 외부 스코프의 this를 "상속" 받으므로, Prefixer 클래스의 add 메서드 내부에서 화살표 함수를 사용하면, this가 Prefixer 인스턴스를 정확하게 가리키게 됩니다.

해결 방법: 
```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map((item) => {
      return this.prefix + item;
    });
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```

이렇게 하면, map 메서드 내의 화살표 함수는 add 메서드가 호출된 같은 this 컨텍스트(여기서는 prefixer 객체)를 사용하게 되므로, this.prefix를 올바르게 참조할 수 있게 되니까 기대한 대로 ['-webkit-transition', '-webkit-user-select'] 결과를 얻을 수 있다.

프로토타입 메서드 내부인 ①에서 this는 메서드를 호출한 객체(위 예제의 경우 prefixer 객체)를 가리킨다. 그런데 Array.prototype.map의 인수로 전달한 콜백 함수의 내부인 ②에서 this는 undefined를 가리킨다. 이는 Array.prototype.map 메서드가 콜백 함수를 일반 함수로서 호출하기 때문이다. 

> Array.prototype.map 메서드는 배열을 순회하며 배열의 각 요소에 대하여 인수로 전달된 콜백 함수를 호출한다. 그리고 콜백 함수의 반한강들로 구성된 새로운 배열을 반환한다. 위 예제에서 map 메서드는 매개변수 arr에 전달 된 [transition', 'user-select']를 순회하며 콜백 함수의 item 매개변수에게 arr의 요소값을 전달하면서 콜백 함수를 arr의 요소 개수만큼 호출한다. 그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다. 이에 대해서는 27.9.3절 prototype.map"에서 자세히 살펴볼 것이다. 지금은 일반 함수로서 호출되는 "콜백 함수 내부의 this 문제'에 주목하자. 

클래스 쓰면 무조건 엄격모드여서 일반 함수를 호출하면 this는 undefined를 가리킨다. 콜백 함수에서도 같은 문제가 발생 ! 배열의 map 같은 메서드의 콜백 함수 내부에서 this를 사용하려고 하면, 기대했던 객체가 아닌 undefined가 되어버립니다. 이로 인해 TypeError가 발생하게 되는 것이다.


`콜백 함수 내부의 this 문제` 해결 방안

ES6 이전 방법

01. add 메서드를 호출한 prefixer 객체를 가리키는 this를 일단 회피시킨 후에 콜백 함수 내부에서 사용한다. 
02. Array.prototype.map의 두 번째 인수로 add 메서드를 호출한 prefixer 객체를 가리키는 this를 전달한다. ES5에서 도입된 Array.prototype.map은 “콜백 함수 내부의 this 문제"를 해결하기 위해 두 번째 인수로 콜백 함 수 내부에서 this로 사용할 객체를 전달할 수 있다. 
03. Function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩 한다.
  

ES6 이후 해결 방법

```javascript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map(item => this.prefix + item);
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
// ['-webkit-transition', '-webkit-user-select']
```
화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 lexical this라 한다. 이는 마치 레시컬 스코프와 같이 화살표 함수 의 this가 함수가 정의된 위치에 의해 결정된다는 것을 의미한다.

화살표 함수를 <U>*제외*</U>한 모든 함수에는 this 바인딩이 반드시 존재한다. 따라서 ES6에서 화살표 함수가 도입전에는 일반적인 식별자처럼 스코프 체인을 통해 this를 탐색할 필요가 없었다. 화살표 함수는 함수 자체의 this 바인딩이 존재하지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 일반적인 식 별자처럼 스코프 체인을 통해 상위 스코프에서 this를 탐색한다. 

```javascript
// 화살표 함수는 상위 스코프의 this를 참조한다.
() => this.x;

// 익명 함수에 상위 스코프의 this를 주입한다. 위 화살표 함수와 동일하게 동작한다.
(function () { return this.x; }).bind(this);
```

만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중에서 <u>화살표 함수가 아닌 함수의 this를 참조한다.</u>

```javascript
// 중첩 함수 foo의 상위 스코프는 즉시 실행 함수다.
// 따라서 화살표 함수 foo의 this는 상위 스코프인 즉시 실행 함수의 this를 가리킨다.
(function () {
  const foo = () => console.log(this);
  foo();
}).call({ a: 1 }); // { a: 1 }

// bar 함수는 화살표 함수를 반환한다.
// bar 함수가 반환한 화살표 함수의 상위 스코프는 화살표 함수 bar다.
// 하지만 화살표 함수는 함수 자체의 this 바인딩을 갖지 않으므로 bar 함수가 반환한
// 화살표 함수 내부에서 참조하는 this는 화살표 함수가 아닌 즉시 실행 함수의 this를 가리킨다.
(function () {
  const bar = () => () => console.log(this);
  bar()();
}).call({ a: 1 }); // { a: 1 }
```

만약 화살표 함수가 전역 함수라면 화살표 함수의 this는 전역 객체를 가리킨다. 전역 함수의 상위 스코프는 전역이고 전역에서 this는 전역 객체를 가리키기 때문이다. 

```javascript
// 전역 함수 foo의 상위 스코프는 전역이므로 화살표 함수 foo의 this는 전역 객체를 가리킨다.
const foo = () => console.log(this);
foo(); // window
```

프로퍼티에 할당한 화살표 함수도 스코프 체인 상에서 가장 가까운 상위 함수 중에서 <u>화살표 함수가 아닌 함수의 this를 참조한다.</u>

```javascript
// increase 프로퍼티에 할당한 화살표 함수의 상위 스코프는 전역이다.
// 따라서 increase 프로퍼티에 할당한 화살표 함수의 this는 전역 객체를 가리킨다.
const counter = {
  num: 1,
  increase: () => ++this.num
};

console.log(counter.increase()); // NaN
```

화살표 함수는 `함수 자체의 this 바인딩을 갖지 않기` 때문에 Function.prototype.call, apply, Function.prototype.bind 메서드를 사용해도 화살표 함수 내부의 `this를 교체할 수 없다`.


```javascript
window.x = 1;

const normal = function () { return this.x; };
const arrow = () => this.x;

console.log(normal.call({ x: 10 })); // 10
console.log(arrow.call({ x: 10 }));  // 1
```

화살표 함수가 Function.prototype.call, Function.prototype.apply, Function.prototype.bind 메서드를 호출할 수 없다는 의미는 아니다. `화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 this를 교체 할 수 없고 언제나 상위 스코프의 this 바인딩을 참조한다.`


```javascript
const add = (a, b) => a + b;

console.log(add.call(null, 1, 2));    // 3
console.log(add.apply(null, [1, 2])); // 3
console.log(add.bind(null, 1, 2)());  // 3
```
메서드를 화살표 함수로 표현은 할 수 있으나 비추천


```javascript
// Bad
const person = {
  name: 'Lee',
  sayHi: () => console.log(`Hi ${this.name}`)
};

// sayHi 프로퍼티에 할당된 화살표 함수 내부의 this는 상위 스코프인 전역의 this가 가리키는
// 전역 객체를 가리키므로 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는
// window.name과 같다. 전역 객체 window에는 빌트인 프로퍼티 name이 존재한다.
person.sayHi(); // Hi
```

sayHi 프로퍼티에 할당한 화살표 함수 내부의 this는 메서드를 호출한 객체인 person을 가리키지 않고 상위 스코프인 전역의 this가 가리키는 전역 객체를 가리킨다. 따라서 화살표 함수로 메서드를 정의하는 것은 바람직하지 않다. 

메서드를 정의할 때는 `ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용 하는 것이 좋다. `

```javascript
// Good
const person = {
  name: 'Lee',
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
};

person.sayHi(); // Hi Lee
```
 프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 동일한 문제가 발생한다.


```javascript
// Bad
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = () => console.log(`Hi ${this.name}`);

const person = new Person('Lee');
// 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는 window.name과 같다.
person.sayHi(); // Hi
```
프로퍼티 동적 추가시 ES6 메서드 정의는 사용할 수 없으므로 아래처럼 일반 함수를 사용한다.

```javascript
// Good
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function () { console.log(`Hi ${this.name}`); };

const person = new Person('Lee');
person.sayHi(); // Hi Lee
```

일반 함수가 아닌 ES6 메서드를 동적 추가하고 싶다면 다음과 같이 `객체 리터럴`을 바인딩하고 프로토타입의 constructor 프로퍼티와 생성자 함수 간의 연결을 재설정한다. 


```javascript
function Person(name) {
  this.name = name;
}

Person.prototype = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 재설정
  constructor: Person,
  sayHi() { console.log(`Hi ${this.name}`); }
};

const person = new Person('Lee');
person.sayHi(); // Hi Lee
```

 클래스 필드 정의 제안을 사용하여 클래스 필드에 화살표 함수를 할당할 수도 있다.

```javascript
// Bad
class Person {
  // 클래스 필드 정의 제안
  name = 'Lee';
  sayHi = () => console.log(`Hi ${this.name}`);
}

const person = new Person();
person.sayHi(); // Hi Lee
```

이때 sayhi 클래스 필드에 할당한 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this 바인딩을 참조 한다. 그렇다면 sayhi 클래스 필드에 할당한 화살표 함수의 상위 스코프는 무엇일까? sayhi 클래스 필드는 인스턴스 프로퍼티이므로 다음과 같은 의미다. 

```javascript
class Person {
  constructor() {
    this.name = 'Lee';
    // 클래스가 생성한 인스턴스(this)의 sayHi 프로퍼티에 화살표 함수를 할당한다.
    // sayHi 프로퍼티는 인스턴스 프로퍼티이다.
    this.sayHi = () => console.log(`Hi ${this.name}`);
  }
}
```

SayHi 클래스 필드에 할당한 화살표 함수의 상위 스코프는 사실 클래스 외부다. 하지만 this는 클래스 외부의 this를 참조하지 않고 클래스가 생성할 인스턴스를 참조한다. 따라서 sayHi 클래스 필드에 할당한 화살표 함수 내부에서 참조한 this는 constructor 내부의 this 바인딩과 같다. constructor 내부의 this 바인딩은 클래스가 생성한 인스턴스를 가리키므로 sayHi 클래스 필드에 할당한 화살표 함수 내부의 this 또한 클래 스가 생성한 인스턴스를 가리킨다. 하지만 클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아니라 인스턴스 메서드가 된다. 따라서 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

```javascript
// Good
class Person {
  // 클래스 필드 정의
  name = 'Lee';

  sayHi() { console.log(`Hi ${this.name}`); }
}
const person = new Person();
person.sayHi(); // Hi Lee
```



   

### 26.3.4 super

화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다. 

```javascript
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  // 화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킨다.
  sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

super는 내부 슬롯 [[HomeObject]]를 갖는 ES6 메서드 내에서만 사용할 수 있는 키워드다. sayti 클래스를 핗드에 할당한 화살표 함수는 ES6 메서드는 아니지만 함수 자체의 super 바인딩을 갖지 않으므로 super를 참조해도 에러가 발생하지 않고 this와 마찬가지로 클래스 필드에 할당한 화살표 함수 내부에서 super를 참조 하면 constructor 내부의 super 바인딩을 참조한다. 위 예제의 경우 Derived 클래스의 constructor는 생략 되었지만 암묵적으로 constructor가 생성된다. 



### 26.3.5 arguments

 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 arguments를 참 조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다.

```javascript
(function () {
  // 화살표 함수 foo의 arguments는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
  const foo = () => console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
  foo(3, 4);
}(1, 2));

// 화살표 함수 foo의 arguments는 상위 스코프인 전역의 arguments를 가리킨다.
// 하지만 전역에는 arguments 객체가 존재하지 않는다. arguments 객체는 함수 내부에서만 유효하다.
const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError: arguments is not defined
```

arguments 객체는 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다. 하지만 화살표 함수에서는 arguments 객체를 사용할 수 없다. 상위 스코프의 arguments 객체를 참조할 수는 있지만 화살표 함수 자신에게 전달된 인수 목록을 확 인할 수 없고 상위 함수에게 전달된 인수 목록을 참조하므로 그다지 도움이 되지 않는다. 따라서 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다.



## 26.4 Rest 파라미터 

### 26.4.1 기본문법

Rest 파라미터(나머지 매개변수)는 매개변수 이름 앞에 세개의 점...을 붙여서 정의한 매개변수를 의미한다. Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다. 

```javascript
function foo(...rest) {
  // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터다.
  console.log(rest); // [ 1, 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);
```

일반 매개변수와 Rest 파라미터는 함께 사용할 수 있다. 이때 함수에 전달된 인수들은 매개변수와 Rest 파라 미터에 순차적으로 할당된다

```javascript
function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest);  // [ 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);

function bar(param1, param2, ...rest) {
  console.log(param1); // 1
  console.log(param2); // 2
  console.log(rest);   // [ 3, 4, 5 ]
}

bar(1, 2, 3, 4, 5);
```

Rest 파라미터는 이름 그대로 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당된다. 따라서 Rest 파라미터는 반드시 마지막 파라미터이어야 한다


```javascript
function foo(...rest, param1, param2) { }

foo(1, 2, 3, 4, 5);
// SyntaxError: Rest parameter must be last formal parameter
```

 Rest 파라미터는 단 하나만 선언할 수 있다

```javascript
function foo(...rest1, ...rest2) { }

foo(1, 2, 3, 4, 5);
// SyntaxError: Rest parameter must be last formal parameter
```

Rest 파라미터는 함수 정의시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티에 영향을 주지 않는다. 

```javascript
function foo(...rest) {}
console.log(foo.length); // 0

function bar(x, ...rest) {}
console.log(bar.length); // 1

function baz(x, y, ...rest) {}
console.log(baz.length); // 2
```

### 26.4.2 Rest 파라미터와 arguments 객체

ES5에서는 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수의 경우 매개변수를 통해 인수를 전달받는 것이 불가능하므로 arguments 객체를 활용하여 인수를 전달받았다. arguments 객체는 함수 호출 시 전달된 인수 argument들의 정보를 담고 있는 순회 가능한 유사 배열 객체array-like objecto이며, 함수 내부에서 지역 변수처럼 사용할 수 있다. 


```javascript
// 매개변수의 개수를 사전에 알 수 없는 가변 인자 함수
function sum() {
  // 가변 인자 함수는 arguments 객체를 통해 인수를 전달받는다.
  console.log(arguments);
}

sum(1, 2); // {length: 2, '0': 1, '1': 2}
```

하지만 arguments 객체는 배열이 아닌 유사 배열 객체이므로 배열 메서드를 사용하려면 Function. prototype.call이나 Function.prototype.apply 메서드를 사용해 arguments 객체를 배열로 변환해야 하는 번거로움이 있었다. 

```javascript
function sum() {
  // 유사 배열 객체인 arguments 객체를 배열로 변환한다.
  var array = Array.prototype.slice.call(arguments);

  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
```
ES6에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달받을 수 있다. 이를 통 해 유사 배열 객체인 arguments 객체를 배열로 변환하는 번거로움을 피할 수 있다. 

```javascript
function sum(...args) {
  // Rest 파라미터 args에는 배열 [1, 2, 3, 4, 5]가 할당된다.
  return args.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3, 4, 5)); // 15
```

함수와 ES6 메서드는 Rest 파라미터와 arguments 객체를 모두 사용할 수 있다. 하지만 화살표 함수는 함 수 자체의 arguments 객체를 갖지 않는다. 따라서 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다. 




## 26.5 매개변수 기본값
 
 함수를 호출할 때 매개변수의 개수만큼 인수를 전달하는 것이 바람직하지만 그렇지 않은 경우에도 에러가 발 생하지 않는다. 이는 자바스크립트 엔진이 매개변수의 개수와 인수의 개수를 체크하지 않기 때문이다. 인수가 전달되지 않은 매개변수의 값은 undefined다. 이를 방치하면 다음 예제와 같이 의도치 않은 결과가 나올 수 있다.


```javascript
function sum(x, y) {
  return x + y;
}

console.log(sum(1)); // NaN
```

따라서 다음 예제와 같이 매개변수에 인수가 전달되었는지 확인하여 인수가 전달되지 않은 경우 매개변수에 기본값을 할당할 필요가 있다. 즉, 방어 코드가 필요하다.

```javascript
function sum(x, y) {
  // 인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값을 할당한다.
  x = x || 0;
  y = y || 0;

  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1));    // 1
```

ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.

```javascript
function sum(x = 0, y = 0) {
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1));    // 1
```
매개변수 기본값은 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효하다.

```javascript
function logName(name = 'Lee') {
  console.log(name);
}

logName();          // Lee
logName(undefined); // Lee
logName(null);      // null
```

앞서 살펴본 Rest 파라미터에는 기본값을 지정할 수 없다.

```javascript
function foo(...rest = []) {
  console.log(rest);
}
// SyntaxError: Rest parameter may not have a default initializer
```
매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티와 arguments 객체에 아무런 영향을 주지 않는다. 

```javascript
function sum(x, y = 0) {
  console.log(arguments);
}

console.log(sum.length); // 1

sum(1);    // Arguments { '0': 1 }
sum(1, 2); // Arguments { '0': 1, '1': 2 }
```




## 📌중요한 점


## 🤔궁금한 점