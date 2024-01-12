# 📗챕터 정리
### 블록문 (Block/Compound Statement)

- 블록문이란 0개 이상의 문을 중괄호로 묶은 것을 의미한다.
- 블록문을 단독으로 사용할 수 있지만, 제어문이나 함수를 정의할 때 사용하는 것이 일반적이다.

### **제어문 (Control Flow Statement)**

- 제어문으로 코드의 실행 흐름을 인위적으로 제어할 수 있다.
    - **조건문 조건의 평가 결과에 따라 코드 블록의 실행 여부를 결정할 수 있다.
    - **반복문 조건의 평가 결과에 따라 코드 블록의 실행을 반복할 수 있다.

---

## 조건문
> **Condition Statement**
> 
- 조건문이란, 특정 조건이 `true`인 경우 실행하는 명령의 집합이다.
- 조건식의 평가 결과는 불리언 타입이어야 한다.
- 조건문은 표현식이 아니기 때문에 변수에 할당할 수 없다.

### 1️⃣ if 조건문

- 주어진 조건식의 평가 결과에 따라 실행할 코드 블록을 결정한다.
    - `if` : 조건식이 `true`일 때 실행하고, `false`라면 실행하지 않는다.
    - `else if` : `if` 조건식이 `false` 일 때, 다음 단계로써 `if` 와 동일하게 작동한다.
        - 조건이 여러 개 필요할 때 개수 상관없이 사용할 수 있다.
    - `else` : 상위 단계인 `if` / `else if` 조건식이 모두 `false` 일 때 실행한다.
        - `else`문은 필수가 아닌 선택 사항이다.
    
    ```jsx
    if ( fruit === '딸기' ) {
    	console.log('fruit는 딸기입니다.');
    } else if ( fruit === '사과' ) {
    	console.log('fruit는 사과입니다.');
    } else if ( fruit === '배' ) {
    	console.log('fruit는 배입니다.');
    } else {
    	console.log('fruit는 딸기, 사과, 배가 아닙니다.');
    }
    ```
    
- 조건문이 한 줄일 때는 중괄호(`{ }`) 를 생략할 수 있다.
    - 하지만, 단 한 줄이더라도 코드의 가독성을 위해 중괄호를 사용하는 것을 권장하는 편이다.
    
    ```jsx
    if ( year == 2024 ) console.log(true); // true
    ```
    
- `if ... else` 조건문을 **중첩**하여 사용할 수 있다.
    
    ```jsx
    // if 문(statement) 중첩
    
    // 영화 봤니?
    let didWatchMovie = confirm('너 엘리멘탈 영화 봤니?', '');
    
    if (!didWatchMovie) {
      // 영화 볼거니?
      let goingToWatchMovie = confirm('영화 볼거니?');
    
      if (goingToWatchMovie) {
        let withWho = prompt('누구랑 볼거니?', '');
    
        if (withWho === '여자친구') {
          console.log('zzz');
        } else if (withWho === '가족') {
          console.log('화목하네');
        } else {
          console.log('재밌게보구와~~~~');
        }
      }
    } else {
      let alone = confirm('너 혼자 봤니?');
    
      if (alone) {
        let didCry = confirm('너 울었니..?');
    
        if (didCry) {
          console.log('너..찌질했네..');
        } else {
          console.log('너 T야?');
        }
      }
    }
    ```


### 2️⃣ switch 조건문

- 주어진 조건식을 평가하여, 평가 결과와 일치하는 표현식을 가진 `case`문을 실행한다.
    - switch 문의 조건식은 불리언 타입보다는 숫자 타입인 경우가 많다.
- `**break**`문을 사용하여 반복문을 중지하도록 설계할 수 있다.
    - **반복문을 중지하지 않으면 조건식을 만족하는 `case`문의 아래에 있는 모든 코드가 실행된다.**
        
        조건식을 만족하는 `case`문을 실행한 후, 조건문을 탈출하기 위해서는 `break`문을 작성해야 한다.
        
        ```jsx
        let month = 11;
        let monthName;
        
        // break 문이 없으면 조건을 만족하는 case문 아래 모든 코드가 실행된다.
        switch (month) {
          case 1: monthName = 'January';
          case 2: monthName = 'February';
        	...
          case 11: monthName = 'November';
          case 12: monthName = 'December';
          default: monthName = 'Invalid month';
        }
        
        console.log(monthName); // Invalid month
        
        // break 문을 사용해야 조건을 만족하는 case문에서 탈출한다.
        switch (month) {
          case 1: monthName = 'January';
            break;
          case 2: monthName = 'February';
            break;
        	...
          case 11: monthName = 'November';
            break;
          case 12: monthName = 'December';
            break;
          default: monthName = 'Invalid month';
        }
        
        console.log(monthName); // November
        ```
        
