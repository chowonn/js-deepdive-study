# 📗챕터 정리

생성자 함수란 `new` 키워드와 함께 호출하여 객체를 생성하는 함수를 말한다.

이 때, 생성자 함수에 의해 생성된 객체는 인스턴스(Instance)라고 한다.

`new` 키워드와 함께 `Object` 생성자 함수를 호출하여 빈 객체를 생성할 수 있다.

생성자 함수에 의해 생성된 빈 객체에는 프로퍼티나 메서드를 추가하고 수정할 수 있다.

```jsx
const user = new Object();

console.log(user); // {}

user.name = "gyu";
user.age = 27;

console.log(user); // {name: 'gyu', age: 27}
```

`Object` 외에도 `String`, `Number`, `Boolean`, `Function`, `Array`, `Date`, `RegExp`, `Promise` 등의 언어 설계 과정에서 미리 만들어진 빌트인(built-in) 생성자 함수를 제공한다.

빌트인 생성자 함수는 `new` 키워드와 함께 호출되었는지 확인한 후, 적절한 값을 반환한다.

그러나, `Object`와 `Function` 생성자 함수는 new 연산자를 생략하더라도 동일하게 동작한다.

```jsx
let obj = new Object();
console.log(obj); // {}

obj = Object();
console.log(obj); // {}

let f = new Function("x", "return x ** x");
console.log(f); // ƒ anonymous(x) { return x ** x }

f = Function("x", "return x ** x");
console.log(f); // ƒ anonymous(x) { return x ** x }
```

### 객체 리터럴 vs Object 생성자 함수

> 객체 리터럴

객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하다.

단, 객체 리터럴 방식은 하나의 객체만 생성할 수 있다.

프로퍼티 구조가 동일한 여러 객체를 생성하는 경우에는 적절하지 않다.

> Object 생성자 함수

생성자 함수는 내부 프로퍼티 구조가 동일한 객체를 여러 개 생성해야 할 때 유용하게 사용된다.

```jsx
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

생성자 함수는 구조가 동일한 객체를 생성하기 위한 템플릿으로서, 인스턴스 생성 및 초기화 후 반환한다.

하지만, 생성자 함수 내부에는 초기화 이외에 인스턴스를 생성하거나 반환하는 코드는 존재하지 않는다.

이는 `new` 키워드를 사용하여 생성자 함수를 호출할 때, 자바스크립트 엔진이 암묵적으로 생성과 반환을 수행하기 때문이다.


---
#### **this ?**

💡 this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수를 의미한다.

`this` 바인딩은 this 식별자와 값을 연결하는 과정을 의미하며, 바인딩 결과 값은 함수 호출 방식에 따라 동적으로 결정된다.

1. 일반 함수에서 호출된 `this` : 전역 객체를 참조
2. 메서드에서 호출된 `this` : 메서드를 호출한 객체를 참조
3. 생성자 함수에서 호출된 `this` : 생성자 함수가 생성할 인스턴스 객체를 참조

```jsx
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

자세한 내용은 this 파트에서 다루도록 한다.

---

자바스크립트는 객체로 이루어진 언어이다.

함수 역시 일반 객체가 가지고 있는 내부 슬롯과 메서드를 모두 가지고 있어 일반 객체와 동일하게 동작할 수 있다.

하지만 일반 객체와 달리 함수는 함수로서 동작하기 위해 존재하는 내부 슬롯(`[[Environment]]`, `[[FormalParameters]]`)과 메서드(`[[Call]]`, `[[Construct]]`)를 호출할 수 있다.

함수 내부에 `[[Call]]` 메서드가 존재한다면 호출 가능한 객체인 함수임을 의미한다.

이는 `[[Call]]` 메서드가 없다면 함수 객체로 볼 수 없다는 말이기도 하다.

함수를 일반 함수로 사용하면 `[[Call]]` 메서드가 호출된다.

함수 내부에 `[[Construct]]` 메서드가 존재한다면 생성자 함수로 호출할 수 있는 함수임을 의미한다.

이러한 함수를 생성자 함수로 사용하면 `[[Construct]]` 메서드가 호출된다.

`[[Call]]` 메서드와 달리 함수 내부에 `[[Construct]]` 메서드가 존재하지 않을 수도 있는데, 이는 생성자 함수로는 함수를 호출할 수 없음을 의미한다.

생성자 함수로 호출할 수 있는지 여부에 따라 constructor / non-constructor 함수로 구분하기도 한다.

일반적으로는 ECMAScript 사양에 따라서 다음과 같이 구분된다.

- constructor 함수 : **함수 선언문**, **함수 표현식**, **클래스**
    
    ```jsx
    // 일반 함수 정의: 함수 선언문, 함수 표현식
    function foo() {}
    const bar = function () {};
    // 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
    const baz = {
      x: function () {}
    };
    
    // 일반 함수로 정의된 함수만이 constructor이다.
    new foo();   // -> foo {}
    new bar();   // -> bar {}
    new baz.x(); // -> x {}
    ```
    
- non-constructor 함수 : **메서드**, **화살표 함수**
    
    ```jsx
    // 화살표 함수 정의
    const arrow = () => {};
    
    new arrow(); // TypeError: arrow is not a constructor
    
    // 메서드 정의: ES6의 메서드 축약 표현만을 메서드로 인정한다.
    const obj = {
      x() {}
    };
    
    new obj.x(); // TypeError: obj.x is not a constructor
    ```


### new.target
생성자 함수가 `new` 키워드 없이 호출되는 것을 방지하기 위해 ES6 버전부터 `new.target` 을 지원한다.

`new.target`은 cunstructor 함수 내부에서 암묵적인 지역 변수와 같이 사용되는 this와 유사한 개념이다.

함수 내부에서 `new.target`을 사용하면 `new` 키워드와 함께 생성자 함수로서 호출되었는지 확인할 수 있다.

생성자 함수로 호출되었을 경우, `new.target`은 함수 자신을 가리키게 되고,

`new` 키워드 없이 일반 함수로 호출되었을 때는 undefined 를 반환한다.

```jsx
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());  // 10
```

<br />

## 🤔궁금한 점

## 📌중요한 점
