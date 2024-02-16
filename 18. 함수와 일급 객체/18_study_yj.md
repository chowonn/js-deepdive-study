## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 18.1 일급 객체

다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체  
여기서 연산이란 변수에 대입하기, 수정하기, 함수에 인자로 넘기기를 말함

다음 조건을 만족하는 객체를 일급 객체라 함

1. 무명의 리터럴로 생성할 수 있음. 즉, 런타임에 생성 가능
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있음
3. 함수의 매개변수에 전달할 수 있음
4. 함수의 반환값으로 사용할 수 있음

자바스크립트의 함수는 위 조건을 모두 만족하므로 일급 객체

```javascript
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
  return ++num;
};

const decrease = function (num) {
  return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수의 매개변수에게 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
  let num = 0;

  return function () {
    num = aux(num);
    return num;
  };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const decreaser = makeCounter(auxs.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미

객체는 값이므로 함수는 값과 동일하게 취급 할 수 있음  
→ 함수는 값을 사용할 수 있는 곳(변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문)이라면 어디서든지 리터럴로 정의할 수 있으며 런타임에 함수 객체로 평가

일급 객체로 함수가 가지는 가장 큰 특징 : 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며, 함수의 반환값으로 사용할 수 있다는 것

함수 객체와 일반 객체의 차이점 : 일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있음, 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유

## 18.2 함수 객체의 프로퍼티

```javascript
function square(number) {
  return number * number;
}

console.dir(square);
```

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/93283a25-050f-4e24-993f-aaa7116ecb3f)

```javascript
function square(number) {
  return number * number;
}

console.log(Object.getOwnPropertyDescriptors(square));
/*
{
  length: {value: 1, writable: false, enumerable: false, configurable: true},
  name: {value: "square", writable: false, enumerable: false, configurable: true},
  arguments: {value: null, writable: false, enumerable: false, configurable: false},
  caller: {value: null, writable: false, enumerable: false, configurable: false},
  prototype: {value: {...}, writable: true, enumerable: false, configurable: false}
}
*/

// __proto__는 square 함수의 프로퍼티가 아니다.
console.log(Object.getOwnPropertyDescriptor(square, "__proto__")); // undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티다.
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

- square 함수의 모든 프로퍼티의 프로퍼티 어트리뷰트를 Object.getOwnProPertyDescriptors 메서드로 확인
- arguments, caller, length, name, prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티
- 이들 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티

### 18.2.1 arguments 프로퍼티

함수 객체의 arguments 프로퍼티 값은 arguments 객체

arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용  
즉, 함수 외부에서는 참조할 수 없음

자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않음

따라서 함수 호출 시 매개변수 개수만큼 인수를 전달하지 않아도 에러가 발생하지 않음

```javascript
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply()); // NaN
console.log(multiply(1)); // NaN
console.log(multiply(1, 2)); // 2
console.log(multiply(1, 2, 3)); // 2
```

- 함수를 정의할 때 선언한 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급
  - 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined로 초기화된 인후 인수가 할당됨
- 선언된 매개변수의 개수보다 인수를 적게 전달했을 경우(multiply(), multiply(1)) 인수가 전달되지 않은 매개변수는 undefined로 초기화된 상태를 유지
- 매개변수의 개수보다 인수를 더 많이 전달한 경우(multiply(1, 2, 3)) 초과된 인수는 무시됨

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/c54aaf3d-2067-4327-92d3-f2d1577dad6d)

- 초과된 인수는 버려지지 않고, 모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관됨

- arguments 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타냄

- arguments 객체의 callee 프로퍼티는 호출되어 arguments 객체를 생성한 함수, 즉 함수 자신을 가리키고 arguments 객체의 length 프로퍼티는 인수의 개수를 가리킴
  - callee 프로퍼티 : 함수의 몸통(body) 내에서 현재 실행 중인 함수를 참조하는 데 쓰임, 이미지에서 Arguments [1, callee: f, ...] 으로 확인 가능

자바스크립트의 특성 때문에 함수가 호출되면 인수 개수를 확인하고 이에 따라 함수의 동작을 달리 정의할 필요가 있을 수 있음

이 때 유용하게 사용되는 것이 arguments 객체, arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용

```javascript
function sum() {
  let res = 0;

  // arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for 문으로 순회할 수 있다.
  for (let i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }

  return res;
}

console.log(sum()); // 0
console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6
```

- arguments 객체는 배열 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 유사 배열 객체
- 유사 배열 객체란 length 프로퍼티를 가진 객체로 for문으로 순회할 수 있는 객체를 말함
- 유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생

```javascript
// ES6 Rest parameter
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3, 4, 5)); // 15
```

- 이러한 번거로움을 해결하기 위해 ES6에서는 Rest 파라미터를 도입

### 18.2.3 length 프로퍼티

함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킴

```javascript
function foo() {}
console.log(foo.length); // 0

function bar(x) {
  return x;
}
console.log(bar.length); // 1

function baz(x, y) {
  return x * y;
}
console.log(baz.length); // 2
```

- arguments 객체의 length 프로퍼티는 인자의 개수를 가리키고, 함수 객체의 length 프로퍼티는 매개변수의 개수를 가리킴

### 18.2.4 name 프로퍼티

함수 객체의 name 프로퍼티는 함수 이름을 나타냄

```javascript
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
var anonymousFunc = function () {};
// ES5: name 프로퍼티는 빈 문자열을 값으로 갖는다.
// ES6: name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 갖는다.
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문(Function declaration)
function bar() {}
console.log(bar.name); // bar
```

### 18.2.5 \_\_proto\_\_ 접근자 프로퍼티

\_\_proto\_\_ 프로퍼티는 [[prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티

내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있음

```javascript
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("__proto__")); // false
```

## 18.2.6 prototype 프로퍼티

prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체

즉, constructor만이 소유하는 프로퍼티

일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없음

```javascript
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty("prototype"); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty("prototype"); // -> false
```

- prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴

## 🤔궁금한 점

## 📌중요한 점

일급 객체의 조건

1. 무명의 리터럴로 생성할 수 있음. 즉, 런타임에 생성 가능
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있음
3. 함수의 매개변수에 전달할 수 있음
4. 함수의 반환값으로 사용할 수 있음

함수 객체는 고유의 프로퍼티를 가짐

- arguments, caller, length, name, prototype 프로퍼티
