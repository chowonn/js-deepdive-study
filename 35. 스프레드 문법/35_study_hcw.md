# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

# spread syntax

하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든 것

스프레드 문법을 사용할 수 있는 대상은 Array, String, Map, Set, Dom 컬렉션, arguments와 같이 for...of 문으로 순회할 수 있는 이터러블에 한정됨

```javascript
// ...[1, 2, 3]은 [1, 2, 3]을 개별 요소로 분리한다(→ 1, 2, 3)
console.log(...[1, 2, 3]); // 1 2 3

// 문자열은 이터러블이다.
console.log(..."Hello"); // H e l l o

// Map과 Set은 이터러블이다.
console.log(
  ...new Map([
    ["a", "1"],
    ["b", "2"],
  ])
); // [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Set([1, 2, 3])); // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다.
console.log(...{ a: 1, b: 2 });
// TypeError: Found non-callable @@iterator
```

<br>

# 📗배운 점

<br>

# 🤔궁금한 점
