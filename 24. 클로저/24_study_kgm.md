# 📗챕터 정리

## 렉시컬 스코프

```jsx
const x = 1;

function outerFunc() {
  const x = 10;

  function innerFunc() {
    console.log(x);
  }

  innerFunc();
}

outerFunc(); // 10
```

전역에 `outerFunc` 함수, 그리고 그 내부에 중첩 함수인 `innerFunc` 함수가 있다.

`innerFunc` 중첩 함수의 상위 스코프는 `outerFunc` 함수의 `x` 변수에 접근할 수 있다.

이는 `innerFunc` 함수의 렉시컬 환경이 외부 렉시컬 환경에 대한 참조로 `outerFunc` 함수 스코프를 가리키기 때문이다.

`innerFunc` 함수가 `outerFunc` 함수의 내부에서 정의된 중첩 함수가 아니라면 어떨까?

```jsx
const x = 1;

function innerFunc() {
  console.log(x);
}

innerFunc(); // 1

function outerFunc() {
  const x = 10;

  innerFunc();
}

outerFunc(); // 1
```

`innerFunc` 함수를 `outerFunc` 함수의 내부에서 호출한다 하더라도 `outerFunc` 함수에 존재하는 변수에는 접근할 수 없다.

이런 현상이 발생하는 이유는 자바스크립트가 **렉시컬 스코프**를 따르는 프로그래밍 언어이기 때문이다.

자바스크립트 엔진은 함수를 어디서 호출했는지가 아닌 어디에 정의했는가에 따라 상위 스코프를 결정한다.

함수를 어디에 선언했는가에 따라 상위 스코프가 결정되는 것을 **렉시컬 스코프**라고 한다.

### 함수 객체의 [[Environment]] 내부 슬롯

함수 평가 후 함수 객체를 생성할 때, 생성될 객체 내에 상위 스코프가 저장될 [[Environment]] 슬롯을 생성한다.

생성된 [[Environment]] 슬롯에는 현재 실행 중인 실행 컨텍스트를 저장한다.

현재 실행 중인 실행 컨텍스트를 저장하는 이유는 코드 실행부가 아직 함수 객체를 정의할 환경, 즉 외부(상위) 함수에 위치하기 때문이다.

함수 객체 생성이 완료되고 나서야 코드 실행부는 함수 스코프 내부로 이동할 것이다.

생성될 함수 객체에게는 자신의 [[Environment]] 슬롯에 저장된 스코프가 자신의 상위 객체가 된다.

“외부 렉시컬 환경에 대한 참조”를 호출할 때 [[Environment]] 슬롯을 통해 상위 스코프를 참조하게 된다.

