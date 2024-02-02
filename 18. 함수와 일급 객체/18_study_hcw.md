# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

---

<br>

# 📌중요한 점

## 일급객체 조건

- 무명의 리터럴로 생성할 수 있다 => 런타임에 생성 가능
- 변수나 객체, 배열에 저장이 가능하다
- 함수의 매개변수로 전달할 수 있다
- 함수의 반환값으로 사용 가능하다

=> `자바스크립트에서 함수는 일급 객체다.`

#### 18-01

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

> 함수와 객체의 차이점은?

=> 일반 객체는 호출할 수 없지만 함수 객체는 호출이 가능하다. 그리고 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다

<br>

# 📗배운 점

## arguments 객체

- `유사 배열 객체` ( length프로퍼티를 가진 객체로 for문으로 순회할 수 있는 객체 ) 이면서 `이터러블`
- 유사 배열 객체는 배열이 아니므로 배열 메서드를 사용하면 에러가 발생함

<br>

## arguments 객체를 사용하는 경우는?

매개변수 개수를 확정할 수 없는 `가변 인자 함수`를 구현할 때 유용하다

<br>

## name 프로퍼티

- name 프로퍼티는 함수 이름을 나타낸다
- 해당 프로퍼티는 ES5와 ES6에서 각각 다르게 동작한다
- `익명 함수 표현식의 경우 ES5에서 name 프로퍼티는 빈 문자열을 값으로 가지고, ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다`
  <br>
