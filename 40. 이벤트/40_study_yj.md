## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 40 이벤트

## 40.1 이벤트 드리븐 프로그래밍

이벤트가 발생했을 때 호출될 함수 : 이벤트 핸들러

이벤트가 발생했을 떄 브라우저에게 이벤트 핸들러의 호출을 위임하는 것 : 이벤트 핸들러 등록

프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식 : 이벤트 드리븐 프로그래밍

## 40.3 이벤트 핸들러 등록

### 40.3.1 이벤트 핸들러 어트리뷰트 방식

이벤트 핸들러 어트리뷰트의 이름은 onclick과 같이 on 접두사와 이벤트 종류를 나타내는 이벤트 타입으로 이루어짐

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="sayHi('Lee')">Click me!</button>
    <script>
      function sayHi(name) {
        console.log(`Hi! ${name}.`);
      }
    </script>
  </body>
</html>
```

- 이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문을 할당
- 이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미

### 40.3.2 이벤트 핸들러 프로퍼티 방식

이벤트 핸들러 프로퍼티의 키는 이벤트 핸들러 어트리뷰트와 마찬가지로 onclick과 같이 on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있음

이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
      $button.onclick = function () {
        console.log("button click");
      };
    </script>
  </body>
</html>
```

- ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/56419102-f397-4fd5-b8be-7649c188bd51)
- 이벤트를 발생시킬 객체인 이벤트 타깃, 이벤트의 종류를 나타내는 문자열인 이벤트 타입, 이벤트 핸들러

이벤트 핸들러 프로퍼티 방식은 이벤트 핸들러 어트리뷰트 방식의 HTML과 자바스크립트가 뒤섞이는 문제를 해결할 수 있지만 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩할 수 있다는 단점이 있음

### 40.3.3 addEventListener 메서드 방식

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/700ffaac-ec3f-4cf0-b85c-a6de74735849)

addEventListener 메서드의 첫 번째 매개변수에는 이벤트의 종류를 나타내는 문자열인 이벤트 타입을 전달

이때 이벤트 핸들러 프로퍼티 방식과는 달리 on 접두사를 붙이지 않음

두 번째 매개변수에는 이벤트 핸들러를 전달

마지막 매개변수에는 이벤트를 캐치할 이벤트 전파 단계를 지정

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // 이벤트 핸들러 프로퍼티 방식
      // $button.onclick = function () {
      //   console.log('button click');
      // };

      // addEventListener 메서드 방식
      $button.addEventListener("click", function () {
        console.log("button click");
      });
    </script>
  </body>
</html>
```

- 이벤트 핸들러 프로퍼티 방식은 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩하지만 addEventListener 메서드에는 이벤트 핸들러를 인수로 전달

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // 이벤트 핸들러 프로퍼티 방식
      $button.onclick = function () {
        console.log("[이벤트 핸들러 프로퍼티 방식]button click");
      };

      // addEventListener 메서드 방식
      $button.addEventListener("click", function () {
        console.log("[addEventListener 메서드 방식]button click");
      });
    </script>
  </body>
</html>
```

- addEventListener 메서드 방식은 이벤트 핸들러 프로퍼티에 바인딩된 이벤트 핸들러에 아무런 영향을 주지 ㅇ낳음
- 버튼 요소를 클릭하면 2개의 이벤트 핸들러가 모두 호출

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // addEventListener 메서드는 동일한 요소에서 발생한 동일한 이벤트에 대해
      // 하나 이상의 이벤트 핸들러를 등록할 수 있다.
      $button.addEventListener("click", function () {
        console.log("[1]button click");
      });

      $button.addEventListener("click", function () {
        console.log("[2]button click");
      });
    </script>
  </body>
</html>
```

- 동일한 HTML 요소에서 발생한 동일한 이벤트에 대해 하나 이상의 이벤트 핸들러를 등록할 수 있음

## 40.4 이벤트 핸들러 제거

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      const handleClick = () => console.log("button click");

      // 이벤트 핸들러 등록
      $button.addEventListener("click", handleClick);

      // 이벤트 핸들러 제거
      // addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에
      // 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않는다.
      $button.removeEventListener("click", handleClick, true); // 실패
      $button.removeEventListener("click", handleClick); // 성공
    </script>
  </body>
</html>
```

