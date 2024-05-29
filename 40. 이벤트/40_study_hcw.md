# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

# 이벤트 드리븐 프로그래밍

브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 **이벤트**를 발생시킨다.

- **이벤트 핸들러** : 이벤트가 발생했을 때 호출될 함수
- **이벤트 핸들러 등록** : 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것

이벤트와 이벤트 핸들러를 통해 사용자와 애플리케이션은 상호작용하는데 이때 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 **이벤트 드리븐 프로그래밍**이라고 한다.

<br>

# 이벤트 핸들러 등록

이벤트 핸들러를 등록하는 방법에는 3가지가 있다.

## 이벤트 핸들러 어트리뷰트 방식

HTML 요소의 어트리뷰트 중에는 이벤트에 대응하는 이벤트 핸들러 어트리뷰트가 있다.

이벤트 핸들러 어트리뷰트 이름 : `on + 이벤트 타입` ex) onchange

이벤트 핸들러 어트리뷰트에 함수 호출문을 할당하면 이벤트 핸들러가 등록됨

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

- 이벤트 핸들러 어트리뷰트 값으로 함수 호출문을 할당함
- `이벤트 핸들러 어트리뷰트 값은 이벤트 핸들러의 함수 몸체를 의미한다.`
- 이벤트 핸들러 어트리뷰트 방식은 오래된 방식으로 더는 사용하지 않는 것이 좋다.

<br>

## 이벤트 핸들러 프로퍼티 방식

window 객체, Document, HTMLElement 타입의 DOM 노드 객체에는 이벤트에 대응하는 이벤트 핸들러 프로퍼티가 있다.

이벤트 핸들러 프로퍼티 키 : `on + 이벤트타입` ex) onclick

이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록됨

이벤트 핸들러 등록을 위해서는 `이벤트 타깃, 이벤트 타입, 이벤트 핸들러`를 지정해야한다.

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

- $button: 이벤트 타깃 (이벤트 발생시키는 객체)
- onclick: on + 이벤트 타입 (이벤트 종류)
- function: 이벤트 핸들러
- 이벤트 핸들러 프로퍼티에는 단 하나의 이벤트 핸들러만 바인딩 가능하다는 단점이 있음

<br>

## addEventListener 메서드 방식

addEventListener 메서드의 첫 번째 매개변수에는 이벤트의 종류를 나타내는 이벤트 타입을 전달함 (이때 on 접두사는 붙이지 않음)

두 번째 매개변수에는 이벤트 핸들러를 전달

세 번째 매개변수에는 이벤트를 캐치할 이벤트 전파 단계를 지정한다.

- false , 생략 : 버블링 단계에서 이벤트 캐치
- true : 캡처링 단계에서 이벤트 캐치

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

addEventListener 메서드 방식은 이벤트 핸들러 프로퍼티에 바인딩된 이벤트 핸들러에 아무런 영향을 주지 않는다.

따라서 하나 이상의 이벤트 핸들러를 등록할 수 있으며 등록된 순서대로 호출된다.

<br>

# 이벤트 객체

이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성되고 이벤트 핸들러의 첫 번째 인수로 전달된다.

따라서 이벤트 객체를 전달받으려면 이벤트 핸들러를 정의할 때 이벤트 객체를 받을 매개변수를 선언해야한다

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

이벤트 핸들러 어트리뷰트 방식으로 이벤트 핸들러를 등록했다면 **event**를 통해 이벤트 객체를 전달받을 수 있다

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

- 이 경우 핸들러의 첫 번째 매개변수 이름은 반드시 event 이어야 함
- 이벤트 핸들러 어트리뷰트 값은 함수 몸체를 의미하기 때문.

  <br>

# 이벤트 전파

DOM 트리 상에 존재하는 DOM요소 노드에서 발생한 이벤트는 DOM트리를 통해 전파됨 => `이벤트 전파`

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

- li요소를 클릭하면 클릭 이벤트가 발생함
- `이때 생성된 이벤트 객체는 이벤트를 발생시킨 DOM요소인 이벤트 타깃을 중심으로 DOM트리를 통해 전파됨`

  <br>

![image](https://github.com/chowonn/js-deepdive-study/assets/70478015/46096ef6-2faf-498f-9948-230e8041ef1c)

1. 이벤트 객체가 생성되면 window부터 시작해서 이벤트 타깃 방향으로 전파된다 (상위 요소에서 하위 요소로) => `캡처링 단계`

2. 이벤트 객체가 이벤트를 발생시킨 이벤트 타깃에 도달함 => `타깃 단계`

3. 다시 이벤트 타깃부터 window 방향으로 전파 (하위 요소에서 상위 요소로) => `버블링 단계`

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

  <br>

# 이벤트 위임

- **이벤트 위임** : 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 것이 아닌 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법
- 주의할 점 => 상위 요소에 이벤트 핸들러를 등록하기에 이벤트 타깃, 즉 원하지 않는 요소에서 이벤트가 발생할 수도 있음

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

- ul#fruits 요소에 바인딩된 이벤트 핸들러는 하위 요소 중에서 클릭 이벤트를 발생시킨 모든 DOM 요소에 반응함
- 따라서 이벤트 반응이 필요한 DOM 요소에 한정하여 이벤트 핸들러가 실행되도록 이벤트 타깃을 검사해야 함

# 📗배운 점

<br>

# 🤔궁금한 점
