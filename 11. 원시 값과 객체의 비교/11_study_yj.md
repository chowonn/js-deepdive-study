## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 11.1 원시 값

### 11.1.1 변경 불가능한 값

원시 타입의 값, 즉 원시 값은 변경 불가능한 값

변수와 값

- 변수는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름
- 값은 변수에 저장된 데이터로서 표현식이 평가되어 생성된 결과
- 변경 불가능하다는 것은 변수가 아니라 값에 대한 진술
- 변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있음

상수 : 변수의 상대 개념으로 재할당이 금지된 변수

- 상수는 단 한 번만 할당이 허용되므로 변수 값을 변경할 수 없음
- 상수와 변경 불가능한 값을 동일시 하면 안됨

```javascript
// const 키워드를 사용해 선언한 변수는 재할당이 금지된다. 상수는 재할당이 금지된 변수일 뿐이다.
const o = {};

// const 키워드를 사용해 선언한 변수에 할당한 원시값(상수)은 변경할 수 없다.
// 하지만 const 키워드를 사용해 선언한 변수에 할당한 객체는 변경할 수 있다.
o.a = 1;
console.log(o); // {a: 1}
```

원시 값은 읽기 전용 값이며 이러한 원시 값의 특성은 데이터의 신뢰성을 보장

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/1827e3fd-3f34-4566-8d49-68372e762783)

- 변수가 참조하던 메모리 공간의 주소가 변경된 이유는 변수에 할당된 원시 값이 변경 불가능한 값이기 때문
- 만약 원시 값이 변경 가능한 값이라면 변수에 새로운 원시 값을 재할당했을 때 변수가 가리키던 메모리 공간의 주소를 바꿀 필요 없이 원시 값 자체를 변경하면 됨
- 그렇다면 변수가 참조하던 메모리 공간의 주소는 바뀌지 않음

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/0764e312-14f8-40e2-8c91-a8564670b080)

- 원시 값은 변경 불가능한 값이기 때문에 값을 직접 변경할 수 없음
- 변수 값을 변경하기 위해 원시 값을 재할당하면 새로운 메모리 공간을 확보하고 재할당한 값을 저장한 후, 변수가 참조하던 메모리 공간의 주소를 변경
- 이러한 특성을 **불변성**이라 함

불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없음

### 11.1.2 문자열과 불변성

자바스크립트의 문자열은 원시타입이며 변경 불가능함

```javascript
var str = "Hello";
str = "world";
```

- 첫 번째 문이 실행되면 문자열 'Hello'가 생성되고 식별자 str은 문자열 'Hello'가 저장된 메모리 공간의 첫 번째 메모리 셀 주소를 가리킴
- 두 번째 문이 실행되면 이전에 생성된 문자열 'Hello'를 수정하는 것이 아니라 새로운 문자열 'world'를 메모리에 생성하고 식별자 str은 이것을 가리킴
- 이때 문자열 'Hello'와 'world'는 모두 메모리에 존재
- 식별자 str은 문자열 'Hello'를 가리키고 있다가 문자열 'world'를 가리키도록 변경되었을 뿐

문자열은 유사 배열 객체이면서 이터러블이므로 배열과 유사하게 각 문자에 접근 가능

- 유사 배열 객체란 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체

```javascript
var str = "string";

// 문자열은 유사 배열이므로 배열과 유사하게 인덱스를 사용해 각 문자에 접근할 수 있다.
// 하지만 문자열은 원시값이므로 변경할 수 없다. 이때 에러가 발생하지 않는다.
str[0] = "S";

console.log(str); // string
```

- 문자열은 변경 불가능한 값이기 때문에 str[0] = 'S'처럼 이미 생성된 문자열의 일부 문자를 변경해도 반영되지 않음

그러나 변수에 새로운 문자열을 재할당하는 것은 가능함

### 11.1.3 값에 의한 전달