- EventTarget.prototype.removeEventListener 메서드 사용

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      const handleClick = () => console.log("button click");

      // 이벤트 핸들러 프로퍼티 방식으로 이벤트 핸들러 등록
      $button.onclick = handleClick;

      // removeEventListener 메서드로 이벤트 핸들러를 제거할 수 없다.
      $button.removeEventListener("click", handleClick);

      // 이벤트 핸들러 프로퍼티에 null을 할당하여 이벤트 핸들러를 제거한다.
      $button.onclick = null;
    </script>
  </body>
</html>
```

- 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러는 removeEventListener 메서드로 제거할 수 없음
- 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러를 제거하려면 이벤트 핸들러 프로퍼티에 null을 할당

## 40.5 이벤트 객체

이벤트가 발생하면 이벤트에 관련한 정보를 담고 있는 이벤트 객체가 동적으로 생성

생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달

```html
<!DOCTYPE html>
<html>
  <body>
    <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
    <em class="message"></em>
    <script>
      const $msg = document.querySelector(".message");

      // 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
      function showCoords(e) {
        $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
      }

      document.onclick = showCoords;
    </script>
  </body>
</html>
```

- 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달되어 매개변수 e에 암묵적으로 할당
- 이벤트 객체를 전달받으려면 전달받을 매개변수를 명시적으로 선언해야 함

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      html,
      body {
        height: 100%;
      }
    </style>
  </head>
  <!-- 이벤트 핸들러 어트리뷰트 방식의 경우 event가 아닌 다른 이름으로는 이벤트 객체를
전달받지 못한다. -->
  <body onclick="showCoords(event)">
    <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
    <em class="message"></em>
    <script>
      const $msg = document.querySelector(".message");

      // 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
      function showCoords(e) {
        $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
      }
    </script>
  </body>
</html>
```

- 이벤트 핸들러 어트리뷰트 방식의 경우 이벤트 객체를 전달받으려면 이벤트 핸들러의 첫 번째 매개변수 이름이 반드시 event이어야 함
  - 다른 이름이면 전달받지 못함
  - 이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성되는 이벤트 핸들러의 함수 몸체를 의미하기 때문
  - 이때 암묵적으로 생성된 onclick 이벤트 핸들러의 첫 번째 매개변수의 이름이 event로 암묵적으로 명명되기 때문

### 40.5.1 이벤트 객체의 상속 구조

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/0de0ad8d-3753-4ffb-a6b7-712e665f27c5)

- Event, UIEvent, MouseEvent 등 모두는 생성자 함수

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      // Event 생성자 함수를 호출하여 foo 이벤트 타입의 Event 객체를 생성한다.
      let e = new Event("foo");
      console.log(e);
      // Event {isTrusted: false, type: "foo", target: null, ...}
      console.log(e.type); // "foo"
      console.log(e instanceof Event); // true
      console.log(e instanceof Object); // true

      // FocusEvent 생성자 함수를 호출하여 focus 이벤트 타입의 FocusEvent 객체를 생성한다.
      e = new FocusEvent("focus");
      console.log(e);
      // FocusEvent {isTrusted: false, relatedTarget: null, view: null, ...}

      // MouseEvent 생성자 함수를 호출하여 click 이벤트 타입의 MouseEvent 객체를 생성한다.
      e = new MouseEvent("click");
      console.log(e);
      // MouseEvent {isTrusted: false, screenX: 0, screenY: 0, clientX: 0, ... }

      // KeyboardEvent 생성자 함수를 호출하여 keyup 이벤트 타입의 KeyboardEvent 객체를
      // 생성한다.
      e = new KeyboardEvent("keyup");
      console.log(e);
      // KeyboardEvent {isTrusted: false, key: "", code: "", ctrlKey: false, ...}

      // InputEvent 생성자 함수를 호출하여 change 이벤트 타입의 InputEvent 객체를 생성한다.
      e = new InputEvent("change");
      console.log(e);
      // InputEvent {isTrusted: false, data: null, inputType: "", ...}
    </script>
  </body>
