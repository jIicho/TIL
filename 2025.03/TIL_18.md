# 3/18

1. 해체 분해 할당

- **해체 할당(구조 분해 할당, Destructuring Assignment)**
- 배열이나 객체에서 값을 추출하여 변수에 쉽게 할당할 수 있도록 해주는 JavaScript 문법

  1. 배열에서 해체 할당

  ```javascript
  const numbers = [1, 2, 3];

  // 해체 할당 적용
  const [x, y, z] = numbers;
  console.log(x, y, z); // 1 2 3
  ```

  > 배열에서 x, y, z에 순서대로 값이 들어감

  - 배열 값 건너뛰기

    ```javascript
    const colors = ["red", "green", "blue"];

    const [, secondColor] = colors;
    console.log(secondColor);
    // "green" (첫 번째 값 건너뜀)
    ```

  - 나머지 값 한꺼번에 받기 (... 연산자)

    ```javascript
    const fruits = ["apple", "banana", "cherry", "grape"];
    const [first, ...rest] = fruits;

    console.log(first); // "apple"
    console.log(rest); // ["banana", "cherry", "grape"]
    ```

  2. 객체(Object)에서 해체 할당  
     &rarr; 객체에서 속성 값을 쉽게 변수에 할당할 수 있다

  ```javascript
  const person = { name: "Alice", age: 25, city: "Seoul" };

  // 일반적인 방식
  const name1 = person.name;
  const age1 = person.age;

  console.log(name1, age1); // "Alice" 25

  // 해체 할당 적용
  const { name, age, city } = person;
  console.log(name, age, city); // "Alice" 25 "Seoul"
  ```

  > 객체의 속성 이름과 변수 이름이 동일해야한다  
  > { name, age, city }는 person.name, person.age, person.city를 가져와 변수에 할당

  - 속성 이름 변경해서 할당하기

  ```javascript
  const user = { id: 1, username: "john_doe" };

  // username을 userName으로 변경하여 할당
  const { username: userName } = user;

  console.log(userName); // "john_doe"
  ```

  - 기본값 설정하기  
    &rarr; 만약 객체에 해당 속성이 없으면 기본값을 설정할 수도 있다

    ```javascript
    const settings = { theme: "dark" };

    // fontSize가 없으면 기본값 16 설정
    const { theme, fontSize = 16 } = settings;

    console.log(theme); // "dark"
    console.log(fontSize); // 16
    ```

  - 중첩된 객체 구조 분해

    ```javascript
    const student = {
      name: "Tom",
      info: { age: 20, grade: "A" },
    };

    // 중첩 객체의 값 구조 분해
    const {
      name,
      info: { age, grade },
    } = student;

    console.log(name); // "Tom"
    console.log(age); // 20
    console.log(grade); // "A"
    ```

  3. 함수에서 해체 할당
     &rarr; 함수의 매개변수에서 직접 해체 할당을 사용할 수도 있다

  ```javascript
  const user = { name: "Jane", age: 30 };

  function printUser({ name, age }) {
    console.log(`이름: ${name}, 나이: ${age}`);
  }

  printUser(user); // "이름: Jane, 나이: 30"
  ```

  **해체 할당의 장점**

  - 코드가 더 간결하고 읽기 쉬워짐
  - 객체나 배열에서 원하는 값만 쉽게 추출 가능
  - 기본값 설정으로 안전한 코드 작성 가능
  - 함수 매개변수에서도 유용하게 사용 가능

2. 기존 코드에서 store-view 구분하기

- 어떻게, 어떤 것을 분리 해야할 지 고민
- 클래스를 사용하여 구현

  &rarr; json 파일에서 불러오던 데이터를
  store라는 클래스 내에 담아 처리

  - Store 클래스 내에 담겨야 할 것들

    - 기존에 json 파일에 담겨있던 데이터를 클래스 내에 정보를 담을 것 (생성자 함수)
    - 추가하는 함수
    - 삭제하는 함수
    - 수정하는 함수
    - 이동시키는 함수

    ***

    - 추가되는 데이터

      1. 추가된 영역이 무엇인지 탐색
      2. Store 클래스 접근(클래스 내에 추가시키는 함수 만들기)
      3. (추가 함수 내에서) 해당되는 칼럼을 찾기  
         &rarr; 추가된 영역이 todo영역이면 데이터를 담고 있는 곳에서 todo칼럼 객체 찾기
      4. 그 객체 내에 count를 +1
      5. 카드리스트 배열 안에 추가되는 내용을 담는 객체 추가하기
      6. Store 클래스 내에서 기존 데이터를 담는 변수에서 historyList 찾기
      7. historyList 배열 맨 앞에 데이터(객체) 추가하기

      ***
