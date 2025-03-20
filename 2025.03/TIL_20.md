# 2025.03.20

1. Store-view 분리하기

- 비동기 처리하기

```javascript
// store.js
export const store = {
  state: { columns: [], historyList: [] },

  fetchData() {
    fetch("../data/data.json")
      .then((response) => response.json())
      .then((data) => {
        this.state.columns = data.columns;
        this.state.historyList = data.historyList;
      })
      .catch((error) => console.error("오류 발생:", error));
  },
};
// view.js
export function fetchData() {
  store.fetchData();
  console.log(store.state.columns); // []
  console.log(store.state.historyList); // []
  cardView(store.state.columns);
  historyView(store.state.historyList);
}
```

> 이렇게 처음에 처리했으나 [] 빈 배열이 담기는 것을 확인  
> &rarr; store.fetchData()가 처리되기 전에  
>  다음 코드가 실행되기 때문에 [](빈 배열)로 처리됨

**해결 방법**  
&rarr; **콜백 함수** 를 활용하여 처리  
(콜백 함수는 어떤 함수의 실행이 끝난 후 호출되는 함수)  
즉, fetchData()가 데이터를 다 가져온 후에 특정 함수(A)를 실행하고자 할 때,  
그 함수(A)를 인자로 넘겨서 실행할 수 있다

```javascript
// store.js
export const store = {
  state: { columns: [], historyList: [] },

  fetchData(callback) {
    fetch("../data/data.json")
      .then((response) => response.json())
      .then((data) => {
        this.state.columns = data.columns;
        this.state.historyList = data.historyList;
        callback();
      })
      .catch((error) => console.error("오류 발생:", error));
  },
};

// view.js
import { store } from "../store/store.js";
export function getData() {
  store.fetchData(function () {
    cardView(store.state.columns);
    historyView(store.state.historyList);
  });
}
```

- HTML data-\* 속성  
   &rarr; HTML 요소에 사용자 정의 데이터를 저장할 수 있도록 제공되는 속성  
   이 속성은 JavaScript에서 쉽게 접근할 수 있다  
   동적인 UI나 사용자 데이터 관리에 유용

  1.  기본 문법  
      HTML에서 data-로 시작하는 속성을 만들고,  
      JavaScript에서 dataset을 통해 접근할 수 있다
      예를 들어 보자!!!!

  ```javascript
  <button id="myButton" data-user-id="1234" data-role="admin">
    클릭하세요
  </button>
  ```

  이 코드에서

  ```javascript
  const button = document.getElementById("myButton");

  console.log(button.dataset.userId); // "1234"
  console.log(button.dataset.role); // "admin"
  ```

  이처럼 dataset 객체를 사용하면 data-\* 속성 값을 가져올 수 있다  
  &rarr; dataset은 객체다!!!!

  ```
  data-user-id &rarr; dataset.userId
  data-role &rarr; dataset.role
  ```

  이렇게 사용될 수 있다