</html>
```

- 생성자 함수를 호출하여 이벤트를 호출할 수 있음
- 이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체도 생성자 함수에 의해 생성

```html
<!DOCTYPE html>
<html>
  <body>
    <input type="text" />
    <input type="checkbox" />
    <button>Click me!</button>
    <script>
      const $input = document.querySelector("input[type=text]");
      const $checkbox = document.querySelector("input[type=checkbox]");
      const $button = document.querySelector("button");

      // load 이벤트가 발생하면 Event 타입의 이벤트 객체가 생성된다.
      window.onload = console.log;

      // change 이벤트가 발생하면 Event 타입의 이벤트 객체가 생성된다.
      $checkbox.onchange = console.log;

      // focus 이벤트가 발생하면 FocusEvent 타입의 이벤트 객체가 생성된다.
      $input.onfocus = console.log;

      // input 이벤트가 발생하면 InputEvent 타입의 이벤트 객체가 생성된다.
      $input.oninput = console.log;

      // keyup 이벤트가 발생하면 KeyboardEvent 타입의 이벤트 객체가 생성된다.
      $input.onkeyup = console.log;

      // click 이벤트가 발생하면 MouseEvent 타입의 이벤트 객체가 생성된다.
      $button.onclick = console.log;
    </script>
  </body>
</html>
```

- Event 인터페이스는 DOM 내에서 발생한 이벤트에 의해 생성되는 이벤트 객체
- Event 인터페이스에는 모든 이벤트 객체의 공통 프로퍼티가 정의되어 있고 FocusEvent, MouseEvent 같은 하위 인터페이스에는 이벤트 타입에 따라 고유한 프로퍼티가 정의
- 이벤트 객체의 프로퍼티는 발생한 이벤트의 타입에 따라 달라짐

### 40.5.2 이벤트 객체의 공통 프로퍼티

Event.prototype에 정의되어 있는 이벤트 관련 프로퍼티는 UIEvent, CustomEvent, MouseEvent 등 모든 파생 이벤트 객체에 상속

Event 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티

```html
<!DOCTYPE html>
<html>
  <body>
    <input type="checkbox" />
    <em class="message">off</em>
    <script>
      const $checkbox = document.querySelector("input[type=checkbox]");
      const $msg = document.querySelector(".message");

      // change 이벤트가 발생하면 Event 타입의 이벤트 객체가 생성된다.
      $checkbox.onchange = (e) => {
        console.log(Object.getPrototypeOf(e) === Event.prototype); // true

        // e.target은 change 이벤트를 발생시킨 DOM 요소 $checkbox를 가리키고
        // e.target.checked는 체크박스 요소의 현재 체크 상태를 나타낸다.
        $msg.textContent = e.target.checked ? "on" : "off";
      };
    </script>
  </body>
</html>
```

- 체크박스 요소의 체크 상태가 변경되면 현재 상태를 출력
- 사용자의 입력에 의해 체크박스 요소의 체크 상태가 변경되면 checked 프로퍼티의 갑싱 변겨오디고 change 이벤트가 발생
- 이때 Event 타입의 이벤트 객체가 생성
- 이벤트 객체의 target 프로퍼티는 이벤트를 발생시킨 객체
- 따라서 target 프로퍼티가 가리키는 객체는 change 이벤트를 발생시킨 DOM 요소 $checkbox이고 이 객체의 checked 프로퍼티는 현재의 체크 상태를 나타냄

## 40.6 이벤트 전파

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
  </body>
</html>
```

- ul 요소의 두 번째 자식 요소인 li 요소를 클릭하면 클릭 이벤트 발생
- 이 때 생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파
- ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/747703fc-3011-4b0a-bc29-d86e16bbba77)

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById("fruits");

      // #fruits 요소의 하위 요소인 li 요소를 클릭한 경우
      $fruits.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 3: 버블링 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLLIElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
      });
    </script>
  </body>
