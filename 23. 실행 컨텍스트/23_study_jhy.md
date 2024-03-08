# 목차

[📗배운 점](#📗배운-점)

[📌중요한 점](#📌중요한-점)

[🤔궁금한 점](#🤔궁금한-점)

## 📗배운 점

실행 컨텍스트는 자바스크립트의 동작 원리를 담고 있는 핵심 개념

### 23.1 소스코드의 타입

ECMAScript 소스코드 4가지 타입으로 구분 = 실행 컨텍스트 생성
![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/bee0c6cd-45b9-4835-8111-454434f85ce8)

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/438ff323-87fe-46b0-b392-7fc8c1a2df53)

### 23.2 소스코드의 평가와 실행

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/1dfe95f8-2f97-4bc3-9caa-550dd0672780)

```
var x;
x= 1
```

- 자바스크립트 엔진은 위 예제를 2개 과정으로 처리

1. 소스코드 평과 과정: 변수 선언문 먼저 실행(생성된 변수 식별자 x는 실행 컨텍스트가 관리하는 스코프레 등록되고 undefined로 초기화)

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/caa1e3fc-682c-41cc-91c7-bec61cc82128)

2. 소스코드 평가 과정이 끝나면 소스코드 실행과정이 시작.

- 소스코드 실행과정에서는 변수 할당문 x =1만 실행
- x변수가 선어된 변수인지 홗인
- x변수가 실행 컨텍스트가 관리하는 스코프에 등록되어 있다면 소스코드 평과과정에서 선언문이 실행되어 등록된 변수
  ![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/ec8ec849-009c-4919-9575-03b291b7e345)

### 23.3 실행 컨텍스트의 역할

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/9126076c-5cae-44a4-817b-d68001df16cb)

1. 전역코드 평가
2. 전역 코드 실행
3. 함수 코드 평가
4. 함수 코드 실행

- 코드가 실행되려면 스코프, 식별자 코드 실행 순서 관리가 필요함.

  1. 선언에 의해 생성된 모든 식별자를 스코프를 구분하여 등록하고 상태변화를 지속적으로 관리
  2. 스코프는 중첩관계에 의해 스코프체인을 형성해야함.
  3. 현재 실행 중인 코드의 실행순서를 변경 할 수 있어야하며 다시 되돌아갈 수도 있어야한다.

- 실행 컨텍스트는 소스코드를 실행하는 데 필요한 환ㄱ셩을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.
- 실행 컨텍스트는 식별자(변수,함수,클래스 등의 이름)을 등록하고 솬리하는 스코프와 코드 실행 순서관리를 구현한 내부 메커니즘으로, 모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.

### 23.4 실행 컨텍스트 스택

스택 자료구조로 관리되는 실행 컨텍스트 스택

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/4f8bd06a-9afe-487c-b55e-cc83410e5863)

- 실행 컨텍스 스택의 최상위 존대하는 실행 컨텍스트는 언제나 현재 실행 중인 코드의 실행 컨텍스트다.

### 23.5 렉시컬 환경

식별자와 실별자에 바인딩된값, 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트다.

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/4f8bd06a-9afe-487c-b55e-cc83410e5863)

- 렉시컬 환경은 스코프를 구분하여 식별자를 등록하고 관리하는 저장 소 역할을 한다.

### 23.6 실행 컨텍스트의 생성과 실별자 검색 과정

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/c37177ac-5eac-44dc-98ea-f376b96b2f4c)

#### 23.6.1 전역 객체 생성

#### 23.6.2 전역 코드 평가

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/47704808-76dd-4d9f-af8b-be99e20b3ffa)

1. 전역 실행 컨텍스트 생성
   ![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/49358796-e7a1-4a4c-9e39-922a11bffb5b)

2. 전역 렉시털 환경 설정
   전역 렉시컬 환경을 생성하고 전역실행 컨텍스트에 바인딩한다.
   ![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/1f7737b8-89ef-473e-915a-73576d84725a)

2-1 전역 환경 레코드 생성
2.1.1 객체 환경 레코드 생성
![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/2827f798-2cda-4f88-8b3c-f9e1c78b519a)

    2.1.2 선연적 환경 레코드 생성
    ![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/21fb8d6a-20b2-4b4f-9059-f0f04ad18a18)

2-2 this 바인딩
![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/62ee43e1-d6cb-46bc-bedf-9cc374b11f1d)

2-3 외부 렉시컬 환경에 대한 참조 설정
![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/195abc68-9ae0-476d-a5ac-bdcf3e419c5d)

#### 23.6.3 전역 코드 실행

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/bc35d363-3b60-4117-b2f1-2a7ba1c3d0e5)

- 식별자 결정을 위해 식별자를 검색할때는 실행 중인 실행 컨텍스트에서 식별자를 검색하기 시작함

#### 23.6.4 foo 함수 코드 평가

함수를 어디서 호출했는지가 아니라 어디에 정의했는지에 따라 상위 스크포를 결정한다.

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/32b5d270-e7bc-4eba-af64-edd55f07322e)

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
   ![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/031818da-0be4-420a-88cc-bc2c102e0188)

#### 23.6.5 foo 함수 코드 실행

식별자 결정을 위해 실행중인 실행 컨텍스트의 렉시컬 환경에서 식별자를 검색하기 시작.

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/ba95d15a-26d6-4524-8216-c326cbd05fab)

#### 23.6.6 bar 함수 코드 평가

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/5bb9587d-eccb-4a66-9695-e637d43dea7f)

#### 23.6.7 bar 함수 코드 실행

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/6fff83a4-9c6e-4209-930a-5681fe1e86ad)

#### 23.6.8 bar 함수 코드 실행 종료

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/a1635e69-dde9-41ca-8c61-96c378cb85e9)

#### 23.6.9 foo 함수 코드 실행 종료

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/41f8b27c-ebc4-4ac0-a54e-53968a6a8f3b)

#### 23.6.10 전역 코드 실행 종료

### 23.7 실행 컨텍스트와 블록 레벨 스코프

- let, const 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프라고 부른다.
- ![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/34b18672-574a-4f73-a98c-4099b2240659)
- ig문의 코드 블록을 위핸 블록 레벨 스코르를 생성
- 이를 위해 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성, 기존 전역 렉시컬 환경을 교체
- 새롭게 생성된 렉시컬 환경의 외부 렉시컬 환경에 대한 참조는 if문이 실행되기 이전의 전역 렉시컬 환경을 가리킴.

![image](https://github.com/chowonn/js-deepdive-study/assets/101866872/7b71214c-2af5-4d5a-84a3-cd0d37ec0d31)

- if문 뿐 아니라 블록레벨 스코프를 생성하는 모든 블록문에 적용

- for문 반복해서 실행될 때마다 코드 블록을 위한 새로운 렉시컬 환경 생성
- for 문의 코드 블록이 반복해서 실행될 때마다 독립적인 렉시컬 환경을 갱성하여 식별자 값을 유지 => 클로저에서 자세히 살펴보자.
