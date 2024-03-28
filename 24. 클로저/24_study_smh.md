# 목차

[📗배운 점](#📗배운-점)

[📌중요한 점](#📌중요한-점)

[🤔궁금한 점](#🤔궁금한-점)

## 📗배운 점

## 24.1 렉시컬 스코프

```javascript
const x = 1;

function outerFunc() {
  const x = 10;

  function innerFunc() {
    console.log(x); // 10
  }

  innerFunc();
}

outerFunc();
```

innerFunc 함수는 outerFunc 안에 있는 중첩함수여서 innerFunc의 상위 스코프가 outerFunc가 된다. 즉, innerFunc가 외부 함수인 outerFunc의 x 변수에 접근 가능하다.


> JS에서는 자신이 정의된 곳이 중요하다. 어디에 정의 되었느냐에 따라 상위 스코프를 결정하기 때문. 이를 렉시컬/정적 스코프라 한다.

```javascript
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // ?
bar(); // ?
```
foo랑 bar가 지금 밖에 선언된거라 상위 스코프는 `전역`이다.

**함수의 상위 스코프**는 함수를 정의한 위치에 의해 정적으로 결정되고 변하지 않음.

스코프의 실체 = 실행 컨텍스트의 렉시컬 환경

**스코프 체인** - `외부 렉시컬 환경에 대한 참조`를 통해 상위 렉시컬 환경과 연결된다.

>렉시컬 환경의 '외부 렉시컬 환경에 대한 참조'에 의해 저장할 참조값. 즉, 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 정의된 환경(위치)에 따라 결정된다. 이것이 바로 렉시컬 스코프이다.

## 24.2 함수 객체의 내부 슬록 [envirenment]

자신이 정의된 환경을 기준으로 함수는 자신의 내부 슬롯 [envirenment]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.

**[Environment]** ? 함수는 자신이 만들어진 주변 환경, 즉 '어디에서' 정의되었는지에 대한 정보를 기억한다. 이를 위해서 함수 내부에는 자신이 생성될 때의 환경을 참조하는 내부 공간이 있을 말한다.

예)어떤 함수가 전역 환경에서 만들어졌다면, 그 함수는 전역 환경에 대한 정보를 [[Environment]]에 저장해둠. 만약 다른 함수 안에서 새로운 함수가 만들어진다면, 그 새 함수는 자신이 만들어진 그 '부모 함수'의 환경 정보를 [[Environment]]에 저장함. (전역에서 만들어졌으면 상위 스코프인 전역을 ! 중첩 함수로 만들어졌으면 나를 싸고 있는 함수 정보를 상위 스코프로 잡고 저장 ~)

> 함수가 자신이 만들어진 환경을 기억하는 이유 ? 나중에 그 함수가 어디서 호출되든 간에, 자신이 처음 만들어졌을 때의 주변 환경(변수나 다른 함수들이 뭐가 있었는지 등)을 잊지 않고 사용하기 위함.

함수는 자신이 태어난 '집'의 주소를 항상 가지고 다니며, 필요할 때마다 그 '집'을 찾아가서 자신이 사용해야 할 정보들을 꺼내 쓸 수 있는 것. 이런 메커니즘 덕분에, 함수는 자신이 어디서 정의되었는지에 관계없이, 필요한 정보와 변수들을 안정적으로 접근할 수 있게 되는 것.

```javascript
const x = 1;

function foo() {
  const x = 10;

  // 상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
  // 함수 호출 위치와 상위 스코프는 아무런 관계가 없다.
  bar();
}

// 함수 bar는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Environment]]에 저장하여 기억한다.
function bar() {
  console.log(x);
}

foo(); // ?
bar(); // ?
```

foo/bar 모두 전역에서 정의됨. foo/bar 모두 전역 코드가 평가되는 시점에 foo/bar도 평가되어 함수 객체를 생성하고 전역 객체 window의 메서드가 된다.

이때 [envirenment] 슬롯에는 전역 코드 평가 시점에 실행 중인 실행 컨텍스트의 렉시컬 환경인 `전역 렉시컬 환경`의 참조가 저장된다.

함수 코드 평가 순서

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
 2-1. 함수 환경 레코드 생성
 2-2. this 바인딩
 2-3. 외부 렉시컬 환경에 대한 참조 결정

함수 렉시컬 환경의 구성 요소인 외부 렉시컬 환경에 대한 함조에는 함수 객체의 내부 슬롯[envirenment]에 저장된 렉시컬 환경의 참조가 할당된다. 즉, 함수 객체의 내부 슬롯 [envirenment]에 저장된 렉시컬 환경의 참조는 바로 함수의 상위 스코프를 의미한다.

이것이 함수 정의 위치에 따라 상위 스코프를 결정하는 렉시컬 스코프의 실체다.


## 24.3 클로저와 렉시컬 환경

```javascript
const x = 1;

// ①
function outer() {
  const x = 10;
  const inner = function () { console.log(x); }; // ②
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```
1. outer 호출하니까 당근 자기 안에 있던 inner를 반환하고 사망(생명 주기 마감)함.
2. 생명 주기 마감하니까 outer 실행 컨텍스트는 실행 컨텍스트 스택에서 pop됨.
3. outer 함수의 지역 변수 x와 변수 값 10을 저장했던 실행 컨텍스트가 제거된거라서 (outer가 걍 실행 컨텍스트에서 pop 제거 되버렸으니까)
4. outer 안에 있던 x 변수도 사망
5. outer 함수 내 x 변수 더 이상 유효하지 않음

이게 정상인데 ?

`innerFunc(); // ④ 10` 여기서 10이 나와버림
why? 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료된 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 closer라 한다. 

[["A closure is the combination of a function and the lexical environment within which that function was declared"]] 
클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

 > "그 함수가 선언된 렉시절 환경"이란 ?  함수가 정의된 위치의 스코프, 즉 상위 스코프를 의미하는 실행 컨텍스트의 렉시컬 환경을 말한다. 자바스크립트의 모든 함수는 자신의 상위 스코프를 기억한다고 했다. 모든 함수가 기억하는 상위 스코프는 함수를 어디서 호출하든 상관없이 유지된다. 따라서 함수를 어디서 호출하든 상관없이 함수는 언제나 자신이 기억하는 상위 스코프의 식별자를 참조할 수 있으며 식별자에 바인딩된 값을 변경할 수도 있다. 
 
 
 위 예제에서 inner 함수는 자신이 평가될 때 자신이 정의된 위치에 의해 결정된 상위 스코프를 [[Environment]] 내부 슬롯에 저장한다. 이때 저장된 상위 스코프는 함수가 존재하는 한 유지된다. 
 
  위 예제에서 outer 함수가 평가되어 함수 객체를 생성할 때(①) 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 전역 렉시컬 환경을 outer 함수 객체의 [Environment] 내부 슬롯에 상위 스코 프로서 저장한다.

실행 컨텍스트에서 outer 사망을 인지하고 pop 했지만 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것이 아니다. 
가비지 컬렉터는 inner가 참조하고 있으니깐 컬렉팅 해가지 않았다. 그래서 참조 가넝 ~

그래서 inner 호출하며는 실행 컨텍스트 생성되고 스택에 푸시되고 함수 내부 [Environment] 슬롯에 있는 참조값 써먹는다.

외부보다 오래 살아남은 중첩 함수 inner는 외부 사망 소식에도 흔들림없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억했다 써먹는다. 상위 스코프를 참조 가능하니 식별자를 참도할 수 있고 이는 값을 변경도 할 수 있다는 것이다.

클로저는 상위 스코프를 기억하는 함수를 말한다고 했지만 그렇다고 모두가 그런 것은 아님
지 값을 가지고 지거만 쓰고 있으면 클로저 아님 상위 스코프거를 참조 중이어야만 ! 클로저임

```html
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;
      const y = 2;

      // 일반적으로 클로저라고 하지 않는다.
      function bar() { // 나 클로저 아니다 !!! 나는 내거만 쓰고 있다구 ! 
        const z = 3;

        debugger;
        // 상위 스코프의 식별자를 참조하지 않는다.
        console.log(z);
      }

      return bar;
    }

    const bar = foo();
    bar();
  </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;

      // 일반적으로 클로저라고 하지 않는다.
      // bar 함수는 클로저였지만 곧바로 소멸한다.
      function bar() {
        debugger;
        // 상위 스코프의 식별자를 참조한다.
        console.log(x); // 상위 스코프 값을 쓰는데 왜 클로저가 아니냐고 ?
        //값을 쓰고 있는 (호출된 위치)가 
        //상위 스코프의 영역 내라서 외부 함수 foo보다 
        //중첩함수 bar가 먼저 죽어서 아닌거
      }
      bar();
    }

    foo();
  </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;
      const y = 2;

      // 클로저
      // 중첩 함수 bar는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자를 참조한다.
      function bar() {
        debugger;
        console.log(x);
      }
      return bar;
    }

    const bar = foo();
    bar();
  </script>
</body>
</html>
```
> 클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.

**자유 변수** : 클로저에 의해 참조되는 상위 스코프의 변수

## 24.4 클로저의 활용

> 클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다. 상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.

```javascript
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태를 1만큼 증가 시킨다.
  return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```
num을 누군가 건들면 쉬이 변경되는 상태이다.

```javascript
// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태 변수
  let num = 0;

  // 카운트 상태를 1만큼 증가 시킨다.
  return ++num;
};

// 이전 상태를 유지하지 못한다.
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```
이렇게 바꾸면 ? num을 increase 함수의 지역 변수로 변경하여 increase 함수만 변경이 가능하다.

그러나 1씩 증가해야 하는 기능을 하지 못 한다. 호출될때마다 num이 다시 선언되고 0으로 초기화 된다.

```javascript
// 카운트 상태 변경 함수
const increase = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저
  return function () {
    // 카운트 상태를 1만큼 증가 시킨다.
    return ++num;
  };
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```
실행해보면 실행 함수가 반환한 값이 increase 변수에 할당된다.
increase 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저다.

`즉시 실행 함수(Immediately Invoked Function Expression, IIFE)`는 정의되자마자 바로 실행되는 함수. 주로 코드를 모듈화하고 전역 변수의 오염을 방지하기 위해 사용

클로저(Closure)는 함수와 그 함수가 선언될 때의 렉시컬 환경과의 조합. 쉽게 말해, 한 함수가 자신이 생성될 때의 환경을 기억하는 기능. 이를 통해 해당 함수는 자신이 생성될 때의 변수나 상태를 나중에도 접근할 수 있다.


1. increase 변수에 할당된 즉시 실행 함수는 바로 실행되고 그 안에서 변수인 `let num = 0;`을 가지고 있고 이 변수는 카운트 변경 상태를 기록해 담는다.
2. 즉시 실행 함수는 또 다른 자기 안의 내부 함수를 반환한다.
3. 반환된 함수 즉, 내부 함수가 클로저로서 자신이 생성될 때의 환경(즉시 실행 함수의 num 변수 포함)을 '기억'한다.
4. increase를 호출할 때마다, 이 함수(클로저)는 자신이 기억하고 있는 num 변수의 값을 증가시킨다.
5. num 변수는 increase 함수 밖에서는 접근할 수 없다. 즉, 이 변수는 "은닉된" 상태로, 오직 increase 함수를 통해서만 변경될 수 있다.
6. 즉시 실행 함수는 이미 실행되었고 소멸했지만, 그 안에서 생성된 클로저(increase 함수)는 여전히 살아 있으며, num 변수에 접근하고 변경할 수 있습니다.
  
  
  > 즉시 실행 함수를 사용해 은닉된 상태(num 변수)를 만들고, 이 상태를 변경할 수 있는 유일한 방법(클로저를 통한)을 제공하는 방법을 알려줌. 이를 통해 안정적이고 의도치 않은 변경으로부터 보호된 프로그래밍을 할 수 있다.

```javascript
const Counter = (function () {
  // ① 카운트 상태 변수
  let num = 0;

  function Counter() {
    // this.num = 0; // ② 프로퍼티는 public하므로 은닉되지 않는다.
  }

  Counter.prototype.increase = function () {
    return ++num;
  };

  Counter.prototype.decrease = function () {
    return num > 0 ? --num : 0;
  };

  return Counter;
}());

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```
1. 즉시 실행 함수와 클로저의 활용: 즉시 실행 함수와 클로저를 활용하여 카운트 상태(num)를 안전하게 은닉하고, 이 상태를 변경할 수 있는 메서드(increase, decrease)를 제공하는 Counter 함수를 만든 것.
   
2. 즉시 실행 함수: 코드 최상단에 정의된 함수가 즉시 실행 함수. 정의되자마자 바로 실행되며, Counter 함수를 반환. 이 즉시 실행 함수 내부에서 num 변수와 increase, decrease 메서드가 정의. 즉시 실행 함수는 한 번 실행된 후 사망, 그 안에서 생성된 num 변수와 메서드들은 사라지지 않음.
   
3. 클로저의 역할: increase와 decrease 메서드는 num 변수에 접근할 수 있는 클로저를 형성. `클로저`란, 함수가 정의될 당시의 렉시컬 환경을 기억하는 함수. 여기서 렉시컬 환경이란, 해당 함수가 정의된 환경, 즉 변수 등이 존재하는 스코프를 말한다. 이 경우, increase와 decrease 메서드는 num 변수가 정의된 즉시 실행 함수의 스코프를 기억함.
   
4. 은닉화와 안전성: num 변수는 즉시 실행 함수 내부에 있기 때문에 외부에서 직접 접근할 수 없다. 이는 num 변수가 은닉되어 있기 때문이며, 오직 increase와 decrease 메서드를 통해서만 num 변수의 값에 접근하고 변경할 수 있다. 이로 인해 num 변수의 값이 의도치 않게 변경되는 것을 방지한다.
   
5. Counter 함수의 활용: Counter 함수는 즉시 실행 함수에 의해 반환된 후, counter라는 변수에 할당된다. 이제 counter 변수를 통해 increase와 decrease 메서드를 호출할 수 있으며, 이 메서드들은 num 변수의 값을 증가시키거나 감소시키는 역할을 해준다. 이 과정에서 num 변수는 계속해서 은닉된 상태를 유지한다.

이 코드는 즉시 실행 함수와 클로저를 활용하여 num 변수를 안전하게 은닉하면서도, 특정 메서드를 통해 num 변수의 값을 조작할 수 있는 구조를 만드는 방법을 사용함. 이를 통해 안전하고 안정적인 코드 작성이 가능해짐.


>변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본 원인이 된다.
>외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수 효과를 최대한 억제하여 안전성을 높이기 위해 클로저를 사용한다.


```javascript
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(aux) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 클로저를 반환
  return function () {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다
const increaser = makeCounter(increase); // ①
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease); // ②
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```
1. makeCounter라는 함수는 클로저를 생성하는 고차 함수(higher-order function). 
   **고차 함수**: 함수를 인자로 받거나 함수를 반환하는 함수. makeCounter는 인자로 보조 함수(aux)를 받고, 내부에서 counter라는 변수를 사용해 호출될 때마다 그 값을 변화시키는 새로운 함수를 반환.

2. 보조 함수 increase와 decrease는 각각 받은 숫자(n)에 대해 1을 더하거나 빼는 역할. makeCounter에 increase를 인자로 넣어 호출하면, increaser라는 새로운 함수가 생성되고, 이 함수는 내부적으로 counter 값을 1씩 증가시키는 클로저가 되는 것. 마찬가지로 decrease를 인자로 넣어 호출하면, counter 값을 1씩 감소시키는 decreaser 클로저가 생성됨.

3. 중요한 점! increaser와 decreaser는 각각 독립된 counter 값을 가진다. 
  왜냐하면 makeCounter 함수를 호출할 때마다 그 함수 내에서 사용되는 변수들을 포함한 새로운 환경인 렉시컬 환경(즉, 변수들이 저장된 공간)이 새로 생성되기 때문. 그래서 increaser와 decreaser는 서로 다른 counter 값을 '기억'하게 되는 거고, 이를 통해 독립적으로 작동할 수 있게됨. 
  이렇게 생성된 렉시컬 환경은 함수가 '사망'하는 시점과는 다른 의미를 가진다. 
  함수가 실행을 마치고 반환을 하더라도, 그 함수 내부에서 생성된 클로저가 외부에서 계속 참조되고 있다면, 해당 클로저는 그 렉시컬 환경(변수들이 저장된 공간)을 계속 유지한다. 이는 클로저가 해당 함수의 변수들에 계속 접근할 수 있게 하기 위함이다.

4. 즉, makeCounter 함수를 호출할 때마다 각각 독립된 counter 변수를 가지는 새로운 렉시컬 환경이 생성되고, 이 환경은 increaser나 decreaser 같은 클로저에 의해 '기억'된다. 이 클로저들은 makeCounter 함수의 실행이 끝난 후에도 해당 렉시컬 환경에 접근할 수 있기 때문에, 각각 독립적으로 counter 값이 유지되게 됨.
`함수가 실행될 때마다 새로운 렉시컬 환경이 생성되고, 클로저를 통해 이 환경이 계속 유지될 수 있다는 점에 주목하는 것이 중요`. 클로저가 외부에서 계속 참조될 때 해당 렉시컬 환경은 사라지지 않고 계속 유지된다.

클로저 ~ 캡슐화 은닉 !!!


5. makeCounter 함수의 실행 컨텍스트와 렉시컬 환경: makeCounter 함수를 호출하면, 해당 함수만의 실행 컨텍스트가 생성된다. 이 실행 컨텍스트는 함수가 실행되는 동안 필요한 정보를 담고 있으며, 함수가 실행을 마치면 소멸된다. 하지만, 함수 실행 중에 생성된 렉시컬 환경은 함수가 종료된 후에도 소멸되지 않을 수 있다. 이는 makeCounter 함수가 반환하는 클로저 때문이다. 클로저는 함수가 실행될 때 생성된 렉시컬 환경을 '기억'합니다. 이러한 메커니즘은 클로저가 해당 함수의 변수들에 계속해서 접근할 수 있게 해준다.
6. 독립된 카운터: 위의 설명대로, makeCounter 함수를 한 번 호출할 때마다, 해당 함수만의 독립된 렉시컬 환경이 생성된다. 그리고 이 렉시컬 환경은 makeCounter 함수가 반환하는 클로저에 의해 '기억'된다. 이 클로저는 increaser나 decreaser와 같은 전역 변수에 할당될 수 있고, 각각 독립된 counter 변수를 가지게 된다. 따라서, makeCounter 함수를 두 번 호출하면 increaser와 decreaser는 서로 다른 두 개의 렉시컬 환경을 기반으로 하기 때문에, 각각 독립된 카운터를 가지게 됩니다.
7. 연동되는 카운터: 만약 카운트의 증감이 서로 연동되게 하고 싶다면, increaser와 decreaser가 동일한 렉시컬 환경을 공유해야 한다. 이를 위해서는 makeCounter 함수를 두 번 호출하지 않고, 단 한 번만 호출하여 생성된 클로저를 공유하는 방식으로 접근해야 한다. 예를 들어, makeCounter 함수가 카운터를 증가시키는 함수와 감소시키는 함수를 모두 반환하고, 이 두 함수가 동일한 counter 변수를 공유하도록 설계할 수 있는데, 반환된 두 함수는 같은 counter 변수를 조작하게 되므로, 카운터의 증감이 서로 연동되게 되는 것.
8. 즉, makeCounter를 여러 번 호출하면 독립된 카운터를 만들 수 있지만, 카운터의 증감을 서로 연동시키고 싶다면, makeCounter를 한 번만 호출하고 반환된 클로저들이 동일한 렉시컬 환경을 공유하도록 해야 한다. 이를 통해 같은 counter 변수에 접근하여 카운터의 증감을 연동시킬 수 있다.

## 24.5 캡슐화와 정보 은닉

**캡슐화(encapsulation)**는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.

캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉(information hiding)이라 한다. 

객체의 상태가 변경되는 것을 방지해 정보를 보호하고, 객체 간의 상호 의존성, 즉, 결합도(coupling)를 낮추는 효과가 있다. 

자바스크립트에서 객체의 모든 프로퍼티와 메서드는 기본적으로 공개적 형태이다.

```javascript
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private

  // 인스턴스 메서드
  this.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };
}

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

name은 공개상태. _age 변수는 person 생성자 함수의 지역 변수로서 person 생성자 함수 외부에서 참조하거나 변경 불가하다. 즉 _age 함수는 private하다.

sayHi 메서드가 인스턴스 메서드라서 person 객체 생성시마다 중복 생성됨. 
```js
Person.prototype.sayHi = function () {
  // Person 생성자 함수의 지역 변수 _age를 참조할 수 없다
  console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
};
```
이렇게 바꾸면 중복 생성 방지됨. 그러나 Person.prototype.sayHi 메서드 내에서 Person 생성자 함수의 지역 변수 _age를 참조할 수 없는 문제가 발생함.

```javascript
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

즉시 실행 함수를 사용해서 Person 생성자 함수와  Person.prototype.sayHi 메서드를 모음
즉시 실행 함수가 반환하는 Person 생성자 함수와 Person 생성자 함수의 인스턴스가 상속받아 호툴할 Person.prototype.sayHi메서드는 즉시 실행 함수가 종료된 이후 호출됨. 하지만 Person 생성자 함수와 sayHi 메서드는 이미 종료되어 소멸한 즉시 실행 함수의 지역 변수  _age를 참조 할 수 있는 클로저이다.

_age는 즉시 실행 함수(IIFE, Immediately Invoked Function Expression) 내에 선언되어 있으며, Person 생성자 함수의 외부에서는 접근할 수 없는 비공개(private) 변수가 됨.

문제 - 모든 Person 인스턴스가 생성될 때 _age 변수가 공유되어 사용된다. 즉, 새로운 Person 인스턴스를 생성할 때마다 _age 값이 마지막에 생성된 인스턴스의 나이로 업데이트되며, 이는 모든 인스턴스의 sayHi 메서드에서 같은 나이(마지막에 설정된 나이)를 출력하게 만듬.

## 24.6 자주 발생하는 실수

```javascript
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () { return i; }; // ①
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // ②
}
```
var는 전역인데 이걸로 선언하면 기대한 0,1,2가 나오지 않음

```javascript
const funcs = [];

for (let i = 0; i < 3; i++) {
  funcs[i] = function () { return i; };
}

for (let i = 0; i < funcs.length; i++) {
  console.log(funcs[i]()); // 0 1 2
}
```
var는 전역이니까 let으로 변경 !!!

>let 키워드의 동작 방식: let은 블록 스코프(block scope) 변수를 선언하는 데 사용됨. 이는 let으로 선언된 변수가 오직 그 변수가 선언된 블록({} 안) 내에서만 유효하다는 것을 의미. for 루프의 각 반복에서 i에 대한 새로운 스코프가 생성되며, 각 함수는 자신이 생성될 때의 i의 값에 접근할 수 있다.


1. for 루프는 0부터 시작해 3보다 작을 때까지 도는 것. 각 반복마다 i는 0, 1, 2의 값을 갖게 되고, let 키워드 덕분에 각 i 값에 대해 새로운 스코프가 생성된다.
2. 루프 내부에서, 익명 함수(function () { return i; })가 funcs 배열의 각 요소로 할당되어  이 함수는 각자의 스코프에 있는 i 값을 "기억하는" 클로저가 된다.
3. 루프가 종료된 후, funcs 배열에 저장된 각 함수를 호출하면, 각 함수는 자신이 생성될 때 접근했던 i의 값을 반환한다. 따라서, 첫 번째 함수는 0을, 두 번째 함수는 1을, 세 번째 함수는 2를 반환한다.

클로저와 let의 블록 스코프 특성을 이용하여, 각 함수가 루프의 각 반복 단계에서의 i 값을 정확히 "기억하고" 있게 하는 방법.

#### JavaScript의 실행 컨텍스트와 렉시컬 환경

**실행 컨텍스트(execution context)**: 코드가 실행되는 환경. 변수, 함수 선언 등의 정보가 포함. 

**렉시컬 환경(Lexical Environment)** : 실행 컨텍스트에 포함된 개념, 식별자(변수 이름, 함수 이름 등)와 그에 대응하는 값, 그리고 외부 렉시컬 환경에 대한 참조를 저장.

##### let과 렉시컬 환경
let 키워드로 선언한 변수는 블록 스코프를 가진다. 이는 해당 변수가 선언된 블록(예: {}) 내에서만 접근 가능하다는 것을 의미.

#### for 문과 렉시컬 환경의 동작

- 새로운 렉시컬 환경의 생성: for 문이 시작될 때, let으로 선언된 초기화 변수를 위한 새로운 렉시컬 환경이 만들어진다. 
  이 환경은 초기화 변수의 식별자와 값을 포함. 이렇게 생성된 렉시컬 환경은 현재 실행 중인 실행 컨텍스트의 렉시컬 환경으로 교체된다.

- 반복마다의 새로운 렉시컬 환경: for 문의 코드 블록이 반복 실행될 때마다, 즉 각 반복(iteration)마다 새로운 렉시컬 환경이 생성된다. 이 환경은 for 문의 코드 블록 내의 식별자와 값을 포함하고, 증감문 반영 이전의 상태를 담고 있다. 새롭게 생성된 렉시컬 환경은 현재 실행 중인 실행 컨텍스트의 렉시컬 환경으로 다시 교체된다.
- 원래의 렉시컬 환경으로의 복귀: for 문의 실행이 모두 종료되면, for 문 실행 이전의 원래 렉시컬 환경으로 실행 컨텍스트의 렉시컬 환경이 되돌려진다.
  
이 과정을 통해 let으로 선언된 변수는 각 반복마다 독립적인 스코프를 가지게 되며, 이는 클로저나 다른 스코프와 관련된 동작을 이해하는 데 중요한 기초가 된다. 이러한 메커니즘은 JavaScript가 변수의 스코프와 생명 주기를 어떻게 관리하는지, 그리고 코드의 다양한 부분에서 변수에 어떻게 접근할 수 있는지를 정확히 이해하는 데 도움을 준다.


#### 반복문과 렉시컬 환경

let이나 const로 선언된 변수는 블록 스코프를 가짐. 이는 해당 변수가 선언된 블록(예: 반복문의 {}) 내에서만 접근 가능하다는 것을 의미. 
반복문(for, for...in, for...of, while 등)이 실행될 때마다, 이러한 변수를 포함하는 새로운 렉시컬 환경이 만들어진다. 이는 마치 반복할 때마다의 상태를 '스냅샷'처럼 저장하는 것과 유사하다.

- 함수와 스코프
반복문 내부에서 함수가 정의되는 경우, 이 함수는 해당 반복 단계에서 생성된 렉시컬 환경을 '캡처'할 수 있다. 이는 클로저(Closure)라는 개념과 보자면 각 반복 단계에서의 변수 상태를 함수가 기억하게 만들 수 있다. 이는 반복문 내부에서 이벤트 리스너를 등록하거나, 비동기 작업을 수행할 때 유용하게 사용된다.

- 가비지 컬렉션과 렉시컬 환경
반복문의 코드 블록 내에서 함수를 정의하지 않는 경우, 각 반복 단계에서 생성된 렉시컬 환경은 특별히 '캡처'되어 사용되지 않기 때문에, 반복이 끝나고 나면 가비지 컬렉션의 대상이 되는 것. 즉, 더 이상 필요 없어진 메모리는 자동으로 정리해버림.

- 함수형 프로그래밍과 고차 함수
함수형 프로그래밍은 '함수'를 일급 객체로 취급하며, 데이터의 불변성과 순수 함수를 중시하는 프로그래밍 패러다임. 고차 함수(Higher-order function)는 다른 함수를 인자로 받거나, 함수를 결과로 반환하는 함수를 말한다. JavaScript에서는 배열의 .map(), .filter(), .reduce() 같은 메소드들이 이에 해당. 이러한 고차 함수를 사용하면, 명령형 프로그래밍(예: 명시적인 반복문 사용) 대신 선언적 프로그래밍을 할 수 있게 되어, 코드의 오류 가능성을 줄이고 가독성을 높일 수 있다.




## 📌중요한 점

뭔가를 기억할거야 ? 그리고 그거를 함부로 못 바꾸게 할거야 ? 클로저써라 그러면

## 🤔궁금한 점