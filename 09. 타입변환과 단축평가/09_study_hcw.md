# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

## 9.1 타입 변환이란?

- 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것
- 명시적 타입 변환(타입 캐스팅) : `개발자가 의도적으로 값을 변환`하는 것
- 암묵적 타입 변환(타입 강제 변환) : 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 `자바스크립트 엔진에 의해 암묵적으로 타입이 자동으로 변환`되는 것

### 09-01

```javascript
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

### 09-02

```javascript
var x = 10;

// 암묵적 타입 변환
// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + "";
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

## 9.2 암묵적 타입 변환

- 표현식이 코드의 문맥에 부합하지 않는 경우 `자바스크립트는 에러를 발생시키기 보다 문맥을 고려해 암묵적으로 타입을 변환`해 표현식을 평가한다.
- 암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 값은 원시 타입 중 하나로 타입을 자동 변환한다.

### 09-03

```javascript
// 피연산자가 모두 문자열 타입이어야 하는 문맥
"10" + 2; // -> '102'

// 피연산자가 모두 숫자 타입이어야 하는 문맥
5 * "10"; // -> 50

// 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
!0; // -> true
if (1) {
}
```

### 9.2.3 불리언 타입으로 변환

- 자바스크립트 엔진은 불리언 타입이 아닌 값을 `Truthy 값(참으로 평가되는 값)` 또는 `Falsy 값(거짓으로 평가되는 값)` 으로 구분한다.
- 즉, if 문이나 for 문과 같은 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy 값은 true 로 Falsy 값은 false로 암묵적 타입 변환이 된다.

<br>

**false로 평가되는 Falsy 값**

```javascript
false, undefined, null, 0, -0, NaN, ''(빈 문자열)
```

### 09-13

```javascript
// 전달받은 인수가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

// 전달받은 인수가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

// 모두 true를 반환한다.
isFalsy(false);
isFalsy(undefined);
isFalsy(null);
isFalsy(0);
isFalsy(NaN);
isFalsy("");

// 모두 true를 반환한다.
isTruthy(true);
isTruthy("0"); // 빈 문자열이 아닌 문자열은 Truthy 값이다.
isTruthy({});
isTruthy([]);
```

## 9.3 명시적 타입 변환

- 명시적으로 타입을 변경하는 방법 => 표준 빌트인 생성자 함수를 new 연산자 없이 호출하기, 빌트인 메서드 사용, 암묵적 타입 변환

## 9.4 단축평가

논리합(||) , 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다. 논리합(||) , 논리곱(&&) 연산자 표현식은 2개의 피연산자 중 어느 한쪽으로 평가된다.

### 09-17

```javascript
"Cat" && "Dog"; // -> "Dog"
```

- 첫 번째 피연산자 'Cat'은 Truthy 값이다. 그러나 두 번째 피연산자인 'Dog' 도 확인해봐야함.
- 'Dog' 도 Truthy 값. 이때 논리곱 연산자는 논리 연산의 결과를 결정한 두 번째 피연산자인 'Dog'를 반환한다.

### 09-18

```javascript
"Cat" || "Dog"; // -> "Cat"
```

<br>

> 논리합(||) , 논리곱(&&) 연산자는 이처럼 논리 연산의 결과를 결정하는 피연산자를 그대로 반환함 => `단축평가`
>
> 단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.

<br>

<table>
 <tr>
    <td>단축 평가 표현식</td>
    <td>평가 결과</td>
    </tr>
  <tr>
    <td>true || anything</td>
    <td>true</td>
   </tr>
  <tr>
  <td>false || anything</td>
    <td>anything</td>
   </tr>
  <tr>
  <td>true && anything</td>
    <td>anything</td>
     </tr>
     <tr>
  <td>false && anything</td>
    <td>false</td>
     </tr>
</table>

<br>

# 📗배운 점

## 단축 평가가 유용하게 사용되는 경우

### 1. 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

- 변수의 값이 객체가 아니라 null 또는 undefined인 경우 객체의 프로퍼티를 참조하면 타입 에러 발생

### 09-23

```javascript
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null
```

**🙋‍♀️ 이때 단축 평가를 사용하자!**

### 09-24

```javascript
var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // -> null
```

### 2. 함수 매개변수에 기본 값을 설정할 때

- 함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당됨.
- 이때 단축 평가를 사용해 매개변수의 기본 값을 설정하면 undefined로 인한 혹시 모를 에러를 방지할 수 있다.

### 09-25

```javascript
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || "";
  return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = "") {
  return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2
```