</html>
```

- li 요소를 클릭하면 클릭 이벤트가 발생하여 클릭 이벤트 객체가 생성되고 클릭된 li 요소가 이벤트 타깃이 됨
- 이때 클릭 이벤트 객체는 window에서 시작해서 이벤트 타깃 방향으로 전파되며 이것이 캡처링 단계
- 이후 이벤트 객체는 이벤트를 발생시킨 이벤트 타깃에 도달하며 이것이 타깃 단계
- 이후 이벤트 객체는 이벤트 타깃에서 시작해서 window 방향으로 전파되며 이것이 버블링 단계

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById("fruits");
      const $banana = document.getElementById("banana");

      // #fruits 요소의 하위 요소인 li 요소를 클릭한 경우
      // 캡처링 단계의 이벤트를 캐치한다.
      $fruits.addEventListener(
        "click",
        (e) => {
          console.log(`이벤트 단계: ${e.eventPhase}`); // 1: 캡처링 단계
          console.log(`이벤트 타깃: ${e.target}`); // [object HTMLLIElement]
          console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
        },
        true
      );

      // 타깃 단계의 이벤트를 캐치한다.
      $banana.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 2: 타깃 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLLIElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLLIElement]
      });

      // 버블링 단계의 이벤트를 캐치한다.
      $fruits.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 3: 버블링 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLLIElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
      });
    </script>
  </body>
</html>
```

- 이벤트 핸들러 어트리뷰트/프로퍼티 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계의 이벤트만 캐치
- addEventListener 메서드 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계뿐만 아니라 캡처링 단계의 이벤트도 선별적으로 캐치할 수 잇음

이처럼 이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위 DOM 요소에서도 캐치할 수 있음

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/b638452a-4ef1-4597-b031-9c6a4956f250)

위 이벤트는 버블링되지 않으므로 이벤트 타깃의 상위 요소에서 캐치하려면 캡처링 단계의 이벤트를 캐치해야 함

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { height: 100%; }
  </style>
<body>
  <p>버블링과 캡처링 이벤트 <button>버튼</button></p>
  <script>
    // 버블링 단계의 이벤트를 캐치
    document.body.addEventListener('click', () => {
      console.log('Handler for body.');
    });

    // 캡처링 단계의 이벤트를 캐치
    document.querySelector('p').addEventListener('click', () => {
      console.log('Handler for paragraph.');
    }, true);

    // 타깃 단계의 이벤트를 캐치
    document.querySelector('button').addEventListener('click', () => {
      console.log('Handler for button.');
    });
  </script>
</body>
</html>
```

- 이벤트는 캡처링 - 타깃 - 버블링 단계로 전파되므로 만약 button 요소에서 클릭 이벤트가 발생하면 먼저 캡처링 단계를 캐치하는 p 요소의 이벤트 핸들러가 호출, 그후 버블링 단계의 이벤트를 캐치하는 body 요소의 이벤트 핸들러가 순차적으로 호출
- 출력은 다음과 같음

```
Handler for paragraph.
Handler for button.
Handler for body.
```

- 만약 p 요소에서 클릭 이벤트가 발생하면 다음과 같이 출력

```
Handler for paragraph.
Handler for body.
```

## 40.7 이벤트 위임

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      #fruits {
        display: flex;
        list-style-type: none;
        padding: 0;
      }

      #fruits li {
        width: 100px;
        cursor: pointer;
      }

      #fruits .active {
        color: red;
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <nav>
      <ul id="fruits">
        <li id="apple" class="active">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
    </nav>
    <div>선택된 내비게이션 아이템: <em class="msg">apple</em></div>
    <script>
      const $fruits = document.getElementById("fruits");
      const $msg = document.querySelector(".msg");

      // 사용자 클릭에 의해 선택된 내비게이션 아이템(li 요소)에 active 클래스를 추가하고
      // 그 외의 모든 내비게이션 아이템의 active 클래스를 제거한다.
      function activate({ target }) {
        [...$fruits.children].forEach(($fruit) => {
          $fruit.classList.toggle("active", $fruit === target);
          $msg.textContent = target.id;
        });
      }

      // 모든 내비게이션 아이템(li 요소)에 이벤트 핸들러를 등록한다.
      document.getElementById("apple").onclick = activate;
      document.getElementById("banana").onclick = activate;
      document.getElementById("orange").onclick = activate;
    </script>
  </body>
</html>
```