- 표현식의 평가 결과와 일치하는 `case`문이 없을 때, `default`문이 있다면 `default`문을 실행한다.
    - switch 조건문을 작성할 때, `default`문을 필수로 사용해야 하는 것은 아니다.
    - `default`문은 switch 조건문 어디에 위치해도 상관 없지만, 가장 아래에 작성하는 것을 권장한다.
        
        조건문 실행 흐름이 `default`문을 만날 때, 그 아래에 있는 모든 코드도 함께 실행되기 때문이다.
        
- 일반적으로 switch 조건문보다 if 조건문을 많이 사용한다.
    
    그러나, 조건이 매우 많은 상황이라면 switch 조건문이 if 조건문 보다 유용하게 사용될 수 있다.
    
    ```jsx
    var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
    var month = 2;
    var days = 0;
    
    switch (month) {
      case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        days = 31;
        break;
      case 4: case 6: case 9: case 11:
        days = 30;
        break;
      case 2:
        // 윤년 계산 알고리즘
        // 1. 연도가 4로 나누어떨어지는 해(2000, 2004, 2008, 2012, 2016, 2020...)는 윤년이다.
        // 2. 연도가 4로 나누어떨어지더라도 연도가 100으로 나누어떨어지는 해(2000, 2100, 2200...)는 평년이다.
        // 3. 연도가 400으로 나누어떨어지는 해(2000, 2400, 2800...)는 윤년이다.
        days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
        break;
      default:
        console.log('Invalid month');
    }
    ```

---

## 반복문
> **Loop** **Statement**
> 
- 지정한 횟수/조건을 달성할 때까지 반복하는 구문을 의미한다.
- 반복 처리가 필요한 부분을 조건문으로 처리하면 불필요한 공정을 축소할 수 있다.
- 반복문의 `i` 변수는 반복(iteration)을 의미하는 말이다.
    
    *cf*. 객체를 조회할 때는 정방향보다 역방향으로 조회하는 것이 성능 관점에서 이점이 있다.
    

## 반복문의 종류

### 1️⃣ **for 반복문**

- 일반적으로 가장 많이 쓰이는 반복문으로, 특정 횟수만큼 반복할 때 사용하게 된다.
- for 문의 소괄호는 `(**변수 선언문;** **조건식;** **증감식)**`으로 이루어진다.
    
    ```jsx
    for (let i = 0; i < 3; i++) { 
      console.log(i);
    }
    
    // 0 1 2
    ```
    
- 소괄호 내부 요소는 모두 필수가 아닌 옵션 요소이지만, 어떤 식도 선언하지 않으면 무한루프가 된다.
- for 문 내에 for 문을 중첩하여 사용할 수 있다.
    
    ```jsx
    for (var i = 1; i <= 6; i++) {
      for (var j = 1; j <= 6; j++) {
        if (i + j === 6) console.log(`[${i}, ${j}]`);
      }
    }
    ```
    

### 2️⃣ **while 반복문**

- while 문의 조건식이 `true`일 때, 조건식이 `false`로 평가될 때까지 블록 내부의 코드를 반복 실행한다.
    
    ```jsx
    let i = 0;
    while (i < 3) {
      console.log(i);
      i++;
    }
    
    // 0 1 2
    ```
    
- 본문이 한 줄이라면 중괄호를 쓰지 않아도 된다.
- 무한루프를 탈출하기 위해 코드 블록 내에 if 조건문으로 탈출 조건을 작성하여 사용할 수도 있다.
    
    ```jsx
    // 무한루프
    while (true) {
      console.log(count);
      count++;
    
      // count가 3이면 코드 블록을 탈출한다.
      if (count === 3) break;
    }
    
    // 0 1 2
    ```
    

### 3️⃣ **do … while 반복문**

- 기본적으로 while 반복문과 동일하지만, 조건이 `false`이더라도 반드시 1번은 실행되는 반복문이다.
    
    ```jsx
    let i = 0;
    
    do {
      console.log(i);
      i++;
    } while (i < 3);
    
    // 0 1 2
    ```
    

### 4️⃣ **break 문**

- break 문이 실행되면 코드의 실행 흐름이 **코드 블록을 탈출**하여 불필요한 반복을 회피할 수 있다.
- switch 조건문에서 사용되는 break 문과 동일하게 작동한다.
- 이중 중첩 for 문의 내부 for 문에서 break 문이 실행되면 내부 for 문을 탈출하여 외부 for 문으로 진입한다.

### 5️⃣ **continue 문**

- continue 문이 실행되면 코드의 실행 흐름이 반복문 내부에서 증감식/조건식 영역으로 이동한다.
- 특정 조건에서는 반복문에서 열외시키고 싶을 때 사용할 수 있다.
    
    ```jsx
    var string = 'Hello World.';
    var search = 'l';
    var count = 0;
    
    // 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
    for (var i = 0; i < string.length; i++) {
      // 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.

      if (string[i] !== search) continue;
      
      count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
    }
    
    console.log(count); // 3
    
    // 참고로 String.prototype.match 메서드를 사용해도 같은 동작을 한다.
    const regexp = new RegExp(search, 'g');
    console.log(string.match(regexp).length); // 3
    ```

<br/>

## 🤔궁금한 점

## 📌중요한 점
**모두.. 중요해..**