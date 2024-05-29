# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

**DOM(Document Object Model)은 HTML 문서의 계층적 구조와 정보를 표현하여 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.**

# 노드

HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.

이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.

**HTML 문서는 요소들의 집합으로 이루어지며 요소 간에는 중첩 관계에 의해 계층적인 부자 관계가 형성된다.**

이러한 HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성한다.

## 노드의 객체 타입

DOM은 노드 객체의 계층적인 구조로 구성되며 노드 객체는 종류가 있고 상속 구조를 갖는다.

노드 객체는 총 12개의 종류가 있으며 중요한 노드 타입은 다음과 같다.

`문서 노드(Document node)`

최상위에 존재하는 루트 노드로서 document 객체를 가리킨다.

document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩되어 있다. 따라서 문서 노드는 window.document 또는 document로 참조할 수 있다.

자바스크립트 코드는 하나의 전역 객체 window를 공유하므로 하나의 document 객체를 바라본다. 즉, HTML 문서당 document 객체는 유일하다.

문서 노드는 진입점 역할이므로 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.

<br>

`요소 노드(Element node)`

요소 노드는 HTML 요소를 가리키는 객체다. 요소 노드는 HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보를 구조화한다. 따라서 요소 노드는 문서의 구조를 표현한다.

<br>

`어트리뷰트 노드(Attribute node)`

어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체다.

요소 노드는 부모 노드와 연결되어 있지만 어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결되어 있다.

즉, 어트리뷰트 노드는 부모 노드가 없으므로 요소 노드의 형제 노드는 아니다. 따라서 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야 한다.

<br>

`텍스트 노드(Text node)`

텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체로 문서의 정보를 표현한다.

텍스트 노드는 요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프 노드다. 즉, 텍스트 노드는 DOM 트리의 최종단이다.

따라서 텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근해야 한다.

<br>

## 노드 객체의 상속 구조

- DOM을 구성하는 노드 객체는 표준 빌트인 객체가 아닌 브라우저 환경에서 추가적으로 제공하는 호스트 객체이다.
- 그러나 노드 객체도 자바크스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.
- 모든 노드 객체는 **`Object`, `EventTarget`, `Node`** 인터페이스를 상속 받는다.
- 또한 문서 노드는 **`Document`와 `HTMLDocument`**를 상속받고, 어트리뷰트 노드는 **`Attr`**, 텍스트 노드는 **`CharacterDate`** 인터페이스를 상속 받는다.

<br>

배열이 객체인 동시에 배열인 것처럼 **`input`** 요소 노드 객체도 다음과 같이 다양한 특성을 갖는다.

| input 요소 노드 객체의 특성                                                | 프로토타입을 제공하는 객체 |
| -------------------------------------------------------------------------- | -------------------------- |
| 객체                                                                       | Object                     |
| 이벤트를 발생시키는 객체                                                   | EventTarget                |
| 트리 자료구조의 노드 객체                                                  | Node                       |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element                    |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체                            | HTMLElement                |
| HTML 요소 중에서 input 요소를 표현하는 객체                                | HTMLInputElement           |

**결과적으로 DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다.**

**이 DOM API를 통해 HTML의 구조와 내용 또는 스타일 등을 동적으로 조작할 수 있다.**

<br>

# 요소 노드 취득

## id를 이용한 요소 노드 취득

- `Document.prototype.getElementById` 메서드 사용
- 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환함. 즉, 단 하나의 요소 노드 반환

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="banana">Apple</li>
      <li id="banana">Banana</li>
      <li id="banana">Orange</li>
    </ul>
    <script>
      // getElementById 메서드는 언제나 단 하나의 요소 노드를 반환한다.
      // 첫 번째 li 요소가 파싱되어 생성된 요소 노드가 반환된다.
      const $elem = document.getElementById("banana");

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = "red";
    </script>
  </body>
