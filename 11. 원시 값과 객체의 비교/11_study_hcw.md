# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

| 데이터 타입           | 데이터 전달 방식 |
| --------------------- | ---------------- |
| 원시 타입 (변경 불가) | 값의 전달        |
| 객체 타입 (변경 가능) | 참조의 전달      |

## 불가능한 값(immutable value)

- 원시 타입의 값 => 변경 불가능한 값(immutable value)
- **변경 불가능한 것은 변수가 아니라 값**이다.
- 원시 값을 할당한 변수에 새로운 원시 값을 재할당하면 새로운 메모리 공간을 확보하게 되고, 변수는 새롭게 재할당한 원시 값을 가리킨다. (참조하던 메모리 주소 바뀜)
- 즉, 원시 값을 갖는 변수의 값을 변경하려면 `재할당` 밖에 방법이 없음

## 값에 의한 전달

- 변수에 값이 전달되는 것이 아닌 `메모리 주소가 전달됨` => 식별자는 값이 아닌 메모리 주소를 기억 !
- 즉 값에 의한 전달도 값이 아닌 메모리 주소를 전달하는 것으로 전달된 메모리 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있다.

### 11-06

```javascript
var score = 80;
var copy = score;

console.log(score); // 80
console.log(copy); // 80

score = 100;

console.log(score); // 100
console.log(copy); // ?
```

- 변수 copy에 변수 score을 대입. 변수 score의 값인 80을 `복사`해 변수 copy에 전달한다. => `**값에 의한 전달**`
- 변수 copy와 변수 score에 들어있는 값 80은 서로 다른 메모리 공간에 저장된 별개의 값

## 변경 가능한 값(mutable value)

- **`참조 값은 생성된 객체가 저장된 메모리 주소 그 차제다.`**
- 객체는 재할당 없이 프로퍼티를 동적으로 추가, 갱신, 삭제가 가능

## 참조에 의한 전달

- 객체를 가리키는 변수를 다른 변수에 할당하면 `원본의 참조 값이 복사되어 전달`된다.

### 11-16

```javascript
var person = {
  name: "Lee",
};

// 참조값을 복사(얕은 복사)
var copy = person;
```

- 원본 person을 copy에 할당하면 `person의 참조 값을 복사`해서 copy 에 저장함 => 메모리 주소는 다르나, 동일한 참조 값을 가짐
- 즉, person과 copy는 동일한 객체를 가리킨다.
- 한 쪽 객체를 변경하면 서로 영향을 주고 받는다.

<br>

# 📗배운 점

## 데이터 타입에 의한 메모리 공간 확보

- 문자열과 숫자 타입 이외의 원시 타입은 크기를 명확히 규정하고 있지는 않음
- **문자열**은 `몇 개의 문자로 이뤄졌느냐`에 따라 메모리 공간의 크기가 달라진다 => `문자열 1개: 2바이트`, 문자열 10개: 20바이트
- **숫자 타입**은 1도, 10000도 동일하게 `8바이트` 필요

<br>
