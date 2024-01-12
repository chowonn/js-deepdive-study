# 📗챕터 정리

## 자바스크립트 데이터 **타입 변환**

- 자바스크립트는 선언된 변수의 데이터 타입이 변경되더라도 문제가 발생하지 않는다.
- 데이터 타입 변환은 `Number()`, `String()` 등 명시적 타입 변환 함수를 사용하는 것이 일반적이다.
- 암시적 타입 변환을 통해 필요에 따라 변수의 데이터 타입을 변경할 수도 있다.
  의도를 갖고 암시적 타입 변환을 사용한다면 이것은 명시적 타입 변환으로 생각할 수 있다.

  ```jsx
  const YEAR = 2023;

  console.log(typeof YEAR); // number

  // 명시적 타입 변환
  console.log(String(YEAR)); // '2023'

  // 암시적 타입 변환
  console.log(YEAR + ""); // '2023'
  ```

  ```jsx
  let num = "250";

  // 명시적 타입 변환
  console.log(Number(num)); // 250

  // 암시적 타입 변환
  console.log(+num); // 250
  console.log(num * 1); // 250
  console.log(num / 1); // 250
  ```

1️⃣ **문자열 타입으로 변환**

- `String()` 함수를 사용하여 명시적으로 숫자 타입 변경이 가능하다.
  ```jsx
  String(1); // "1"
  String(true); // "true"
  ```
- `+` 연산의 피연산자 중 하나가 문자열이라면 문자열 연결 연산자로 동작한다.
  문자열 연결 연산자는 피연산자를 모두 문자열로 변환한 후 연산한다.

  ```jsx
  const text = "나이: ";
  const age = 25;

  console.log(text + age); // "나이: 25"
  ```

- 심볼 타입을 제외한 모든 타입에 대해 암시적인 문자열 타입 변환이 가능하다.

  ```jsx
  // 숫자 타입
  0 + ''         // -> "0"
  -0 + ''        // -> "0"
  1 + ''         // -> "1"
  -1 + ''        // -> "-1"
  NaN + ''       // -> "NaN"
  Infinity + ''  // -> "Infinity"
  -Infinity + '' // -> "-Infinity"

  // 불리언 타입
  true + ''  // -> "true"
  false + '' // -> "false"

  // null 타입
  null + '' // -> "null"

  // undefined 타입
  undefined + '' // -> "undefined"

  // 심볼 타입
  (Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string

  // 객체 타입
  ({}) + ''           // -> "[object Object]"
  Math + ''           // -> "[object Math]"
  [] + ''             // -> ""
  [10, 20] + ''       // -> "10,20"
  (function(){}) + '' // -> "function(){}"
  Array + ''          // -> "function Array() { [native code] }"
  ```

2️⃣ **숫자 타입으로 변환**

- `Number()`, `parseInt()`, `parseFloat()` 함수를 사용하여 명시적으로 숫자 타입 변경이 가능하다.

  ```jsx
  let num = "100";

  console.log(Number(num)); // 100
  console.log(parseInt(num)); // 100
  console.log(parseFloat(num)); // 100
  ```

- 숫자로 이루어진 문자열의 덧셈 연산 시, 두 문자열이 이어 붙는다.
  숫자로 이루어진 문자열로 실제 덧셈 연산을 수행하려면 문자열 타입을 숫자 타입으로 바꿔주어야 한다.

  ```jsx
  console.log("2" + "2"); // "22"
  console.log(2 + 2); // 4

  let x = "19";
  let y = "12";

  console.log(x + y); // "1912"
  console.log(Number(x) + Number(y)); // 31
  ```

- 숫자로 이루어진 문자열끼리 덧셈을 제외한 `-`, `*`, `/`, `%` 연산 시 숫자 타입으로 변환된다.
  숫자로 이루어진 문자열이 아닌 값에 `-`, `*`, `/`, `%` 연산 시 `NaN`이 반환된다.

  ```jsx
  4 - 2; // 2
  "4" - "2"; // 2

  "a" - "2"; // NaN
  "a" - "b"; // NaN
  ```

