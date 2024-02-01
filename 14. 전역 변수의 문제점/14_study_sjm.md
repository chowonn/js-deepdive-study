##변수의 생명 주기

### 지역 변수의 생명 주기

# 14-01

변수는 자신이 선언된 위치에서 생성되고 소멸한다. 전역 변수의 생명 주기는 애플리케이션의 생명주기와 같다.
하지만 함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸한다.

지역변수 x는 foo 함수가 호출되기 이전까지는 생성되지 않는다. foo함수를 호출하지 않으면 함수 내부의 변수 ㅅ선언문이 실행되지 않기 때문이다.

```javascript
function foo() {
  var x = "local";
  console.log(x); // local
  return x;
}

foo();
console.log(x); // ReferenceError: x is not defined
```

호이스팅에서 살펴 보았듯이
변수 선언문은 어디에 있는 상관없이 가장먼저 실행된다. 변수 선언 코드가 한 줄씩 순차적으로 실행되는 시점인
런타임에 실행 되는 것이 아니라 런타임 이전 단계에서ㅏ 자바스크립트 엔진에 의해 먼저 실행된다.

# 14-02

```javascript
var x = "global";

function foo() {
  console.log(x); // ①
  var x = "local";
}

foo();
console.log(x); // global
```

var 키워드로 선언한 전역변수의 생명주기는 전역 객체의 생명 주기와 일치한다.

# 14-03

```javascript
var x = 1;

// ...

// 변수의 중복 선언. 기존 변수에 값을 재할당한다.
var x = 100;
console.log(x); // 100
```

### 전역 변수의 사용을 억제하는 방법

# 14-04

즉시 실행 함수
모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.

```javascript
(function () {
  var foo = 10; // 즉시 실행 함수의 지역 변수
  // ...
})();

console.log(foo); // ReferenceError: foo is not defined
```

# 14-05

네임스페이스 객체
전역에 네임스페이스 역할을 담당할 객체를 생성하고 전역 변수 처럼 사용하고 싶은 변수를 프로퍼티로 추가하는 방법이다.

```javascript
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.name = "Lee";

console.log(MYAPP.name); // Lee
```

# 14-06

```javascript
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.person = {
  name: "Lee",
  address: "Seoul",
};

console.log(MYAPP.person.name); // Lee
```

# 14-07

```javascript
var Counter = (function () {
  // private 변수
  var num = 0;

  // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환한다.
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    },
  };
})();

// private 변수는 외부로 노출되지 않는다.
console.log(Counter.num); // undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```

# 14-08

ES6모듈을 사용하면 더는 전역 변수를 사용할 수 없다.
ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다.

```html
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```
