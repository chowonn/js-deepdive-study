# 목차

[📗배운 점](#📗배운-점)

[📌중요한 점](#📌중요한-점)

[🤔궁금한 점](#🤔궁금한-점)

## 📗배운 점

### 21.1

객체 세가지
1. **표준 빌트인 객체** - ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역의 기능을 제공한다. 사양에 미리 정의된 객체이기에 자바스크립트 실행 환경에 구애받지 않고 언제나 사용 가능.
전역 객체의 프로퍼티로서 제공됨. 별도 선언 없이 전역 변수처럼 언제나 참조 가능
2. **호스트 객체** - 사전 정의되지는 않았지만 자바스크립트 실행 환경에서 추가 제공되는 객체를 말한다. DOM,BOM,Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 Web API를 호스트 객체로 제공, Node.js 환경을 고유의 API를 호스트 객체로 제공한다.
3. **사용자 정의 객체** - 사용자가 직접 정의한 객체

### 21.2 표준 빌트인 객체

Object, String, Number ... 등 40여 개의 표준 빌트인 객체를 제공한다.
Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수이다. 생성자 빌트인 객체는 프로토타입 메서드, 정적 메서드를 제공한다. 그 외에는 정적 메서드만 제공

```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}
console.log(typeof strObj);       // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj);     // object

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj= new Boolean(true); // Boolean {true}
console.log(typeof boolObj);      // object

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x'); // ƒ anonymous(x )
console.log(typeof func);                       // function

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3); // (3) [1, 2, 3]
console.log(typeof arr);        // object

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i); // /ab+c/i
console.log(typeof regExp);         // object

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();  // Fri May 08 2020 10:43:25 GMT+0900 (대한민국 표준시)
console.log(typeof date); // object
```

생성자 함수인 표준 빌트인 객체가 생성한 인스턴스 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체이다. 

```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```
정적 메서드도 제공한다.

```javascript
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}

// toFixed는 Number.prototype의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2

// isInteger는 Number의 정적 메서드다.
// Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 Boolean으로 반환한다.
console.log(Number.isInteger(0.5)); // false
```
Number.prototype의 다양한 프로토타입 메서드는 모든 Number 인스턴스가 상속을 통해 사용가능하다. 

### 21.3 원시값과 래퍼 객체

원시값 /= 객체 즉, 프로퍼티나 메서드 없음. 그러나 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환 사용한다. 그러면 프로퍼티에 접근하거나 메서드를 호출 후 다시 원시값으로 되돌린다.

> **래퍼 객체**란 문자열, 숫자, 불리언 값에 대해 객체처럼 접근시 생성되는 임시 객체를 의미한다.

```javascript
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

문자열에 대해 마침표 표기법으로 접근하니 래퍼 객체인 String 생성자 함수의 인스턴스가 생성됨.
문자열은 즉시 래퍼 객체의 [StringData] 내부 슬롯에 할당됨. 
이때, 문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype의 메서드를 상속받아 사용할 수 있다.

그 후 식별자가 원시값을 갖도록 되돌려주고 래처 객체는 가비지 컬렉터가 꿀꺽

Math, Reflect, JSON제외한 빌트인 객체는 new 붙일 필요 없음 자동으로 그렇게 되니까 ~ 

### 21.4 전역 객체

코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 제일 먼저 생성되는 최상위 객체

명칭
1. 브라우저 - window
2. Node.js - global    

전역 객체의 프로퍼티 종류
1. 표준 빌트인 객체
2. 호스트 객체
3. var 키워드 전역 변수 / 전역 함수

특징

1. 전역 객체는 개발자가 의도적 생성 불가. 생성할 수 있는 생성자 함수가 없음
2. 전역 객체의 프로퍼티 참조시 window를 생략 가능
3. 전역 객체는 모든 표준 빌트인 객체를 프로퍼티로 가짐
4. 실행활경에 따라 추가 프로퍼티와 메서드를 가짐 브라우저와 Node.js에 따라 다름
5. var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역 함수는 전역 객체의 프로퍼티가 된다.
6. let이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님. window.foo 같이 접근 불가. let, const 는 보이지 않는 개념적인 블록 (전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재
7. 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유. 여러 개로 분리해서 하나로 공유. 하나의 전역을 공유

#### 21.4.1 빌트인 전역 프로퍼티

전역 객체의 프로퍼티

애플리케이션 전역에서 사용하는 값 제공

1. Infinity 무한대를 나타내는 숫자값

```javascript
// 전역 프로퍼티는 window를 생략하고 참조할 수 있다.
console.log(window.Infinity === Infinity); // true

// 양의 무한대
console.log(3/0);  // Infinity
// 음의 무한대
console.log(-3/0); // -Infinity
// Infinity는 숫자값이다.
console.log(typeof Infinity); // number
```

2. NaN 숫자가 아님

```javascript
console.log(window.NaN); // NaN

console.log(Number('xyz')); // NaN
console.log(1 * 'string');  // NaN
console.log(typeof NaN);    // number 나름 Number
```

3.undefined 원시 타입

```javascript
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined 나만의 타입이 있다.
```

#### 21.4.2 빌트인 전역 함수

애플리케이션 전역 호출 가능 빌트인 함수
전역 객체의 메서드

1. eval 자바스크립트 코드를 나타내는 문자열을 인수로 받는다.
전달받은 문자열 코드가 표현식이면 eval 함수가 런타임에 평가하여 값을 생성, 문이라면 런타임에 실행, 여러 개의 문이라면 모든 문을 실행

```javascript
// 표현식인 문
eval('1 + 2;'); // -> 3
// 표현식이 아닌 문
eval('var x = 5;'); // -> undefined

// eval 함수에 의해 런타임에 변수 선언문이 실행되어 x 변수가 선언되었다.
console.log(x); // 5

// 객체 리터럴은 반드시 괄호로 둘러싼다.
const o = eval('({ a: 1 })');
console.log(o); // {a: 1}

// 함수 리터럴은 반드시 괄호로 둘러싼다.
const f = eval('(function() { return 1; })');
console.log(f()); // 1
```

여러개의 문이라면 모든 문 실행 후 값내기

```javascript
console.log(eval('1 + 2; 3 + 4;')); // 7
```

eval은 호출된 위치에 해당하는 기존 스코프를 런타임에 동적 수정함. 

```javascript
const x = 1;

function foo() {
  // eval 함수는 런타임에 foo 함수의 스코프를 동적으로 수정한다.
  eval('var x = 2;');
  console.log(x); // 2
}

foo();
console.log(x); // 1 // 기존 스코프를 런타임에 동적으로 수정해서 1 !
```

! eval 함수 보안 취약 사용 자제할 것

2. isFinite

전달받은 이수가 정상적인 유한수인가 아닌가 맞으면 true 아니면 false 
인수 타입이 숫자가 아니라면 숫자로 타입 변환 후 검사 수행 Nan이면 false , null은 0이라 true

3. isNaN

NaN인지 아닌지 확인 전달받은 인수가 숫자가 아니면 숫자로 타입 변환후 확인

4. parseFloat

부동 소수점 숫자, 즉 실수로 해석하여 반환

```javascript
// 숫자
isNaN(NaN); // -> true
isNaN(10);  // -> false

// 문자열
isNaN('blabla'); // -> true: 'blabla' => NaN
isNaN('10');     // -> false: '10' => 10
isNaN('10.12');  // -> false: '10.12' => 10.12
isNaN('');       // -> false: '' => 0
isNaN(' ');      // -> false: ' ' => 0

// 불리언
isNaN(true); // -> false: true → 1
isNaN(null); // -> false: null → 0

// undefined
isNaN(undefined); // -> true: undefined => NaN

// 객체
isNaN({}); // -> true: {} => NaN

// date
isNaN(new Date());            // -> false: new Date() => Number
isNaN(new Date().toString()); // -> true:  String => NaN
```
5. parseInt
  
정수로 해석해 반환


```javascript
// 문자열을 실수로 해석하여 반환한다.
parseFloat('3.14');  // -> 3.14
parseFloat('10.00'); // -> 10

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseFloat('34 45 66'); // -> 34
parseFloat('40 years'); // -> 40

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseFloat('He was 40'); // -> NaN

// 앞뒤 공백은 무시된다.
parseFloat(' 60 '); // -> 60
```
전달 받은게 문자열이 아니면 문자열로 변환 후 정수로 해석 반환함.

6. encodeURI/decodeURI
완전한 URI 전달받아 이스케이프 처리를 위해 인코딩 한다. URI는 인터넷에 있는 자원을 나타내는 유일한 주소 ! 

#### 21.4.3 암묵적 전역

미리 선언하지 않은 식별자에 대해 자바스크립트 엔진이 window.식별자명 = ? 으로 해석해서 전역 객체에 프로퍼티를 동적 생성함.

결국 선언하지 않는 식별자는 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작한다. 

이를 **암묵적 전역**이라 한다.

선언하지 않는 식별자는 전역객체로 프로퍼티에 추가만 된 것 변수가 아니라서 변수 호이스팅이 발생하지 않음 주의 !

delete 연산자로 삭제 가능 ~

## 📌중요한 점

## 🤔궁금한 점