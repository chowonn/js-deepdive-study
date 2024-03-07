# 📗챕터 정리

## ❔ this
**this**는 자신이 속한 객체 혹은 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**로 사용된다.
this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 내 어디서든 참조할 수 있다.
일반적으로 this는 자신이 속한 객체 혹은 생성할 인스턴스의 프로퍼티나 메서드를 참조할 때 사용한다.


### 그럼 this는 언제 필요한가?
객체는 프로퍼티(상태)와 메서드(동작)를 하나의 논리적인 단위로 묶은 복합적 자료구조이다.
메서드(동작)은 자신이 속한 객체의 프로퍼티(상태)를 참조하고 변경할 수 있어야 한다.
객체 리터럴 방식으로 생성한 객체는 메서드가 자신이 속한 객체를 재귀적으로 참조할 수 있다.
하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 것은 바람직하지 않은 방법이다.

```jsx
const circle = {
  radius: 5,
  getDiameter() {
    return 2 * circle.radius;
  }
};

console.log(circle.getDiameter()); // 10
```

생성자 함수로 객체를 생성할 때, 생성자 함수를 정의하는 시점에서 객체에 프로퍼티(메서드)를 추가하기 위해 생성할 인스턴스를 참조할 수 있어야 한다.
하지만 재귀를 사용해서는 생성할 인스턴스를 가리키는 식별자를 알 방법이 없다.
이러한 경우에는 자신이 속한 객체, 생성할 인스턴스를 가리키는 식별자가 필요한데 이 식별자가 바로 **this**이다.

---

## this가 가리키는 값

this가 가리키는 값, 즉 this가 참조한 값을 흔히 **“this 바인딩”**이라고 한다.
이 값은 함수 호출 방식에 의해 동적으로 결정되는데, 상황에 따라 가리키는 대상이 달라진다.

### 1️⃣ **전역 및 일반 함수에서 호출된 `this` : 전역 객체(`window`, `global`)를 참조**

~~사용할 일은 없다.~~

```jsx
console.log(this); // window

function square(number) {
  console.log(this); // window
  return number * number;
}
```

어떠한 함수라도 일반 함수로 호출된 모든 함수 내부의 this에는 전역 객체가 바인딩된다.

- 메서드 내에서 정의한 중첩 함수로 호출된 일반 함수
    
    ```jsx
    const obj = {
      value: 100,
      foo() {
        console.log("foo's this: ", this);  // {value: 100, foo: ƒ}
        console.log("foo's this.value: ", this.value); // 100
    
        // 메서드 내에서 정의한 중첩 함수
        function bar() {
          console.log("bar's this: ", this); // window
          console.log("bar's this.value: ", this.value); // 1
        }
    
        bar();
      }
    };
    
    obj.foo();
    ```
    
- 콜백 함수(`setTimeout`, ...)로 호출된 일반 함수
    
    ```jsx
    let value = 1;
    
    const obj = {
      value: 100,
      foo() {
        console.log("foo's this: ", this); // {value: 100, foo: ƒ}
        // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
        setTimeout(function () {
          console.log("callback's this: ", this); // window
          console.log("callback's this.value: ", this.value); // 1
        }, 100);
      }
    };
    
    obj.foo();
    ```
    

메서드 내 중첩 함수와 콜백 함수는 일반적으로 외부 함수의 헬퍼 함수 역할을 한다.
헬퍼 함수는 외부 함수의 로직을 대신하는 경우가 대부분인데, this가 전역 객체를 바인딩 하는 것은 헬퍼 함수로서의 동작을 어렵게 한다는 문제가 있다.
이 때는 중첩 함수 및 콜백 함수 내부의 this 바인딩을 메서드 내부 this 바인딩과 일치시켜야 한다.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    **// this 바인딩(obj)을 변수 that에 할당한다.
    const that = this;**

    // 콜백 함수 내부에서 this 대신 that을 참조한다.
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);
  }
};

obj.foo();
```

위 방법 외에도 this를 명시적으로 바인딩할 수 있는 메서드들이 존재한다.

- `Function.prototype.apply`
- `Function.prototype.call`
- `Function.prototype.bind`

이 메서드들은 페이지의 아래에 설명하도록 하겠다.
화살표 함수를 사용해서 함수 내부 this 바인딩을 일치시키는 방법도 존재한다.
이는 화살표 함수 챕터에서 내용을 확인할 수 있다. (링크 연결 필요)

### 2️⃣ **메서드에서 호출된 `this` : 메서드를 호출한 객체를 참조**

```jsx
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
}
```

메서드 내부의 this는 메서드를 가지고 있는 객체와 상관 없이, 메서드를 호출한 객체에 바인딩된다.

```jsx
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person('Lee');