- 문자열끼리의 마지막 연산이 덧셈이라면 문자열, 그 외에는 숫자 타입이 반환된다.
  ```jsx
  "2" + "2" + "2"; // "222"
  "2" + "2" - "2"; // 20 ("22" - "2")
  "2" - "2" - "2"; // -2 (0 - "2")
  "2" - "2" + "2"; // "02" (0 + "2")
  ```
- 문자열 타입과 마찬가지로 심볼 타입을 제외한 모든 타입에 대해 암시적인 숫자 타입 변환이 가능하다.
  ```jsx
  // 문자열 타입
  +"" + // -> 0
    "0" + // -> 0
    "1" + // -> 1
    "string" + // -> NaN
    // 불리언 타입
    true + // -> 1
    false + // -> 0
    // null 타입
    null + // -> 0
    // undefined 타입
    undefined + // -> NaN
    // 심볼 타입
    Symbol() + // -> TypeError: Cannot convert a Symbol value to a number
    // 객체 타입
    {} + // -> NaN
    [] + // -> 0
    [10, 20] + // -> NaN
    function () {}; // -> NaN
  ```
  | 전달 받은 값 | 타입 변환 후                                                                                                                                                                   |
  | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
  | undefined    | NaN                                                                                                                                                                            |
  | null         | 0                                                                                                                                                                              |
  | true / false | 1 / 0                                                                                                                                                                          |
  | string       | 문자열의 처음과 끝 공백이 제거됨 →<br/> 1. 남아있는 문자열이 없다면 0<br/> 2. 남아있는 문자열이 숫자라면 숫자 타입으로 변환하여 표시<br/> 3. 숫자 타입으로 변환에 실패하면 NaN |

3️⃣ **불리언(Boolean) 타입으로 변환**

- `Boolean()` 함수로 명시적 타입 변환이 가능하다.
  ```jsx
  Boolean(1); // true
  Boolean(0); // false
  Boolean(-1); // true
  ```
- 논리 부정 연산자(`!`)는 기본적으로 피연산자의 의미를 반대로 바꾼다.
  논리 부정 연산자(`!`)는 불리언이 아닌 다른 타입과 함께 사용하면 암시적인 불리언 타입 변환을 수행한다.

  ```jsx
  let n = 1;

  Boolean(n); // true
  !n; // false
  !!n; // true
  ```

- 숫자 타입인 `0`과 `NaN`은 `false`로 간주한다.
  ```jsx
  Boolean(0); // false
  0 == false; // true
  !0; // true
  !!0; // false
  ```
- 공백은 문자열 데이터가 있는 것으로 간주한다.
  ```jsx
  Boolean(" "); // true
  Boolean(""); // 데이터가 없으면 false
  Boolean("false"); // 불리언 타입이 아닌 문자열 타입 'false' 값은 true
  ```
- `undefined`와 `null`은 `false`로 간주한다.

  ```jsx
  Boolean(undefined); // false
  Boolean(null); // false

  !null; // true
  !undefined; // true
  ```

- 배열과 객체는 내부 데이터 유무와 상관 없이 `true`를 반환한다.

  ```jsx
  Boolean([]); // true
  Boolean({}); // true

  ![]; // false
  !{}; // false
  ```

<br/>

### 단축 평가

- **논리 연산의 평가 결과는 불리언 값이 아닐 수도 있다.**
- 논리합(OR) 연산자(`||`)는 첫번째 `true`로 평가되는 값을 추적한다.

  - 자바스크립트에서 동작하는 논리합(OR) 연산자(`||`)의 추가적인 기능이다.

    - 논리합(OR) 연산식의 첫 번째 `true`로 평가되는 값을 그대로 반환한다.
    - 모든 값이 `false`로 평가된면 마지막 값을 그대로 반환한다.

    ```jsx
    // result = value1 || value2 || value3;

    result = 0 || "a" || ""; // 'a'
    result = 0 || null || ""; // ''
    ```