- 모든 내비게이션 아이템(li 요소)이 클릭 이벤트에 반응하도록 모든 내비게이션 아이템에 이벤트 핸들러인 activate를 등록
- 만일 내비게이션 아이템이 100개라면 100개의 이벤트 핸들러를 등록해야 함

이벤트 위임은 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법을 말함

이벤트 위임을 통해 상위 DOM 요소에 이벤트 핸들러를 등록하면 여러 개의 하위 DOM 요소에 이벤트 핸들러를 등록할 필요가 없음

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      #fruits {
        display: flex;
        list-style-type: none;
        padding: 0;
      }

      #fruits li {
        width: 100px;
        cursor: pointer;
      }

      #fruits .active {
        color: red;
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <nav>
      <ul id="fruits">
        <li id="apple" class="active">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
    </nav>
    <div>선택된 내비게이션 아이템: <em class="msg">apple</em></div>
    <script>
      const $fruits = document.getElementById("fruits");
      const $msg = document.querySelector(".msg");

      // 사용자 클릭에 의해 선택된 내비게이션 아이템(li 요소)에 active 클래스를 추가하고
      // 그 외의 모든 내비게이션 아이템의 active 클래스를 제거한다.
      function activate({ target }) {
        // 이벤트를 발생시킨 요소(target)가 ul#fruits의 자식 요소가 아니라면 무시한다.
        if (!target.matches("#fruits > li")) return;

        [...$fruits.children].forEach(($fruit) => {
          $fruit.classList.toggle("active", $fruit === target);
          $msg.textContent = target.id;
        });
      }

      // 이벤트 위임: 상위 요소(ul#fruits)는 하위 요소의 이벤트를 캐치할 수 있다.
      $fruits.onclick = activate;
    </script>
  </body>
</html>
```

- 이벤트 위임을 통해 하위 DOM 요소에서 발생한 이벤트를 처리할 때 주의할 점
  - 상위 요소에 이벤트 핸들러를 등록하기 때문에 이벤트 타깃, 즉 이벤트를 실제로 발생시킨 DOM 요소가 개발자가 기대한 DOM 요소가 아닐 수도 있다는 것
  - 이벤트 반응에 필요한 DOM 요소에 한정하여 이벤트 핸들러가 실행되도록 이벤트 타깃을 검사할 필요가 있음

## 40.8 DOM 요소의 기본 동작 조작

### 40.8.1 DOM 요소의 기본 동작 중단

```html
<!DOCTYPE html>
<html>
  <body>
    <a href="https://www.google.com">go</a>
    <input type="checkbox" />
    <script>
      document.querySelector("a").onclick = (e) => {
        // a 요소의 기본 동작을 중단한다.
        e.preventDefault();
      };

      document.querySelector("input[type=checkbox]").onclick = (e) => {
        // checkbox 요소의 기본 동작을 중단한다.
        e.preventDefault();
      };
    </script>
  </body>
</html>
```

- 이벤트 객체의 preventDefault 메서드는 DOM 요소의 기본 동작을 중단시킴

### 40.8.2 이벤트 전파 방지

```html
<!DOCTYPE html>
<html>
  <body>
    <div class="container">
      <button class="btn1">Button 1</button>
      <button class="btn2">Button 2</button>
      <button class="btn3">Button 3</button>
    </div>
    <script>
      // 이벤트 위임. 클릭된 하위 버튼 요소의 color를 변경한다.
      document.querySelector(".container").onclick = ({ target }) => {
        if (!target.matches(".container > button")) return;
        target.style.color = "red";
      };

      // .btn2 요소는 이벤트를 전파하지 않으므로 상위 요소에서 이벤트를 캐치할 수 없다.
      document.querySelector(".btn2").onclick = (e) => {
        e.stopPropagation(); // 이벤트 전파 중단
        e.target.style.color = "blue";
      };
    </script>
  </body>