</html>
```

<br>

## 태그 이름을 이용한 요소 노드 취득

- getElementsByTagName 메서드 사용
  - Document.prototype.getElementsByTagName : 루트 노드인 문서노드(document)를 통해 호출
  - Element.prototype.getElementsByTagName : 특정 요소 노드를 통해 호출 (특정 요소 노드의 자식 중에서 탐색)
- DOM 컬렉션 객체인 HTMLCollection 객체 반환(유사 배열 이면서 이터러블)

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      // 탐색된 요소 노드들은 HTMLCollection 객체에 담겨 반환된다.
      // HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
      const $elems = document.getElementsByTagName("li");

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      // HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경한다.
      [...$elems].forEach((elem) => {
        elem.style.color = "red";
      });
    </script>
  </body>
</html>
```

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
    <ul>
      <li>HTML</li>
    </ul>
    <script>
      // DOM 전체에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      const $lisFromDocument = document.getElementsByTagName("li");
      console.log($lisFromDocument); // HTMLCollection(4) [li, li, li, li]

      // #fruits 요소의 자손 노드 중에서 태그 이름이 li인 요소 노드를 모두
      // 탐색하여 반환한다.
      const $fruits = document.getElementById("fruits");
      const $lisFromFruits = $fruits.getElementsByTagName("li");
      console.log($lisFromFruits); // HTMLCollection(3) [li, li, li]
    </script>
  </body>
</html>
```

<br>

## class를 이용한 요소 노드 취득

- Document.prototype/getElementsByClassName 메서드 사용
- 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소를 탐색하여 반환함

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <div class="banana">Banana</div>
    <script>
      // DOM 전체에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환한다.
      const $bananasFromDocument = document.getElementsByClassName("banana");
      console.log($bananasFromDocument); // HTMLCollection(2) [li.banana, div.banana]

      // #fruits 요소의 자손 노드 중에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환한다.
      const $fruits = document.getElementById("fruits");
      const $bananasFromFruits = $fruits.getElementsByClassName("banana");

      console.log($bananasFromFruits); // HTMLCollection [li.banana]
    </script>
  </body>
</html>
```

<br>

## CSS 선택자를 이용한 요소 노드 취득

```css
/* 전체 선택자: 모든 요소를 선택 */
* {
  ...;
}
/* 태그 선택자: 모든 p 태그 요소를 모두 선택 */
p {
  ...;
}
/* id 선택자: id 값이 'foo'인 요소를 모두 선택 */
#foo {
  ...;
}
/* class 선택자: class 값이 'foo'인 요소를 모두 선택 */
.foo {
  ...;
}
/* 어트리뷰트 선택자: input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택 */
input[type="text"] {
  ...;
}
/* 후손 선택자: div 요소의 후손 요소 중 p 요소를 모두 선택 */
div p {
  ...;
}
/* 자식 선택자: div 요소의 자식 요소 중 p 요소를 모두 선택 */
div > p {
  ...;
}
/* 인접 형제 선택자: p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택 */
p + ul {
  ...;
}
/* 일반 형제 선택자: p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul {
  ...;
}
/* 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택 */
a:hover {
  ...;
}
/* 가상 요소 선택자: p 요소의 콘텐츠의 앞에 위치하는 공간을 선택
   일반적으로 content 프로퍼티와 함께 사용된다. */
p::before {
  ...;
}
```

<br>

### querySelectorAll()

- 인수로 전달한 CSS 선택자를 만족 시키는 모든 요소 노드를 탐색하여 반환
- 여러개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 를 반환
- NodeList는 유사 배열 객체 이면서 이터러블
- 요소가 존재하지 않는 경우 빈 NodeList 객체 반환
- 문법에 맞지 않는 경우 DOMException 에러 발생

### querySelector

- 인수로 전달할 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환
- 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null을 반환
- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러 발생
- Document.prototype / Element.prototype 에 정의 된 메서드로 나뉨