Person.prototype.name = 'Kim';

// ① getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // Kim

// ② getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // Lee
```

### 3️⃣ **생성자 함수에서 호출된 `this` : 생성자 함수가 생성할 인스턴스 객체를 참조**

```jsx
function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}
```

this는 객체의 프로퍼티 혹은 메서드를 참조하기 위한 자기 참조 변수이다.
따라서 this는 2️⃣ 객체의 메서드 호출됐을 때, 3️⃣ 생성자 함수에서 호출됐을 때만 의미가 있음을 알아두자.

---

## `Function.prototype.apply/call/bind` 메서드에 의한 this 간접 호출
이 메서드들은 Function 빌트인 함수의 메서드이기 때문에 모든 함수가 상속 받아 사용할 수 있다.

### 1️⃣ `apply` / `call` 메서드
`apply`, `call` 메서드는 this로 사용할 객체 + 실행할 함수가 전달받을 인수를 메서드의 인수로 전달한다.
첫 번째 인수로 전달한 특정 객체는 실행할 함수의 this에 바인딩한다.
나머지 인수들은 실행할 함수의 인수로 전달한 후, this 바인딩이 변경된 함수를 실행시킨다.

```jsx
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// thisArg 객체가 getThisBinding 함수 내부 this에 바인딩된다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

`apply`, `call` 두 메서드는 동일하게 동작하지만, 전달 받는 인수의 형태에 차이가 있다.

- `apply` 메서드는 호출할 함수의 인수를 배열로 묶어서 전달한다. (인수의 개수는 총 2개이다.)
- `call` 메서드는 호출할 함수의 인수를 쉼표로 나열하여 전달한다. (인수가 여러개 전달될 수 있다.)
    
    ```jsx
    // apply
    getThisBinding.apply(thisArg, [1, 2, 3])
    
    // call
    getThisBinding.call(thisArg, 1, 2, 3);
    ```
    

함수의 객체의 프로퍼티인 `arguments` 객체 등 유사 배열 객체에 배열 메서드를 사용하기 위해 흔히 사용된다.
유사 배열 객체는 배열과 형태가 유사하지만 배열은 아니기 때문에 배열 메서드를 사용할 수 없다.
이 때 `apply`, `call` 메서드를 사용하면 배열 메서드를 유사 배열 객체에 사용할 수 있다.

```jsx
function convertArgsToArray() {
	// Array.prototype.slice 메서드를 사용하여 배열 메서드를 사용하여 복사본 생성
  return Array.prototype.slice.call(arguments);
}

const argsToArr = convertArgsToArray(1, 2, 3);

console.log(argsToArr); // [1, 2, 3]
```

### 2️⃣ `bind` 메서드

`bind` 메서드는 메서드 첫 번째 인수에 전달한 객체로 this 바인딩이 변경된 함수를 반환한다.
이 때, `apply`, `call` 메서드와 달리 함수를 실행하지는 않는다.

```jsx
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// thisArg로 this 바인딩이 교체된 getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding

// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

`bind` 메서드는 메서드의 this와 메서드 내부 중첩/콜백 함수의 this를 일치시켜야 할 때 유용하게 사용된다.
메서드 내부 중첩/콜백 함수의 this에는 전역 객체가 바인딩된다.

```jsx
const person = {
  name: 'Lee',
  foo(callback) {
    setTimeout(callback, 100);
  }
};

person.foo(function () { // 콜백 함수로 전달받은 일반 함수의 this는 전역 객체이다.
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is .
});
```

위 코드에서 `person.foo`를 실행시켰을 때, 콜백 함수인 `foo`의 this에는 전역 객체가 바인딩된다.
때문에 `this.name`은 `window.name`을 참조하게 되어 사용자가 의도한 값이 출력되지 않는다.

```jsx
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달하였다.
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```

콜백 함수로 전달 받은 함수의 this를 일치시키기 위해 `bind` 메서드를 사용하여 this 바인딩을 변경하였다.
this가 사용자의 의도대로 인스턴스 객체를 바인딩하여 `person.foo` 메서드의 결과가 정상적으로 출력된다.

<br />

## 🤔궁금한 점

## 📌중요한 점