변수에 원시 값을 갖는 변수를 할당하면 할당받는 변수에는 할당되는 변수의 원시 값이 복사되어 전달되는 것

```javascript
var score = 80;

// copy 변수에는 score 변수의 값 80이 복사되어 할당된다.
var copy = score;

console.log(score, copy); // 80  80
console.log(score === copy); // true
```

- copy 변수에 원시 값을 갖는 score 변수를 할당하면 할당받는 변수(copy)에는 할당되는 변수(score)의 원시 값 80이 복사되어 전달
- 하지만 score 변수와 copy 변수의 값 80은 다른 메모리 공간에 저장된 별개의 값

```javascript
var score = 80;

// copy 변수에는 score 변수의 값 80이 복사되어 할당된다.
var copy = score;

console.log(score, copy); // 80  80
console.log(score === copy); // true

// score 변수와 copy 변수의 값은 다른 메모리 공간에 저장된 별개의 값이다.
// 따라서 score 변수의 값을 변경해도 copy 변수의 값에는 어떠한 영향도 주지 않는다.
score = 100;

console.log(score, copy); // 100  80
console.log(score === copy); // false
```

- score 변수의 값을 변경해도 copy 변수의 값에는 어떠한 영향도 주지 않음<br>
  ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/74629dfe-054a-4978-ac30-338f5c3def3e)

참고

- 엄격하게 표현하자면 변수에는 값이 전달되는 것이 아니라 메모리 주소가 전달되기 때문에 "값에 의한 전달"이란 용어가 오해를 불러올 수 있음
- 변수와 같은 식별자는 값이 아니라 메모리 주소를 기억하고 있음
- 전달된 메모리 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있음

중요한 것은 두 변수의 원시 값은 서로 다른 메모리 공간에 저장된 별개의 값이 되어 어느 한쪽에서 재할당을 통해 값을 변경하더라도 서로 간섭할 수 없다는 것

## 11.2 객체

### 11.2.1 변경 가능한 값

객체(참조) 타입의 값, 즉 객체는 변경 가능한 값

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/0665e9b9-5f38-4289-a38d-a979303b6f71)

- 원시 값을 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 원시 값에 접근할 수 있음
- 즉, 원시 값을 할당한 변수는 원시 값 자체를 값으로 가짐
- 객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리에 접근하면 참조 값에 접근할 수 있음
- 참조 값은 생성된 객체가 저장된 메모리 공간의 주소, 그 자체

```javascript
// 할당이 이뤄지는 시점에 객체 리터럴이 해석되고, 그 결과 객체가 생성된다.
var person = {
  name: "Lee",
};

// person 변수에 저장되어 있는 참조값으로 실제 객체에 접근해서 그 객체를 반환한다.
console.log(person); // {name: "Lee"}
```

- person 변수는 객체 {name: 'Lee'}를 가리키고 있음

```javascript
var person = {
  name: "Lee",
};

// 프로퍼티 값 갱신
person.name = "Kim";

// 프로퍼티 동적 생성
person.address = "Seoul";

console.log(person); // {name: "Kim", address: "Seoul"}
```

- 원시 값을 갖는 변수의 값을 변경하는 방법은 재할당 뿐
- 하지만 객체는 변경 가능한 값이기 때문에 객체를 할당한 변수는 재할당 없이 객체를 직접 변경할 수 있음
- 재할당 없이 프로퍼티를 동적으로 추가하거나 갱신, 삭제할 수 있음

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/9c99c516-8d55-4281-96a2-2db58b88f9e2)

- 메모리에 저장된 객체를 직접 수정할 수 있음
- 이때 객체를 할당한 변수에 재할당을 하지 않았으므로 객체를 할당한 변수의 참조 값은 변경되지 않음

객체는 크기가 매우 클 수도 있고, 원시 값처럼 크기가 일정하지도 않고, 프로퍼티 값이 객체일 수도 있어서 복사해서 생성할 때 메모리의 효율적 소비가 어렵고, 성능이 나빠짐