querySelector, querySelectorAll 메서드는 getElementById, getElementBy\*\*\* 메서드보다 다소 느림

그러나 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다는 장점이 있음.

<br>

## 특정 요소 노드를 취득할 수 있는지 확인

- Element.prototype.matches메서드 사용
- 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인
- 이벤트 위임을 사용할 때 유용

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
  <script>
    const $apple = document.querySelector(".apple");

    // $apple 노드는 '#fruits > li.apple'로 취득할 수 있다.
    console.log($apple.matches("#fruits > li.apple")); // true

    // $apple 노드는 '#fruits > li.banana'로 취득할 수 없다.
    console.log($apple.matches("#fruits > li.banana")); // false
  </script>
</html>
```

<br>

## HTMLCollection 과 NodeList

- HTMLCollection 과 NodeList는 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체(유사 배열이면서 이터러블)
- 노드 객체의 상태를 실시간으로 반영하는 `살아있는 객체`

### HTMLCollection

- HTMLCollection은 객체의 상태 변화를 실시간으로 반영
- 상태가 즉시 반영 되기 때문에 에러가 나기도 함, 이를 해결하기 위해 배열로 변환 하여 사용 권장

```html
<!DOCTYPE html>
<head>
  <style>
    .red {
      color: red;
    }
    .blue {
      color: blue;
    }
  </style>
</head>
<html>
  <body>
    <ul id="fruits">
      <li class="red">Apple</li>
      <li class="red">Banana</li>
      <li class="red">Orange</li>
    </ul>
    <script>
      // class 값이 'red'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $elems = document.getElementsByClassName("red");
      // 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있다.
      console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

      // HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경한다.
      for (let i = 0; i < $elems.length; i++) {
        $elems[i].className = "blue";
      }

      // HTMLCollection 객체의 요소가 3개에서 1개로 변경되었다.
      console.log($elems); // HTMLCollection(1) [li.red]
    </script>
  </body>
</html>
```

### NodeList

- 실시간으로 객체의 상태 변경을 반영하지 않는 객체
- NodeList.prototype.forEach 메서드를 상속 받아 사용할 수 있음
- 예외로, childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection과 같이 상태가 실시간으로 반영

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // childNodes 프로퍼티는 NodeList 객체(live)를 반환한다.
    const { childNodes } = $fruits;
    console.log(childNodes instanceof NodeList); // true

    // $fruits 요소의 자식 노드는 공백 텍스트 노드(39.3.1절 "공백 텍스트 노드" 참고)를 포함해 모두 5개다.
    console.log(childNodes); // NodeList(5) [text, li, text, li, text]

    for (let i = 0; i < childNodes.length; i++) {
      // removeChild 메서드는 $fruits 요소의 자식 노드를 DOM에서 삭제한다.
      // (39.6.9절 "노드 삭제" 참고)
      // removeChild 메서드가 호출될 때마다 NodeList 객체인 childNodes가 실시간으로 변경된다.
      // 따라서 첫 번째, 세 번째 다섯 번째 요소만 삭제된다.
      $fruits.removeChild(childNodes[i]);
    }

    // 예상과 다르게 $fruits 요소의 모든 자식 노드가 삭제되지 않는다.
    console.log(childNodes); // NodeList(2) [li, li]
  </script>
</html>
```

`노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장함.`

<br>

## 노드 정보 취득

_Node.prototype.nodeType_

- 노드 객체의 종류. 즉, 노드 타입을 나타내는 상수를 반환
- 노드 타입 상수는 Node에 정의되어 있음
- Node.ELEMENT_NODE: 요소 노드 타입을 나타내는 상수 1을 반환
- Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3을 반환
- Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9를 반환

<br>

_Node.prototype.nodeName_