</html>
```

- 이벤트 객체의 stopPropagation 메서드는 이벤트 전파를 중지
- 상위 DOM 요소인 container 요소에 이벤트를 위임
- 하위 DOM 요소인 container 요소가 캐치하여 이벤트를 처리
- 하위 요소 중 btn2 요소는 자체적으로 이벤트를 처리
  - btn2 요소는 자신이 발생시킨 이벤트가 전파되는 것을 중단하여 자신에게 바인딩된 이벤트 핸들러만 실행되도록 함

## 40.9 이벤트 핸들러 내부의 this

### 49.9.1 이벤트 핸들러 어트리뷰트 방식

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="handleClick()">Click me</button>
    <script>
      function handleClick() {
        console.log(this); // window
      }
    </script>
  </body>
</html>
```

- 이벤트 핸들러 어트리뷰트의 값으로 지정한 문자열은 암묵적으로 생성되는 이벤트 핸들러의 문
- 따라서 handleClick 함수는 이벤트 핸들러에 의해 일반 함수로 호출
- 일반 함수로서 호출되는 함수 내부의 this는 전역 객체를 가리킴
- 따라서 handleClick 함수 내부의 this는 전역 객체 window를 가리킴

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="handleClick(this)">Click me</button>
    <script>
      function handleClick(button) {
        console.log(button); // 이벤트를 바인딩한 button 요소
        console.log(this); // window
      }
    </script>
  </body>
</html>
```

- 이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소를 가리킴

### 40.9.2 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="btn1">0</button>
    <button class="btn2">0</button>
    <script>
      const $button1 = document.querySelector(".btn1");
      const $button2 = document.querySelector(".btn2");

      // 이벤트 핸들러 프로퍼티 방식
      $button1.onclick = function (e) {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // $button1
        console.log(e.currentTarget); // $button1
        console.log(this === e.currentTarget); // true

        // $button1의 textContent를 1 증가시킨다.
        ++this.textContent;
      };

      // addEventListener 메서드 방식
      $button2.addEventListener("click", function (e) {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // $button2
        console.log(e.currentTarget); // $button2
        console.log(this === e.currentTarget); // true

        // $button2의 textContent를 1 증가시킨다.
        ++this.textContent;
      });
    </script>
  </body>
</html>
```

- 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식 모두 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킴

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="btn1">0</button>
    <button class="btn2">0</button>
    <script>
      const $button1 = document.querySelector(".btn1");
      const $button2 = document.querySelector(".btn2");

      // 이벤트 핸들러 프로퍼티 방식
      $button1.onclick = (e) => {
        // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
        console.log(this); // window
        console.log(e.currentTarget); // $button1
        console.log(this === e.currentTarget); // false

        // this는 window를 가리키므로 window.textContent에 NaN(undefined + 1)을 할당한다.
        ++this.textContent;
      };

      // addEventListener 메서드 방식
      $button2.addEventListener("click", (e) => {
        // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
        console.log(this); // window
        console.log(e.currentTarget); // $button2
        console.log(this === e.currentTarget); // false

        // this는 window를 가리키므로 window.textContent에 NaN(undefined + 1)을 할당한다.
        ++this.textContent;
      });
    </script>
  </body>
</html>
```

- 화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킴
- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="btn">0</button>
    <script>
      class App {
        constructor() {
          this.$button = document.querySelector(".btn");
          this.count = 0;

          // increase 메서드를 이벤트 핸들러로 등록
          // this.$button.onclick = this.increase;

          // increase 메서드 내부의 this가 인스턴스를 가리키도록 한다.
          this.$button.onclick = this.increase.bind(this);
        }

        increase() {
          // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킨다.
          // 따라서 this.$button은 this.$button.$button과 같다.
          this.$button.textContent = ++this.count;
          // -> TypeError: Cannot set property 'textContent' of undefined
        }
      }

      new App();
    </script>
  </body>
</html>
```

- 클래스에서 이벤트 핸들러를 바인딩하는 경우
- increase 메서드 내부의 this는 클래스가 생성할 인스턴스를 가리키지 않음
- 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리키기 때문에 increase 메서드 내부의 this는 this.$button을 가리킴
- 따라서 increase 메서드를 이벤트 핸들러로 바인딩할 때 bind 메서드를 사용해 this를 전달하여 increase 메서드 내부의 this가 클래스가 생성할 인스턴스를 가리키도록 해야 함

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="btn">0</button>
    <script>
      class App {
        constructor() {
          this.$button = document.querySelector(".btn");
          this.count = 0;

          // 화살표 함수인 increase를 이벤트 핸들러로 등록
          this.$button.onclick = this.increase;
        }

        // 클래스 필드 정의
        // increase는 인스턴스 메서드이며 내부의 this는 인스턴스를 가리킨다.
        increase = () => (this.$button.textContent = ++this.count);
      }
      new App();
    </script>
  </body>
