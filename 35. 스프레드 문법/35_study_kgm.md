# 📗챕터 정리

## 스프레드 문법

> ***Spread Syntax***
> 

스프레드 문법은 자바스크립트 ES6 버전에서 도입된 문법이다.

여러 값의 집합을 펼쳐(전개, 분산, spread) 개별적인 값의 목록을 추출한다.

스프레드 문법은 `for … of` 문으로 순회할 수 있는 이터러블에만 사용 가능하다.

```jsx
console.log(...[1, 2, 3]); // 1 2 3
console.log(...'Hello'); // H e l l o

console.log(...{ a: 1, b: 2 }); // TypeError: Found non-callable @@iterator
```

위의 첫 번째 코드에서 `…[1, 2, 3]` 은 배열을 펼치고 내부의 요소들을 개별 값의 목록 `1 2 3` 으로 만든다.

`console.log` 코드를 통해 출력되는 결과인 `1 2 3`은 값이 아닌 목록이다.

따라서 스프레드 문법의 결과는 변수에 할당할 수 없다.

```jsx
const list = ...[1, 2, 3]; // SyntaxError: Unexpected token ...
```

---

## 스프레드 문법의 활용

스프레드 문법은 아래와 같이 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용할 수 있다.

### 1️⃣ 함수 호출문의 인수 목록

함수 호출문을 사용할 때, 배열 요소의 목록만을 인수로 전달해야 하는 경우가 있다.

예를 들어, Math.max 메서드는 숫자가 아닌 배열을 인수로 반환하면 최댓값을 구할 수 없어 `NaN`을 반환한다.

이 때, 스프레드 문법을 사용하면 메서드가 정상적인 결과를 출력할 수 있다.

```jsx
const arr = [1, 2, 3];

console.log(Math.max(arr)); // NaN
console.log(Math.max(...arr)); // 3
```

함수 호출문에서 스프레드 문법을 사용할 때, Rest 파라미터와 형태가 동일하여 혼동할 수 있으니 유의해야 한다.

 Rest 파라미터 는 함수에 전달된 인수의 목록을 배열로 전달받기 위해 사용하는 것이다.

반면  스프레드 문법 은 여러 값이 뭉쳐진 배열 등의 요소를 펼쳐서 값 각각을 목록으로 만드는 것이다.

따라서  Rest 파라미터 와  스프레드 문법 은 서로 반대의 개념이다.

```jsx
// Rest 파라미터는 인수들의 목록을 배열로 전달받는다.
function foo(...rest) {
  console.log(rest); // 1, 2, 3 -> [ 1, 2, 3 ]
}

// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
// [1, 2, 3] -> 1, 2, 3
foo(...[1, 2, 3]);
```

### 2️⃣ 배열 리터럴의 요소 목록

스프레드 문법은 배열의 결합, 수정, 복사 등을 간결하고 가독성 좋게 표현하기 위해 사용할 수 있다.

1. **배열 간의 결합**
    - 스프레드 문법을 사용하면 배열 리터럴만으로 배열 결합이 가능해졌다.
        
        ```jsx
        // ES6
        const arr2 = [...[1, 2], ...[3, 4]];
        console.log(arr2); // [1, 2, 3, 4]
        ```
        
    - ES6 이전
        
        배열 결합을 위해 `concat` 메서드를 사용하였다.
        
        ```jsx
        // ES5
        var arr1 = [1, 2].concat([3, 4]);
        console.log(arr1); // [1, 2, 3, 4]
        ```
        
2. **배열 수정 (배열을 요소로 추가/제거)**
    - 스프레드 문법으로 더 간결하고 가독성 좋게 배열을 수정할 수 있다.
        
        ```jsx
        // ES6 (Spread Syntax)
        const arr1 = [1, 4];
        const arr2 = [2, 3];
        
        arr1.splice(1, 0, ...arr2);
        console.log(arr1); // [1, 2, 3, 4]
        ```
        
    - ES6 이전
        
        `splice` 메서드를 사용할 때, 세 번째 인수로 배열을 전달하면 그 배열 자체가 추가된다.
        
        ```jsx
        // ES5
        var arr1 = [1, 4];
        var arr2 = [2, 3];
        
        arr1.splice(1, 0, arr2);
        console.log(arr1); // [1, [2, 3], 4]
        
        ```
        
        때문에 `splice` 메서드로 대상 배열에 다른 배열 요소를 추가하려면 아래의 복잡한 방법을 사용했다.
        
        ```jsx
        // ES5
        var arr1 = [1, 4];
        var arr2 = [2, 3];
        
        Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
        console.log(arr1); // [1, 2, 3, 4]
        ```
        
