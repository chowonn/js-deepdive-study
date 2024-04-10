# 📗챕터 정리

## 배열 디스트럭처링 할당

구조화된 배열 등 이터러블이나 객체를 **디스트릭처링(destructuring, 비구조화, 구조 파괴)**하여 1개 이상의 변수에 개별적으로 할당하는 것을 의미한다.

```jsx
const arr = [1, 2, 3];

const [one, two, three] = arr;

console.log(one, two, three); // 1 2 3
```

배열 디스트럭처링 할당 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스(Index)이다.

따라서 앞에서부터 순서대로 할당되는데, 이 때 변수의 개수와 이터러블의 요소 개수가 일치할 필요는 없다.

```jsx
const [a, b] = [1, 2];
console.log(a, b); // 1 2

const [c, d] = [1];
console.log(c, d); // 1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); // 1 2

const [g, , h] = [1, 2, 3];
console.log(g, h); // 1 3
```

배열 디스트럭처링 할당을 위한 변수에는 기본값을 설정할 수 있다.

변수에 할당되는 값이 있다면 기본값보다 우선적으로 할당된다.

```jsx
const [a, b, c = 3] = [1, 2];
console.log(a, b, c); // 1 2 3

const [e, f = 10, g = 3] = [1, 2];
console.log(e, f, g); // 1 2 3
```

<aside>
❓ URL 파싱

---

배열 디스트럭처링 할당을 사용하여 필요한 요소만 추출한 후, 변수에 할당할 수 있다.

예를 들어 URL 주소를 파싱하여 protocol, host, path 프로퍼티를 갖는 객체를 생성해 반환한다.

```jsx
function parseURL(url = '') {
  // '://' 앞의 문자열(protocol)과 '/' 이전의 '/'으로 시작하지 않는 문자열(host)과 '/' 이후의 문자열(path)을 검색한다.
  const parsedURL = url.match(/^(\w+):\/\/([^/]+)\/(.*)$/);
  console.log(parsedURL);
  /*
  [
    'https://developer.mozilla.org/ko/docs/Web/JavaScript',
    'https',
    'developer.mozilla.org',
    'ko/docs/Web/JavaScript',
    index: 0,
    input: 'https://developer.mozilla.org/ko/docs/Web/JavaScript',
    groups: undefined
  ]
  */

  if (!parsedURL) return {};

  // 배열 디스트럭처링 할당을 사용하여 이터러블에서 필요한 요소만 추출한다.
  const [, protocol, host, path] = parsedURL;
  return { protocol, host, path };
}

const parsedURL = parseURL('https://developer.mozilla.org/ko/docs/Web/JavaScript');
console.log(parsedURL);
/*
{
  protocol: 'https',
  host: 'developer.mozilla.org',
  path: 'ko/docs/Web/JavaScript'
}
*/
```

</aside>

---

## 객체 디스트럭처링 할당

객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당하는 것을 의미한다.

이 때, 객체 디스트럭처링 할당의 대상(할당문의 우변)은 객체여야 하며, 할당 기준은 프로퍼티 키다.

```jsx
const user = { firstName: 'Ungmo', lastName: 'Lee' };

const { lastName, firstName } = user;

console.log(firstName, lastName); // Ungmo Lee
```

객체 디스트럭처링 할당으로 선언한 변수는 프로퍼티 축약 표현을 통해 선언하는 것이다.

```jsx
const { lastName, firstName } = user;
// 위와 아래는 동치다.
const { lastName: lastName, firstName: firstName } = user;
```

따라서, 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당 받으려면 다음과 같이 변수를 선언한다.

```jsx
const user = { firstName: 'Ungmo', lastName: 'Lee' };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다.
// 프로퍼티 키가 lastName인 프로퍼티 값을 ln에 할당하고,
// 프로퍼티 키가 firstName인 프로퍼티 값을 fn에 할당한다.
const { lastName: ln, firstName: fn } = user;

console.log(fn, ln); // Ungmo Lee
```

배열 디스트럭처링 할당문과 마찬가지로 변수에 기본 값을 설정할 수 있다.

```jsx
const { firstName = 'Ungmo', lastName } = { lastName: 'Lee' };
console.log(firstName, lastName); // Ungmo Lee

const { firstName: fn = 'Ungmo', lastName: ln } = { lastName: 'Lee' };
console.log(fn, ln); // Ungmo Lee
```

### ⭐ 객체 디스트럭처링의 응용

1. 객체 디스트럭처링 할당은 객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용하게 사용할 수 있다.
    
    ```jsx
    const str = 'Hello';
    // String 래퍼 객체로부터 length 프로퍼티만 추출한다.
    const { length } = str;
    console.log(length); // 5
    
    const todo = { id: 1, content: 'HTML', completed: true };
    // todo 객체로부터 id 프로퍼티만 추출한다.
    const { id } = todo;
    console.log(id); // 1
    ```
    
2. 객체 디스트럭처링 할당은 객체를 인수로 전달 받는 함수의 매개변수에도 사용할 수 있다.
    
    ```jsx
    function printTodo(todo) {
      console.log(`할일 ${todo.content}은 ${todo.completed ? '완료' : '비완료'} 상태입니다.`);
    }
    
    printTodo({ id: 1, content: 'HTML', completed: true });
    ```
    
    위 예제에서 객체를 인수로 전달받는 매개변수 `todo`에 객체 디스트럭처링 할당을 사용하여 더 간단하고 가독성 좋은 코드를 작성할 수 있다.
    
    ```jsx
    function printTodo({ content, completed }) {
      console.log(`할일 ${content}은 ${completed ? '완료' : '비완료'} 상태입니다.`);
    }
    
    printTodo({ id: 1, content: 'HTML', completed: true });
    ```
    
3. 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다.
    
    ```jsx
    const todos = [
      { id: 1, content: 'HTML', completed: true },
      { id: 2, content: 'CSS', completed: false },
      { id: 3, content: 'JS', completed: false }
    ];
    
    // todos 배열의 두 번째 요소인 객체로부터 id 프로퍼티만 추출한다.
    const [, { id }] = todos;
    console.log(id); // 2
    ```
    
4. 중첩 객체로 객체 디스트럭처링을 사용할 때는 다음 예제처럼 사용할 수 있다.
    
    ```jsx
    const user = {
      name: 'Lee',
      address: {
        zipCode: '03068',
        city: 'Seoul'
      }
    };
    
    // address 프로퍼티 키로 객체를 추출하고 이 객체의 city 프로퍼티 키로 값을 추출한다.
    const { address: { city } } = user;
    console.log(city); // 'Seoul'
    ```
    
5. Rest 파라미터(Rest 요소)와 유사하게 Rest 프로퍼티를 사용할 수 있다.
    
    Rest 프로퍼티는 반드시 마지막에 위치해야 한다.
    
    ```jsx
    const { x, ...rest } = { x: 1, y: 2, z: 3 };
    console.log(x, rest); // 1 { y: 2, z: 3 }
    ```

<br/>

## 🤔궁금한 점

## 📌중요한 점