- 논리곱(AND) 연산자(`&&`)는 첫번째 `false`로 평가되는 값을 추적한다.

  - 논리합(OR) 연산자(`||`)와 마찬가지로 자바스크립트의 추가적인 기능이다.

    - 논리합(OR) 연산식의 첫 번째 `false`로 평가되는 값을 그대로 반환한다.
    - 모든 값이 `true`로 평가된다면 마지막 값을 그대로 반환한다.

    ```jsx
    // result = value1 || value2 || value3;

    result = 0 && "a" && ""; // 0
    result = 1 && "a" && "name"; // 'name
    ```

- 단축 평가를 사용하여 if 조건문을 대체할 수 있다.

  ```jsx
  let done = true;
  let message = "";

  if (done) message = "완료";

  // if 문은 단축 평가로 대체 가능하다.
  message = done && "완료"; // done이 true라면 '완료'를 할당
  console.log(message); // 완료

  done = false;
  message = done || "미완료"; // done이 false라면 '미완료'를 할당
  console.log(message); // 미완료
  ```

  - 위 예제는 삼항 조건 연산자로 대체하는 것이 편리하다.
    ```jsx
    message = done ? "완료" : "미완료";
    ```

- 단축 평가를 사용하여 조건부로 함수를 실행하는 것도 가능하다.
  ```jsx
  false || console.log("printed"); // 'printed'
  true || console.log("not printed");
  ```

2. 매개변수가 있는 함수를 사용할 때 인수를 전달하지 않으면 `undefined`가 할당되는데,

   이러한 문제를 방지하고자 기본값을 설정하기 위해 단축 평가가 사용되었다.

   ```jsx
   function getStringLength(str) {
     str = str || "";
     return str.length;
   }

   getStringLength(); // -> 0
   getStringLength("hi"); // -> 2
   ```

   그러나, ES6 버전에서 매개변수에 직접 기본값을 설정하는 기능이 추가되면서 사용될 일이 줄었다.

   ```jsx
   // ES6의 매개변수의 기본값 설정
   function getStringLength(str = "") {
     return str.length;
   }

   getStringLength(); // -> 0
   getStringLength("hi"); // -> 2
   ```

<br/>

### nullish 병합 연산자 (`??`)

- 여러 피연산자 중 그 값이 정의되어있는 변수를 찾는 연산자이다. (`null` / `undefined` 가 아닌 변수)
  - `a ?? b` → `a ≠ null || undefined` 라면 a를 반환한다.
  - `a ?? b` → `a = null || undefined` 라면 b를 반환한다.
- nullish 병합 연산자(`??`)는 `false`로 평가되더라도 `null`, `undefined`가 아니라면 반환한다.

  ```jsx
  let height = 0;

  console.log(height || 100); // 100
  // height = false 이기 때문에 100 true 값인 100을 반환

  console.log(height ?? 100); // 0
  // height 이 0으로 정의된 변수이기 때문에 0 을 출력
  ```

- 논리합(OR) 연산자(`||`), 논리곱(AND) 연산자(`&&`) 와는 같이 사용할 수 없다.
- 우선순위가 낮은 편이라 할당 연산자(`=`), 옵셔널 체이닝(`?`) 외 대부분의 연산자보다 나중에 평가된다.

  - 복잡한 표현식에 사용된다면 소괄호를 추가할 것을 권장한다.

  ```jsx
  let height = null;
  let width = null;

  // 원치 않는 결과
  let area = height ?? 100 * width ?? 50;
  // let area = height ?? (100 * width) ?? 50; // 0

  // 괄호를 추가
  let area = (height ?? 100) * (width ?? 50); // 5000
  ```

<br/>

### 논리 할당 연산자 (Logical Assignment Operators)

- 논리 연산자의 연산과 할당을 동시에 하는 연산이다.
- 논리 연산자 ( `||`, `&&`, `??` ) + 할당 연산자 ( `=` )
  ```jsx
  x = x || y;
  x ||= y; // x가 false 라면 일 때 y값을 x에 할당

  x = x && y;
  x &&= y; // x가 true 라면 일 때 y값을 x에 할당

  x = x ?? y;
  x ??= y; // x가 undefined, null 일 때 y값을 x에 할당
  ```

<br/>

## 🤔궁금한 점

## 📌중요한 점
