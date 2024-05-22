## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 43. Ajax

## 43.1 Ajax란?

자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식

장점

1. 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않음
2. 변경할 필요가 없는 부분은 다시 렌더링하지 않음. 따라서 화면이 순간적으로 깜빡이는 현상이 발생하지 않음
3. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않음

## 43.2 JSON

클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷(자바스크립트에 종속되지 않음)

### 43.2.1 JSON 표기 방식

```json
{
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": ["traveling", "tennis"]
}
```

- JSON의 키는 반드시 큰따옴표(작은따옴표 사용 불가)로 묶어야 함
- 값은 객체 리터럴과 같은 표기법을 그대로 사용
- 하지만 문자열은 반드시 큰따옴표로 묶어야 함

### 43.2.2 JSON.stringify

```javascript
const obj = {
  name: "Lee",
  age: 20,
  alive: true,
  hobby: ["traveling", "tennis"],
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 한다.
const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);
/*
string {
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/

// replacer 함수. 값의 타입이 Number이면 필터링되어 반환되지 않는다.
function filter(key, value) {
  // undefined: 반환하지 않음
  return typeof value === "number" ? undefined : value;
}

// JSON.stringify 메서드에 두 번째 인수로 replacer 함수를 전달한다.
const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);
/*
string {
  "name": "Lee",
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/
```

- JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화라 함

```javascript
const todos = [
  { id: 1, content: "HTML", completed: false },
  { id: 2, content: "CSS", completed: true },
  { id: 3, content: "Javascript", completed: false },
];

// 배열을 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(todos, null, 2);
console.log(typeof json, json);
/*
string [
  {
    "id": 1,
    "content": "HTML",
    "completed": false
  },
  {
    "id": 2,
    "content": "CSS",
    "completed": true
  },
  {
    "id": 3,
    "content": "Javascript",
    "completed": false
  }
]
*/
```

- 배열도 가능

### 43.2.3 JSON.parse

```javascript
const obj = {
  name: "Lee",
  age: 20,
  alive: true,
  hobby: ["traveling", "tennis"],
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);

// JSON 포맷의 문자열을 객체로 변환한다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {name: "Lee", age: 20, alive: true, hobby: ["traveling", "tennis"]}
```

- JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환
- 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열
- 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해아 하는데 이를 역직렬화라 함

```javascript
const todos = [
  { id: 1, content: "HTML", completed: false },
  { id: 2, content: "CSS", completed: true },
  { id: 3, content: "Javascript", completed: false },
];

// 배열을 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(todos);

// JSON 포맷의 문자열을 배열로 변환한다. 배열의 요소까지 객체로 변환된다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
/*
 object [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false }
]
*/
```

- 배열도 가능
- 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환

## 43.3 XMLHttpRequest

자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용

### 43.3.1 XMLHttpRequest 객체 생성

```javascript
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();
```

- XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성
- XMLHttpRequest 객체는 브라우저에서 제공하는 Web APO이므로 브라우저 환경에서만 정상적으로 실행

### 43.3.3 HTTP 요청 전송

HTTP 요청을 전송하는 순서

1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송

```javascript
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// 서버에 전송할 HTTP 요청 초기화
xhr.open("GET", "/users");

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader("content-type", "application/json");

// HTTP 요청 전송
xhr.send();
```

- `xhr.open(method, url[, async])`
  - open 메서드는 서버에 전송할 HTTP 요청을 초기화
  - HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법
  - 주로 5가지 요청 메서드(GET, POST, PUT, PATCH, DELETE 등)을 사용하여 CRUD를 구현
- `xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));`
  - send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송
  - 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있음
  - send 메서드에는 요청 몸체에 담아 전송할 데이터(페이로드)를 인수로 전달할 수 있음
  - 페이로드가 객체인 경우 반드시 JSON.stringify 메서드를 사용하여 직렬화한 다음 전달
  - HTTP 요청 메서드가 GET인 경우 send 메서드에 페이로드로 전달한 인수는 무시되고 요청 몸체에는 null로 설정
- setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정
  - 자주 사용하는 HTTP 요청 헤더인 Content-type과 Accept
    - Content-type은 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현
    - HTTP 클라이언트가 서버에 요청할 때 서버가 응답할 데이터의 MIME 타입을 Accept로 지정할 수 있음

### 43.3.4 HTTP 응답 처리

서버가 전송할 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 함

```javascript
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가
// 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
  // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
  // readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 상태다.
  // 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리를 하지 않는다.
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```

- readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가 변경될 때마다 발생
- onreadystatechange 이벤트 핸들러 프로퍼티에 할당한 이벤트 핸들러는 HTTP 요청의 현재 상태를 나타내는 xhr.readyState가 XMLHttpRequest.DONE인지 확인하여 서버의 응답이 완료되었는지 확인
- 서버의 응답이 완료되면 xhr.status가 200인지 확인하여 정상 처리와 에러 처리를 구분

```javascript
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

// HTTP 요청 전송
xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```

- load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생
- load 이벤트를 캐치하는 경우 xhr.readyState가 XMLHttpRequest.DONE인지 확인할 필요 없음

## 🤔궁금한 점

## 📌중요한 점
