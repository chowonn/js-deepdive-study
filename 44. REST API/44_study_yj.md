## 목차

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

[📌중요한 점](#📌중요한-점)

## 📗배운 점

## 44. REST API

REST는 HTTP를 기반으로 클리이언트 서버의 리소스에 접근하는 방식을 규정한 아키텍처

REST AIP는 REST를 기반으로 서비스 API를 구현한 것

## 44.1 REST API의 구성

REST API는 자원, 행위, 표현의 3가지 요소로 구성

![image](https://github.com/jin62413/js-deepdive-study/assets/71061884/28695c04-bbd3-4848-86a1-51dc6f65b8e4)

## 44.2 REST API 설계 원칙

1. URI는 리소스를 표현해야 함
   - 리소스를 식별할 수 있느 ㄴ이름은 동사보다는 명사를 사용
   - 이름에 get 같은 행위에 대한 표현이 들어가서는 안 됨
2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현
   - HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법
   - 주로 5가지 요청 메서드(GET, POST, PUT, PATCH, DELETE 등)를 사용하여 CRUD를 구현

## 44.5 JSON Server를 이용한 REST API 실습

### 44.3.4 GET 요청

리소스에서 모든 데이터를 취득

### 44.3.5 POST 요청

리소스에 새로운 데이터 생성

### 44.3.6 PUT 요청

특정 리소스 전체를 교체

### 44.3.7 PATCH 요청

특정 리소스의 일부를 수정

### 44.3.8 DELETE 요청

리소스에서 id를 사용하여 데이터 삭제

## 🤔궁금한 점

## 📌중요한 점