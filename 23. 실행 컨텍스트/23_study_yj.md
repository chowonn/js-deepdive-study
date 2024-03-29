## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 23.1 소스코드의 타입

4가지 타입의 소스코드는 실행 컨텍스트를 생성

1. 전역 코드
2. 함수 코드
3. eval 코드
4. 모듈 코드

## 23.2 소스코드 평가와 실행

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/57592566-f337-4f60-9988-fc81bd6bd872)

## 23.3 실행 컨텍스트의 역할

1. 전역 코드 평가

   - 전역 코드의 변수 선언문과 함수 선언문이 먼저 실행되고, 그 결과 생성된 전역 변수와 전역 함수가 실행 컨텍스트가 관리하는 전역 스코프에 등록

2. 전역 코드 실행

   - 전역 변수에 값이 할당되고 함수가 호출

3. 함수 코드 평가

   - 함수 호출에 의해 코드 실행 순서가 변경되어 함수 내부로 진입하면 함수 코드 평가 과정을 거치며 함수 코드를 실행하기 위한 준비
   - 매개변수와 지역 변수 선언문이 먼저 실행, 생성된 매개변수와 지역 변수가 실행 컨텍스트가 관리하는 지역 스코프에 등록

4. 함수 코드 실행
   - 함수 코드 평가 과정이 끝나면 런타임이 시작되어 함수 코드가 순차적으로 실행

코드가 실행되려면 다음과 같이 스코프, 식별자, 코드 실행 순서 등의 관리가 필요

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/f6524021-f341-4cf7-945a-8339b9b13992)

실행 컨텍스트는 식별자(변수, 함수, 클래스 등의 이름)를 등록하고 관리하는 스코프와 코드 실행 순서 관리를 구현한 내부 메커니즘으로, 모든 코드는 실행 컨텍스트를 통해 실행되고 관리

## 23.4 실행 컨텍스트 스택

```javascript
const x = 1;

function foo() {
  const y = 2;

  function bar() {
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo(); // 6
```

- 생성된 실행 컨텍스트는 스택 자료구조로 관리, 이를 실행 컨텍스트 스택이라 부름

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/10ee3b35-c664-4260-bede-a40eb563ac85)

1. 전역 코드의 평가와 실행

   - 전역 코드를 평가하여 전역 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시

2. foo 함수 코드의 평가와 실행

   - foo 함수 내부의 함수 코드를 평가하여 foo 함수 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시

3. bar 함수 코드의 평가와 실행

   - 중첩 함수 bar가 호출되면 foo 함수 코드의 실행은 일시 중단되고 코드의 제어권이 bar 함수 내부로 이동
   - bar 함수 내부의 함수 코드를 평가하여 실행 컨텍스트 스택에 푸시

4. foo 함수로의 복귀

   - bar 함수가 종료되면 코드의 제어권은 다시 foo 함수로 이동
   - 자바스크립트 엔진은 bar 함수 실행 컨텍스트를 실행 컨텍스트에서 pop하여 제거

5. 전역 코드로 복귀
   - foo 함수가 종료되면 코드의 제어권은 다시 전역 코드로 이동
   - 자바스크립트 엔진은 foo 함수 실행 컨텍스트를 실행 컨텍스트에서 pop하여 제거

실행 컨텍스트 스택은 코드의 실행 순서를 관리, 최상위에 존대하는 실행 컨텍스트는 언제나 현재 실행 중인 코드의 실행 컨텍스트

## 23.5 렉시컬 환경

렉시컬 환경은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트

실행 컨텍스트 스택이 코드의 실행 순서를 관리한다면 렉시컬 환경은 스코프와 식별자를 관리

렉시컬 환경은 스코프를 구분하여 식별자를 등록하고 관리하는 저장소 역할을 하는 렉시컬 스코프의 실체

## 23.6 실행 컨텍스트 생성과 식별자 검색 과정

```javascript
var x = 1;
const y = 2;

function foo (a) {
...
```

### 23.6.1 전역 객체 생성

전역 객체는 전역 코드가 평가되기 이전에 생성

전역 객체에는 빌트인 전역 프로퍼티와 빌트인 전역 함수, 그리고 표준 필트인 객체가 추가되며 동작 환경(클라이언트 사이드 또는 서버 사이드)에 따라 클라이언트 사이드 Web API 또는 특정 환경을 위한 호스트 객체를 포함

### 23.6.2 전역 코드 평가

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/82ee162c-3cd1-4ef5-82be-6c960a70ac73)

위 순서로 생성된 전역 실행 컨텍스트와 렉시컬 환경

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/47cc2105-6bea-4d50-ab7a-92addfd296ec)

1.  전역 실행 컨텍스트 생성

    - 비어있는 전역 실행 컨텍스트를 생성하여 실행 컨텍스트 스택에 푸시  
      ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/6474ace9-04ad-4369-85ed-554bf36342fe)