이러한 구조적 단점에 따른 부작용이 여러 개의 식별자가 하나의 객체를 공유할 수 있다는 것

얕은 복사와 깊은 복사

- 객체를 프로퍼티 값으로 갖는 객체의 경우
- 앝은 복사는 한 단계까지만 복사하는 것
- 깊은 복사는 객체에 중첩되어 있는 객체까지 모두 복사하는 것

```javascript
const o = { x: { y: 1 } };

// 얕은 복사
const c1 = { ...o }; // 35장 "스프레드 문법" 참고
console.log(c1 === o); // false
console.log(c1.x === o.x); // true

// lodash의 cloneDeep을 사용한 깊은 복사
// "npm install lodash"로 lodash를 설치한 후, Node.js 환경에서 실행
const _ = require("lodash");
// 깊은 복사
const c2 = _.cloneDeep(o);
console.log(c2 === o); // false
console.log(c2.x === o.x); // false
```

- 얕은 복사와 깊은 복사로 생성된 객체는 원본과 다른 객체
- 얕은 복사는 객체에 중첩되어 있는 객체의 경우 참조 값을 복사
- 깊은 복사는 객체에 중첩되어 있는 객체까지 모두 복사해서 원시 값처럼 완전한 복사본을 만듦

```javascript
const v = 1;

// "깊은 복사"라고 부르기도 한다.
const c1 = v;
console.log(c1 === v); // true

const o = { x: 1 };

// "얕은 복사"라고 부르기도 한다.
const c2 = o;
console.log(c2 === o); // true
```

- 원시 값을 할당한 변수를 다른 변수에 할당하는 것을 깊은 복사
- 객체에 할당한 변수를 달느 변수에 할당하는 것을 얕은 복사라고 부르는 경우도 있음

### 11.2.2 참조에 의한 전달

객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달됨

```javascript
var person = {
  name: "Lee",
};

// 참조값을 복사(얕은 복사). copy와 person은 동일한 참조값을 갖는다.
var copy = person;

// copy와 person은 동일한 객체를 참조한다.
console.log(copy === person); // true

// copy를 통해 객체를 변경한다.
copy.name = "Kim";

// person을 통해 객체를 변경한다.
person.address = "Seoul";

// copy와 person은 동일한 객체를 가리킨다.
// 따라서 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받는다.
console.log(person); // {name: "Kim", address: "Seoul"}
console.log(copy); // {name: "Kim", address: "Seoul"}
```

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/44917fa7-f520-43fd-8f62-bab98af44c46)

- 원본 person과 사본 copy는 저장된 메모리 주소는 다르지만 동일한 참조 값을 가짐
- 두 개의 식별자가 하나의 객체를 공유하는 것을 의미
- 원본 또는 사본 중 어느 한쪽에서 객체를 변경하면 서로 영향을 주고받음

"값에 의한 전달"과 "참조에 의한 전달"은 식별자가 기억하는 메모리 공간에 저장되어 있는 값을 복사해서 전달한다는 면에서 동일

식별자가 기억하는 메모리 공간, 즉 변수에 저장되어 있는 값이 원시 값이냐 참조 값이냐의 차이만 있음

따라서 자바스크립트에서는 "참조에 의한 전달"은 존재하지 않고 "값에 의한 전달"만이 존재

## 🤔궁금한 점

자바스크립트에서는 "참조에 의한 전달"은 존재하지 않고 "값에 의한 전달"만이 존재 → 왜일까?

- 자바스크립트는 변수에 저장되는 값이 원시 값이냐 참조 값이냐 차이만 있을 뿐, 메모리 공간에 저장되있는 값을 복사해서 전달한다는 면에서 동일 → 따라서 둘 다 "값에 의한 전달"로 표현할 수 있음
- 그래서 두 용어 다 사용하지 않고 "공유에 의한 전달"이라고 표현하기도 함

## 📌중요한 점