- 노드의 이름을 문자열로 반환
- 요소 노드: 대문자 문자열로 태그 이름("UL", "LI" 등)을 반환
- 텍스트 노트: 문자열 "#text"를 반환
- 문서 노드: 문자열 "#document"를 반환

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    // 문서 노드의 노드 정보를 취득한다.
    console.log(document.nodeType); // 9
    console.log(document.nodeName); // #document

    // 요소 노드의 노드 정보를 취득한다.
    const $foo = document.getElementById("foo");
    console.log($foo.nodeType); // 1
    console.log($foo.nodeName); // DIV

    // 텍스트 노드의 노드 정보를 취득한다.
    const $textNode = $foo.firstChild;
    console.log($textNode.nodeType); // 3
    console.log($textNode.nodeName); // #text
  </script>
</html>
```

<br>

## 요소 노드의 텍스트 조작

### nodeValue

- getter와 setter 모두 존재하는 접근자 프로퍼티 (참조와 할당 모두 가능)
- 노드 객체의 값(텍스트 노드의 텍스트)을 반환
- 문서 노드나 요소 노드는 nodeValue 참조하면 null 반환

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    // 문서 노드의 nodeValue 프로퍼티를 참조한다.
    console.log(document.nodeValue); // null

    // 요소 노드의 nodeValue 프로퍼티를 참조한다.
    const $foo = document.getElementById("foo");
    console.log($foo.nodeValue); // null

    // 텍스트 노드의 nodeValue 프로퍼티를 참조한다.
    const $textNode = $foo.firstChild;
    console.log($textNode.nodeValue); // Hello
  </script>
</html>
```

###textContent

- 요소 노드의 텍스트와 자손 노드의 텍스트를 모두 취득
- textContent는 요소 노드의 콘텐츠 영역(시작 태그와 종료태그 사이)의 모든 텍스트를 모두 반환
- 이때 HTML 마크업은 무시됨

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // #foo 요소 노드의 텍스트를 모두 취득한다. 이때 HTML 마크업은 무시된다.
    console.log(document.getElementById("foo").textContent); // Hello world!
  </script>
</html>
```

<br>

## DOM 조작

DOM 조작은 새로운 노드를 생성하여 DOM에 추가, 삭제 교체하는 것을 말한다 => `리플로우, 리페인트 발생`

## innerHTML

- 요소 노드의 콘텐츠 영역 사이에 포함된 모든 HTML 마크업을 문자열로 반환
- innerHTML 프로퍼티에 문자열을 할당하면 요소노드의 모든 자식노드가 제거되고 할당한 문자열의 파싱되어 DOM에 반영

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // #foo 요소의 콘텐츠 영역 내의 HTML 마크업을 문자열로 취득한다.
    console.log(document.getElementById("foo").innerHTML);
    // "Hello <span>world!</span>"
  </script>
</html>
```

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.
    document.getElementById("foo").innerHTML = "Hi <span>there!</span>";
  </script>
</html>
```

innerHTML 단점

- 구현이 간단하고 직관적이긴 하나 크로스 사이트 스크립트 공격에 취약
- 모든 자식요소를 제거하고 할당하기 때문에 비효율적
- 새로운 요소를 삽입할 때 위치를 지정할 수 없음

<br>

## insertAdjacentHTML

- 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입
- 첫 번재 인수로 전달할 수 있는 문자열 (beforebegin, afterbegin, beforend, afterend)

```html
<!DOCTYPE html>
<html>
  <body>
    <!-- beforebegin -->
    <div id="foo">
      <!-- afterbegin -->
      text
      <!-- beforeend -->
    </div>
    <!-- afterend -->
  </body>
  <script>
    const $foo = document.getElementById("foo");

    $foo.insertAdjacentHTML("beforebegin", "<p>beforebegin</p>");
    $foo.insertAdjacentHTML("afterbegin", "<p>afterbegin</p>");
    $foo.insertAdjacentHTML("beforeend", "<p>beforeend</p>");
    $foo.insertAdjacentHTML("afterend", "<p>afterend</p>");
  </script>
</html>
```

# 📗배운 점

<br>

# 🤔궁금한 점