2.  전역 렉시컬 환경 생성

    - 전역 렉시컬 환경을 생성하고 전역 실행 컨텍스트에 바인딩  
      ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/52e05f08-668c-4947-afaf-a38ec5946e29)

          2.1. 전역 환경 레코드 구성

    - 전역 환경 레코드는 객체 환경 레코드와 선언적 환경 레코드로 구성

      2.1.1. 객체 환경 레코드 구성

      - 전역 코드 평가 과정에서 var 키워드로 선언한 전역 변수와 함수 선언문으로 정의된 전역 함수는 전역 환경 레코드의 객체 환경 레코드에 연결된 BindingObject를 통해 전역 객체의 프로퍼티와 메서드가 됨
      - 전역 코드 평가 시점에 전역 객체에 변수 식별자를 키로 등록한 다음, 암묵적으로 undefined를 바인딩  
        ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/038366e7-3762-4364-85a6-ba8e0da02fdf)

        2.1.2. 선언적 환경 레코드 선언

      - var 키워드로 선언한 전역 변수와 함수 선언문으로 정의한 전역 함수 이외의 선언, 즉 let, const 키워드로 선언한 전역 변수는 선언적 환경 레코드에 등록되고 관리
      - const 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행
      - 즉, 런타임에 실행 흐름이 변수 선언문에 도달하기 전까지 일시적 사각지대에 빠지게 됨  
        ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/1473ee83-3363-4e54-8937-a483b25ee3ff)

      2.2. this 바인딩

    - this 바인딩은 전역 환경 레코드와 함수 환경 레코드에만 존재

      2.3. 외부 렉시컬 환경에 대한 참조 결정

    - 외부 렉시컬 환경에 대한 참조는 현재 평가 중인 소스코드를 포함하는 외부 소스코드의 렉시컬 환경, 즉 상위 스코프를 가리킴

### 23.6.3 전역 코드 실행

전역 코드가 순차적으로 실행, 변수 할당문이 실행되어 전역 변수 x, y에 값이 할당, foo 함수 호출

변수 할당문 또는 함수 호출문을 실행하려면 먼저 변수 또는 함수 이름이 선언된 식별자인지 확인해야 함

동일한 이름의 식별자가 다른 스코프에 여러 개 존재할 수 있기 때문에 어느 스코프의 식별자를 참조하면 되는지 결정할 필요가 있음, 이를 식별자 결정이라 함

식별자 결정을 위해 식별자를 검색할 때는 실행 중인 실행 컨텍스트에서 식별자 검색

### 23.6.4 foo 함수 코드 평가

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/6d8eb675-beae-409f-81b6-a71de12d691d)

1. 함수 실행 컨텍스트 생성

   - 함수 실행 컨텍스트는 실행 컨텍스트 스택의 최상위, 즉 실행 중인 실행 컨텍스트가 됨

2. 함수 렉시컬 환경 생성

   - foo 함수 렉시컬 환경을 생성하고 foo 함수 실행 컨텍스트에 바인딩

     2.1. 함수 환경 레코드 생성

   - 매개변수, arguments 객체, 함수 내부에서 선언한 지역 변수와 중첩 함수를 등록하고 관리

     2.2. this 바인딩

     2.3. 외부 렉시컬 환경에 대한 참조 결정

### 23.6.5 foo 함수 코드 실행

매개 변수에 인수가 할당되고, 변수 할당문이 실행되어 지역 변수 x, y에 값이 할당, 함수 bar 호출

### 23.6.6 bar 함수 코드 평가

bar 함수가 호출되면 bar 함수 내부로 코드의 제어권 이동

### 23.6.7 bar 함수 코드 실행

매개변수에 인수가 할당되고, 변수 할당문이 실행되어 지역 변수 z에 값이 할당

그리고 `console.log(a + b + x + y + z);` 실행

1. console 식별자 검색
2. log 메서드 검색
3. 표현식 `a + b + x + y + z`의 평가

   - a, b, x, y, z 식별자를 검색

4. console.log 메서드 호출

### 23.6.8 bar 함수 코드 실행 종료

실행 컨텍스트 스택에서 bar 함수 실행 컨텍스트 제거

만약 bar 함수 렉시컬 환경을 누군가 참조하고 있다면 bar 함수 렉시컬 환경은 소멸하지 않음

### 23.6.9 foo 함수 코드 실행 종료

실행 컨텍스트 스택에서 foo 함수 실행 컨텍스트가 pop되어 제거되고 전역 실행 컨텍스트가 실행 중인 실행 컨텍스트가 됨

### 23.6.10 전역 코드 실행 종료

foo 함수가 종료되면 더는 실행할 전역 코드가 없으므로 전역 코드의 실행이 종료되고 전역 실행 컨텍스트도 실행 컨텍스트 스택에서 팝되어 실행 컨텍스트 스택에는 아무것도 남아있지 않음

## 23.7 실행 컨텍스트와 블록 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만 지역 스코프로 인정하는 함수 레벨 스코프를 따름

let, const 키워드로 선언한 변수는 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따름

```javascript
let x = 1;

if (true) {
  let x = 10;
  console.log(x); // 10
}

console.log(x); // 1
```

- if 문의 코드 블록이 실행되면 if 문의 코드 블록을 위한 블록 레벨 스코프를 생성
- 이를 위해 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성하여 기존의 전역 렉시컬 환경을 교체  
  ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/70d19386-a902-4810-9f7b-a4c0517d6e39)  
  ![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/b356decd-7574-43b7-9949-293ecac41ee3)
- 이는 if 문뿐 아니라 블록 레벨 스코프를 생성하는 모든 블록문에 적용

## 🤔궁금한 점

## 📌중요한 점