3. **배열 복사 (얕은 복사)**
    - 스프레드 문법으로 각 요소를 복사하여 새로운 배열로 복사할 수 있다.
        
        ```jsx
        // ES6
        const origin = [1, 2];
        const copy = [...origin];
        
        console.log(copy); // [1, 2]
        console.log(copy === origin); // false
        ```
        
    - ES6 이전
        
        배열을 복사할 때 `slice` 메서드를 사용하였다.
        
        ```jsx
        // ES5
        var origin = [1, 2];
        var copy = origin.slice();
        
        console.log(copy); // [1, 2]
        console.log(copy === origin); // false
        ```
        
4. 이터러블을 배열로 변환
    - 이터러블을 간편하게 배열로 변환할 수 있다.
        
        *ex*. `arguments` 객체는 이터러블이면서 유사 배열 객체로, 스프레드 문법을 사용할 수 있다.
        
        ```jsx
        function sum() {
          return [...arguments].reduce((pre, cur) => pre + cur, 0);
        }
        
        console.log(sum(1, 2, 3)); // 6
        ```
        
        다만, 위 경우에는 Rest 파라미터를 사용하는 것이 더 좋은 방법일 수 있다.
        
        ```jsx
        const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);
        
        console.log(sum(1, 2, 3)); // 6
        ```
        
    - 이터러블이 아닌 유사 배열 객체에는 스프레드 문법을 사용할 수 없다.
        
        이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 `Array.from` 메서드를 사용하면 된다.
        
        `Array.from` 메서드는 유사 배열 객체나 이터러블을 인수로 전달 받아 배열로 변환하고 반환한다.
        
        ```jsx
        const arrayLike = {
          0: 1,
          1: 2,
          2: 3,
          length: 3
        };
        
        const arr = Array.from(arrayLike); // -> [1, 2, 3]
        ```
        
        - ES6 이전
            
            `Function.prototype.apply` / `Function.prototype.call` 메서드를 사용하여 이터러블이 아닌 유사 배열 객체를 배열로 변경하였다.
            
            ```jsx
            const arrayLike = {
              0: 1,
              1: 2,
              2: 3,
              length: 3
            };
            
            const arr1 = Array.prototype.slice.call(arrayLike); // -> [1, 2, 3]
            console.log(Array.isArray(arr1)); // true
            
            const arr2 = [...arrayLike];
            // TypeError: object is not iterable
            // (cannot read property Symbol(Symbol.iterator))
            ```
            

### 3️⃣ 객체 리터럴의 프로퍼티 목록

객체 리터럴의 프로퍼티 목록에도 스프레드 문법을 사용할 수 있는데, 이를 스프레드 프로퍼티라고 한다.

스프레드 프로퍼티는 Rest 프로퍼티와 함께 2021년 9월에 제안되었다.

1. 객체 병합 (프로퍼티가 중복되는 경우, 뒤에 위치한 프로퍼티가 우선권을 갖는다.)
    
    ```jsx
    const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
    console.log(merged); // { x: 1, y: 10, z: 3 }
    ```
    
2. 특정 프로퍼티 변경
    
    ```jsx
    const changed = { ...{ x: 1, y: 2 }, y: 100 };
    // changed = { ...{ x: 1, y: 2 }, ...{ y: 100 } }
    console.log(changed); // { x: 1, y: 100 }
    ```
    
3. 프로퍼티 추가
    
    ```jsx
    const added = { ...{ x: 1, y: 2 }, z: 0 };
    // added = { ...{ x: 1, y: 2 }, ...{ z: 0 } }
    console.log(added); // { x: 1, y: 2, z: 0 }
    ```
    
- ES6 이전
    
    `Object.assign` 메서드를 사용하여 여러 개의 객체를 병합하거나 프로퍼티를 수정하였다.
    
    ```jsx
    // 객체 병합 (프로퍼티가 중복되는 경우, 뒤에 위치한 프로퍼티가 우선권을 갖는다.)
    const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
    console.log(merged); // { x: 1, y: 10, z: 3 }
    
    // 특정 프로퍼티 변경
    const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
    console.log(changed); // { x: 1, y: 100 }
    
    // 프로퍼티 추가
    const added = Object.assign({}, { x: 1, y: 2 }, { z: 0 });
    console.log(added); // { x: 1, y: 2, z: 0 }
    ```

<br/>

## 🤔궁금한 점

## 📌중요한 점