클로저를 알기 위해서는 🔗[렉시컬 스코프](https://www.notion.so/816ca51e4bba4a14873ed7ac61f051c1?pvs=21) 개념을 먼저 짚고 가야 한다.

## 클로저 (Closure)

> *"클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다"*
> 

외부 함수보다 내부에 있는 중첩 함수가 더 오래 유지되는 경우, 외부 함수의 생명 주기가 종료되었더라도 변수를 참조할 수 있다.

이러한 중첩 함수를 **클로저**라고 한다.

그런데 클로저의 정의를 알더라도 이게 무슨 뜻인지는 잘 와닿지는 않는다.

예제 코드로 클로저를 확인해보자.

```jsx
const outer = () => {
	let x = 2;
	
	// 클로저
	const inner = (y) => {
		x = x + y;
		console.log(x);
	}
	
	return inner;
}

const addFunc = outer();

addFunc(3); // 5
addFunc(5);	// 10
```

`addFunc` 변수로 `outer` 함수를 실행하면 `outer` 함수는 `inner` 함수를 반환하고 생명 주기를 마감한다.

`outer` 함수의 실행이 종료될 때, `outer` 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거된다.

원래 우리가 배웠던 내용으로는 `outer` 함수의 지역 변수인 `x`는 더 이상 접근할 수 없을 것처럼 보인다.

하지만 `addFunc`로 함수를 실행해보면 함수가 `x` 변수에 접근할 수 있는 것처럼 동작한다.

왜 이렇게 동작할까?

그것을 알기 위해서는 함수가 실행됐을 때 동작하는 과정을 하나하나 뜯어봐야 한다.

1️⃣ 코드가 실행되면 정의된 `outer` 함수를 평가하며 함수 객체를 생성할 것이다.

이 때, 현재 실행 중인 실행 컨텍스트의 렉시컬 환경인 전역 렉시컬 환경을 `outer` 함수 객체의 [[Environment]] 슬롯에 저장할 것이다.

2️⃣ `outer` 함수를 호출하면 `outer` 함수 실행 컨텍스트와 렉시컬 환경이 생성될 것이다.

`outer` 함수의 [[Environment]] 슬롯에 저장된 전역 렉시컬 환경을 `outer` 함수 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 할당한다.

3️⃣ 중첩 함수인 `inner` 함수가 평가된다.

이후 현재 실행 중인 실행 컨텍스트의 렉시컬 환경인 `outer` 함수 렉시컬 환경을 `inner` 함수 객체의 [[Environment]] 슬롯에 저장한다.

이 위까지는 우리도 충분히 이해하고 있다. 문제는 이 다음 단계인데…

4️⃣ `outer` 함수 실행이 종료되면 inner 함수를 반환하면서 생명 주기가 종료된다.

즉, `outer` 함수의 실행 컨텍스트가 실행 컨텍스트 스택에서 제거된다.

**하지만 여기서 `outer` 함수의 렉시컬 환경까지 소멸되지는 않는다!**

`outer` 함수의 렉시컬 환경은 왜 소멸되지 않을까?

`outer` 함수의 렉시컬 환경은 `inner` 함수의 [[Environment]] 슬롯에서 참조하고 있다.

그리고 `inner` 함수는 전역 변수 `addFunc` 에서 참조하고 있기 때문에 가비지 컬렉터의 청소 대상이 아니다.

가비지 컬렉터는 누군가가 참조하고 있는 메모리 공간을 함부로 청소하지 않는다.

(왜 가비지 컬렉터의 청소 대상이 아닌가? 에 대한 내용은 🔗[가비지 컬렉터](https://www.notion.so/Garbage-Collector-d829d1ed68e84dcd8a2dfd8b5df947fa?pvs=21) 참고)

5️⃣ `outer` 함수가 반환한 `inner` 함수를 호출하면 `inner` 함수의 실행 컨텍스트가 생성된다.

생성된 `inner` 함수의 실행 컨텍스트는 실행 컨텍스트 스택에 추가 된다.

실행 컨텍스트와 같이 생성된 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에는 inner 함수 객체의 [[Environment]] 슬롯에 저장되어 있는 참조값인 `outer` 함수 렉시컬 환경이 할당될 것이다.

중첩 함수인 `inner` 함수는 외부 함수 `outer` 함수보다 더 오래 생존했다.

외부 함수의 생존 여부와 상관 없이, **외부 함수보다 더 오래 생존한 중첩 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억한다.**

중첩 함수 inner 함수의 내부에서는 상위 스코프를 참조할 수 있으므로 상위 스코프의 식별자를 참조할 수 있거나 값을 변경할 수도 있다.

---

## 클로저의 활용

**클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.**

즉, **상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용**하기 위해 사용한다.

함수가 호출될 때마다 호출된 횟수를 누적하여 출력하는 카운터를 만들어보자.

예제의 호출된 횟수(num)가 바로 안전하게 변경하고 유지해야 할 상태다.

```jsx
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태를 1만큼 증가시킨다.
  return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

위 예제가 에러 없이 바르게 동작하기 위해선 다음과 같은 조건이 필요하다.

1. `num` 변수가 increase 함수 호출 전까지 변경되지 않고 유지되어야 한다.
2. 이를 위해 `num`은 increase 함수만이 변경할 수 있어야 한다.

그럼 상태 변경을 방지해보자.

`num`을 지역 변수로 설정하여 의도치 않은 상태 변경을 방지하고, 클로저를 통해 상태가 변경되어도 이전 상태를 유지할 수 있도록 한다.

```jsx
const increase = (function () {
  let num = 0;

  // 클로저
  return function () {
    return ++num;
  };
})();

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

위 코드 실행 시, 즉시 실행 함수가 호출되고 즉시 실행 함수가 반환한 함수가 `increase` 변수에 할당된다.

`increase` 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저다.

따라서 즉시 실행 함수가 반환한 클로저는 카운트 상태를 유지하기 위한 자유 변수 `num`을 언제 어디서 호출하든지 참조하고 변경할 수 있다.

즉시 실행 함수는 한 번만 실행되므로 increase가 호출될 때마다 num 변수가 재차 초기화될 일은 없을 것이다. 또한 외부에서 접근 불가한 private 변수이므로 의도치 않은 변경을 걱정할 필요가 없다.

**이처럼 클로저는 상태state 가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.**

카운트 상태를 감소시킬 수 있도록 발전시켜보자.

```jsx
const counter = (function(){
  let num = 0;

  // 클로저인 메서드를 갖는 객체를 반환한다.
  return{
    increase(){
      return ++num;
    };
    decrease(){
      return num > 0 ? --num : 0;
    };
  }
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

위 예제의 increase, decrease 메서드의 상위 스코프는 해당 메서드가 평가되는 시점에 실행 중인 실행 컨텍스트인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경이다.

따라서 increase, decrease 메서드가 언제 어디서 호출되든 상관없이 increase, decrease 함수는 즉시 실행 함수의 스코프 식별자를 참조 가능하다.

위 예제를 생성자 함수로 표현하면 다음과 같다.

```jsx
const Counter = (function () {
  // 1) 카운트 상태 변수
  let num = 0;

  function Counter() {
    // this.num = 0; // 2) 프로퍼티는 public하므로 은닉되지 않는다.
  }

  Counter.prototype.increase = function () {
    return ++num;
  };

  Counter.prototype.decrease = function () {
    return num > 0 ? --num : 0;
  };

  return Counter;
})();
```

위 예제의 num(①)은 생성자 함수 Counter가 생성할 인스턴스의 프로퍼티가 아니라 즉시 실행 함수 내에서 선언된 변수다.

만약 num이 생성자 함수 Counter가 생성할 인스턴스의 프로퍼티라면(②) 인스턴스를 통해 외부에서 접근이 자유로운 public 프로퍼티가 된다.

즉시 실행 함수 내에서 선언된 num 변수는 은닉된 변수이다.

생성자 함수 Counter는 프로토타입을 통해 increase, decrease 메서드를 상속받는 인스턴스를 생성한다.

해당 메서드는 모두 자신의 함수 정의가 평가되어 함수 객체가 될 때 실행 중인 실행 컨텍스트인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경을 기억하는 클로저다.

따라서 프로토타입을 통해 상속되는 프로토타입 메서드일지라도 즉시 실행 함수의 자유 변수 `num`을 참조할 수 있다.

다시 말해, num 변수의 값은 increase, decrease 메서드만이 변경할 수 있다.

---

## 캡슐화와 정보 은닉

캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.

캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라 한다.

대부분의 객체지향 프로그래밍 언어는 클래스를 정의하고 그 클래스를 구성하는 멤버(프로퍼티와 메서드)에 대하여 `public`, `private`, `protected` 같은 접근 제한자를 선언하여 공개 범위를 한정할 수 있다.

자바스크립트는 접근 제한자를 제공하지 않는다. 따라서 모든 프로퍼티와 메서드는 기본적으로 외부에 공개되어 있다.

다음 예제를 살펴보자.

```jsx
const Person = (function () {
  let _age = 0;

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
})();

const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined
```

name 프로퍼티는 현재 외부로 공개되어 있어 자유롭게 참조 및 변경 가능하다. 즉 public 하다.

`_age` 변수는 즉시 실행 함수의 지역 변수로 설정하여 private 하도록 설정하였다. 따라서 외부에서 참조하거나 변경할 수 없다.

즉시 실행 함수가 반환하는 `Person` 생성자 함수와 Person 생성자 함수의 인스턴스가 상속받아 호출할 `Person.prototype.sayHi` 메서드는 즉시 실행 함수가 종료된 이후 호출된다.

하지만 Person 생성자 함수와 `sayHi` 메서드는 이미 종료되어 소멸한 즉시 실행 함수의 지역 변수 `_age`를 참조할 수 있는 클로저다.

하지만 위 코드 또한 문제가 있다.

Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 다음과 같이 _age 변수의 상태가 유지되지 않는다는 것이다.

```jsx
const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person("Kim", 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.
```

이는 `Person.prototype.sayHi` 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상이다.

즉 즉시 실행 함수가 호출될 때 생성된다.

이때 `Person.prototype.sayHi` 메서드는 자신의 상위 스코프인 즉시 실행 함수의 실행 컨텍스트의 렉시컬 환경의 참조를 [[Environment]]에 저장하여 기억한다.

따라서 Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 Person.prototype.sayHi 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 동일한 상위 스코프를 사용하게 된다.

이러한 이유로 여러 인스턴스 생성 시 `_age` 변수 상태가 유지되지 않는다.

---

## 발생할 수 있는 실수

```jsx
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i; // 1)
  };
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // 2)
}
```

for 문의 변수 선언문에서 `var` 키워드로 선언한 i 변수는 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수이다.

따라서 funcs 배열의 요소로 추가한 함수를 호출(②)하면 전역 변수 i를 참조하여 i의 값 3이 출력된다.

올바르게 동작하는 코드로 만들어 보자.

```jsx
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = (function (id) {
    return function () {
      return id;
    };
  })(i);
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
```

이때 즉시 실행 함수의 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수의 상위 스코프에 존재한다.

즉시 실행 함수가 반환한 중첩 함수는 자신의 상위 스코프를 기억하는 클로저고, 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수에 묶여있는 자유 변수가 되어 그 값이 유지된다.

또 다른 방법으로는 `var` 대신 `let` 키워드를 사용하거나, 함수형 프로그래밍 기법인 고차 함수를 사용하는 방법 등이 있다.

<br/>

## 🤔궁금한 점

## 📌중요한 점
