## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 08 제어문

### 8.1 블록문

블록문 : 0개 이상의 문을 중괄호로 묶은 것

### 8.2 조건문

#### 8.2.1 if...else문

조건식의 평가 결과가 true일 경우 if 문의 코드 블록이 실행되고, false일 경우 else 문의 코드 블록이 실행

if문의 조건식은 불리언 값으로 평가

- 만약 불리언 값이 아니면 암묵적으로 불리언 값으로 강제 변환

```js
// x가 짝수이면 result 변수에 문자열 '짝수'를 할당하고, 홀수이면 문자열 '홀수'를 할당한다.
var x = 2;
var result;

if (x % 2) {
  // 2 % 2는 0이다. 이때 0은 false로 암묵적 강제 변환된다.
  result = "홀수";
} else {
  result = "짝수";
}

console.log(result); // 짝수
```

```js
var x = 2;

// 0은 false로 취급된다.
var result = x % 2 ? "홀수" : "짝수";
console.log(result); // 짝수
```

조건에 따라 단순히 값을 결정하여 변수에 할당하는 경우 if...else문보다 삼항 조건 연산자를 사용하는 편이 가독성이 좋음

#### 8.2.2 switch문

주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮김

if...else문은 논리적 참, 거짓으로 실행할 코드 블록을 결정, switch문은 다양한 상황에 따라 실행할 코드 블록을 결정

switch문의 표현식과 일치하는 case문이 없다면 실행 순서는 default문으로 이동

break 키워드로 구성된 break문은 코드 블록에서 탈출하는 역할

- break문이 없다면 case문의 표현식과 일치하지 않더라도 실행 흐름이 다음 case문으로 연이어 이동

```js
// 월을 영어로 변환한다. (11 → 'November')
var month = 11;
var monthName;

switch (month) {
  case 1:
    monthName = "January";
  case 2:
    monthName = "February";
  case 3:
    monthName = "March";
  case 4:
    monthName = "April";
  case 5:
    monthName = "May";
  case 6:
    monthName = "June";
  case 7:
    monthName = "July";
  case 8:
    monthName = "August";
  case 9:
    monthName = "September";
  case 10:
    monthName = "October";
  case 11:
    monthName = "November";
  case 12:
    monthName = "December";
  default:
    monthName = "Invalid month";
}

console.log(monthName); // Invalid month
```

default문은 switch문의 맨 마지막에 위치, default문의 실행이 종료되면 switch문을 빠져나감

- 별고의 break문이 필요 없음

```js
// 월을 영어로 변환한다. (11 → 'November')
var month = 11;
var monthName;

switch (month) {
  case 1:
    monthName = "January";
    break;
  case 2:
    monthName = "February";
    break;
  case 3:
    monthName = "March";
    break;
  case 4:
    monthName = "April";
    break;
  case 5:
    monthName = "May";
    break;
  case 6:
    monthName = "June";
    break;
  case 7:
    monthName = "July";
    break;
  case 8:
    monthName = "August";
    break;
  case 9:
    monthName = "September";
    break;
  case 10:
    monthName = "October";
    break;
  case 11:
    monthName = "November";
    break;
  case 12:
    monthName = "December";
    break;
  default:
    monthName = "Invalid month";
}

console.log(monthName); // November
```

### 8.3 반복문

반복문 : 조건식의 평가 결과가 참인 경우 코드 블록을 실행

#### 8.3.1 for문

조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행

```js
for (var i = 0; i < 2; i++) {
  console.log(i);
}
```

#### while문

주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행

- for문은 반복 횟수가 명확할 때, while문은 반복 횟수가 불명확 할 때 사용

while문은 조건문의 평가 결과가 거짓이 되면 코드 블록을 실행하지 않고 종료

조건식의 평가 결과가 언제나 참이면 무한루프

```js
// 무한루프
while (true) { ... }
```

무한루프에서 탈출하려면 코드 블록 내에 if문으로 탈출 조건을 만들고 break문으로 코드 블록 탈출

```js
var count = 0;

// 무한루프
while (true) {
  console.log(count);
  count++;
  // count가 3이면 코드 블록을 탈출한다.
  if (count === 3) break;
} // 0 1 2
```

#### 8.3.3 do...while문

코드 블록을 먼저 실행하고 조건식 평가

```js
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do {
  console.log(count);
  count++;
} while (count < 3); // 0 1 2
```

### 8.4 break문

break문 : 레이블 문, 반복문 또는 switch 문의 코드 블록을 탈출

```js
var string = "Hello World.";
var search = "l";
var index;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 문자열의 개별 문자가 'l'이면
  if (string[i] === search) {
    index = i;
    break; // 반복문을 탈출한다.
  }
}

console.log(index); // 2

// 참고로 String.prototype.indexOf 메서드를 사용해도 같은 동작을 한다.
console.log(string.indexOf(search)); // 2
```

### 8.5 continue문

continue문 : 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동

```js
var string = "Hello World.";
var search = "l";
var count = 0;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
  if (string[i] !== search) continue;
  count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
}

console.log(count); // 3

// 참고로 String.prototype.match 메서드를 사용해도 같은 동작을 한다.
const regexp = new RegExp(search, "g");
console.log(string.match(regexp).length); // 3
```

## 🤔궁금한 점

## 📌중요한 점