</html>
```

- 클래스 필드에 할당한 화살표 함수를 이벤트 핸들러로 등록하여 이벤트 핸들러 내부의 this가 인스턴스를 가리키도록 할 수 있음
- 다만 이떄 이벤트 핸들러 increase는 프로토타입 메서드가 아닌 인스턴스 메서드가 됨

## 40.10 이벤트 핸들러에 인수 전달

```html
<!DOCTYPE html>
<html>
  <body>
    <label>User name <input type="text" /></label>
    <em class="message"></em>
    <script>
      const MIN_USER_NAME_LENGTH = 5; // 이름 최소 길이
      const $input = document.querySelector("input[type=text]");
      const $msg = document.querySelector(".message");

      const checkUserNameLength = (min) => {
        $msg.textContent =
          $input.value.length < min ? `이름은 ${min}자 이상 입력해 주세요` : "";
      };

      // 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
      $input.onblur = () => {
        checkUserNameLength(MIN_USER_NAME_LENGTH);
      };
    </script>
  </body>
</html>
```

- 이벤트 핸들러 어트리뷰트 방식은 함수 호출문을 사용할 수 있기 때문에 인수를 전달할 수 있지만 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식의 경우 이벤트 핸들러를 브라우저가 호출하기 때문에 함수 호출문이 아닌 함수 자체를 등록해야 함
- 하지만 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달할 수 있음

```html
<!DOCTYPE html>
<html>
  <body>
    <label>User name <input type="text" /></label>
    <em class="message"></em>
    <script>
      const MIN_USER_NAME_LENGTH = 5; // 이름 최소 길이
      const $input = document.querySelector("input[type=text]");
      const $msg = document.querySelector(".message");

      // 이벤트 핸들러를 반환하는 함수
      const checkUserNameLength = (min) => (e) => {
        $msg.textContent =
          $input.value.length < min ? `이름은 ${min}자 이상 입력해 주세요` : "";
      };

      // 이벤트 핸들러를 반환하는 함수를 호출하면서 인수를 전달한다.
      $input.onblur = checkUserNameLength(MIN_USER_NAME_LENGTH);
    </script>
  </body>
</html>
```

- 이벤트 핸들러를 반환하는 함수를 호출하면서 인수를 전달할 수 있음
- checkUserNameLength 함수는 함수를 반환
- 따라서 $input.onblur에서는 결국 checkUserNameLength 함수가 반환하는 함수가 바인딩 됨

## 40.11 커스텀 이벤트

### 40.11.1 커스텀 이벤트 생성

Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있음

이처럼 개발자의 의도로 생성된 이벤트를 커스텀 이벤트라 함

```javascript
// KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new KeyboardEvent("keyup");
console.log(keyboardEvent.type); // keyup

// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent("foo");
console.log(customEvent.type); // foo
```

- 이벤트 생성자 함수는 첫 번째 인수로 이벤트 타입을 나타내는 문자열을 전달받음
- 이때 이벤트 타임을 나타내는 문자열은 기존 이벤트 타입을 사용할 수도 있고, 기존 이벤트 타입이 아닌 임의의 문자열을 사용하여 새로운 이벤트 타입을 지정할 수도 있음

```javascript
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new MouseEvent("click");
console.log(customEvent.type); // click
console.log(customEvent.bubbles); // false
console.log(customEvent.cancelable); // false
```

- 생성된 커스텀 이벤트 객체는 버블링되지 않으며 preventDefault 메서드로 취소할 수도 없음
- 커스텀 이벤트 객체는 bubbles와 cancelable 프로퍼티의 값이 false로 기본 설정

```javascript
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new MouseEvent("click", {
  bubbles: true,
  cancelable: true,
});

console.log(customEvent.bubbles); // true
console.log(customEvent.cancelable); // true
```

- 커스텀 이벤트 객체의 bubbles 또는 cancelable 프로퍼티를 ture로 설정하려면 이벤트 생성자 함수의 두 번째 인수로 bubbles 또는 cancelable 프로퍼티를 갖는 객체를 전달

```javascript
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const mouseEvent = new MouseEvent("click", {
  bubbles: true,
  cancelable: true,
  clientX: 50,
  clientY: 100,
});

console.log(mouseEvent.clientX); // 50
console.log(mouseEvent.clientY); // 100

// KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new KeyboardEvent("keyup", { key: "Enter" });

console.log(keyboardEvent.key); // Enter
```

- 커스텀 이벤트 객체에는 bubbles 또는 cancelable 프로퍼티뿐만 아니라 이벤트 타입에 따라 가지는 이벤트 고유의 프로퍼티 값을 지정할 수 있음
- 이벤트 객체 고유의 프로퍼티 값을 지정하려면 다음과 같이 이벤트 생성자 함수의 두 번째 인수로 프로퍼티를 전달

```javascript
// InputEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new InputEvent("foo");
console.log(customEvent.isTrusted); // false
```

- 이벤트 생성자 함수로 생성한 커스텀 이벤트는 isTrusted 프로퍼티의 값이 언제나 false
- 커스텀 이벤트가 아닌 사용자의 행위에 의해 발생한 이벤트에 의해 생성된 이벤트 객체의 isTrusted 프로퍼티 값은 언제나 true

### 40.11.2 커스텀 이벤트 디스패치

생성된 커스텀 이벤트는 dispatchEvent 메서드로 디스패치 할 수 있음

dispatchEvent 메서드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트가 발생

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="btn">Click me</button>
    <script>
      const $button = document.querySelector(".btn");

      // 버튼 요소에 click 커스텀 이벤트 핸들러를 등록
      // 커스텀 이벤트를 디스패치하기 이전에 이벤트 핸들러를 등록해야 한다.
      $button.addEventListener("click", (e) => {
        console.log(e); // MouseEvent {isTrusted: false, screenX: 0, ...}
        alert(`${e} Clicked!`);
      });

      // 커스텀 이벤트 생성
      const customEvent = new MouseEvent("click");

      // 커스텀 이벤트 디스패치(동기 처리). click 이벤트가 발생한다.
      $button.dispatchEvent(customEvent);
    </script>
  </body>
</html>
```

- 일반적으로 이벤트 핸들러는 비동기 처리 방식으로 동작하지만 dispatchEvent 메서드는 이벤트 핸들러를 동기 처리 방식으로 호출
  - dispatchEvent 메서드를 호출하면 커스텀 이벤트에 바인딩된 이벤트 핸들러를 직접 호출하는 것과 같음
- 따라서 dispatchEvent 메서드로 이벤트를 디스패치하기 이전에 커스텀 이벤트를 처리할 이벤트 핸들러를 등록해야 함

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="btn">Click me</button>
    <script>
      const $button = document.querySelector(".btn");

      // 버튼 요소에 foo 커스텀 이벤트 핸들러를 등록
      // 커스텀 이벤트를 디스패치하기 이전에 이벤트 핸들러를 등록해야 한다.
      $button.addEventListener("foo", (e) => {
        // e.detail에는 CustomEvent 함수의 두 번째 인수로 전달한 정보가 담겨 있다.
        alert(e.detail.message);
      });

      // CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
      const customEvent = new CustomEvent("foo", {
        detail: { message: "Hello" }, // 이벤트와 함께 전달하고 싶은 정보
      });

      // 커스텀 이벤트 디스패치
      $button.dispatchEvent(customEvent);
    </script>
  </body>
</html>
```

- 기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 커스텀 이벤트 객체를 생성한 경우 반드시 addEventListener 메서드 방식으로 이벤트 핸들러를 등록해야 함
- 이벤트 핸들러 어트리뷰트/프로퍼티 방식을 사용할 수 없는 이유는 'on + 이벤트 타입'으로 이루어진 이벤트 핸들러 어트리뷰트/프로퍼티가 노드에 존재하지 않기 때문

## 🤔궁금한 점

## 📌중요한 점
