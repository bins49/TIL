#### VSCode 단축키

- 멀티 커서
  - `Cmd + Shift + L`

- 멀티 커서 만들기
  - `Option` + 클릭
- 찾아 바꾸기
  - `F2`
- 해당하는 파일로 이동
  - `Cmd + 클릭`
- 줄 이동
  - `Option + ↑↓` 

- 줄 복사
  - ` Option + Shift + ↑↓`

#### map으로 렌더링하기

- map메서드를 활용하면 객체에 있는 데이터를 차례대로 뽑을 수 있다. 
- 사용예시
  - 아래에 map을 활용해서 추출한 item을 다른 함수에 prop으로 전달해서 다른 함수에서 데이터의 property에 접근한다.

```react
import "./ReviewList.css";

function formatDate(value) {
  // 날짜로 변환
  const date = new Date(value);
  // 년.월.일
  return `${date.getFullYear()}. ${date.getMonth() + 1}. ${date.getDate()}`;
}

// item을 받아온다.
function ReviewListItem({ item }) {
  return (
    <div className="ReviewListItem">
      <img className="ReviewListItem-img" src={item.imgUrl} alt={item.title} />
      <div>
        <h1>{item.title}</h1>
        <p>{item.rating}</p>
        <p>{formatDate(item.createdAt)}</p>
        <p>{item.content}</p>
      </div>
    </div>
  );
}

function ReviewList({ items }) {
  return (
    <ul>
      {items.map((item) => {
        return (
          <li>
            <ReviewListItem item={item} />
          </li>
        );
      })}
    </ul>
  );
}

export default ReviewList;
```



#### sort 정렬 바꾸기

- sort메서드를 활용하면 내가 원하는 property를 오름차순 및 내림차순으로 정렬할 수 있다.
  - 예를 들어 영화의 평점 및 최신순으로 정렬하고 싶다면 아래와 같은 방식으로 구성할 수 있다.
  - state를 활용하면 특정 버튼을 클릭했을 때 정렬해서 보여주고 싶은 속성을 보여줄 수 있다.

```react
import { useState } from "react";

function App() {
  const [order, setOrder] = useState("createdAt");
  const sortedItems = items.sort((a, b) => b[order] - a[order]);

  const handleNewestClick = () => setOrder("createdAt");

  const handleBestClick = () => setOrder("rating");

  return (
    <div>
      <div>
        <button onClick={handleNewestClick}>최신순</button>
        <button onClick={handleBestClick}>베스트순</button>
      </div>
      <ReviewList items={sortedItems} />
    </div>
  );
}

export default App;
```



#### filter로 삭제하기

- 우리가 어떤 데이터를 삭제하고 싶다면 filter기능을 사용하면 된다. 
  - 내가 삭제하고 싶은 값의 어떤 식별값을 filter해준다면 삭제할 수 있다.

- 예시

```react
// App.js
import mockitems from "../mock.json";
import { useState } from "react";

function App() {
  const [items, setItems] = useState(mockitems);
  
  const handleDelete = (id) => {
    const nextItems = items.filter((item) => item.id !== id);
    setItems(nextItems);
  };

  return (
    <div>
      <ReviewList items={sortedItems} onDelete={handleDelete} />
    </div>
  );
}

export default App;

```

```react
// ReviewList.js
function ReviewListItem({ item, onDelete }) {
  const handleDeleteClick = () => onDelete(item.id);

  return (
    <div className="ReviewListItem">
      <div>
        <button onClick={handleDeleteClick}>삭제</button>
      </div>
    </div>
  );
}

function ReviewList({ items, onDelete }) {
  return (
    <ul>
      {items.map((item) => {
        return (
          <li>
            <ReviewListItem item={item} onDelete={onDelete} />
          </li>
        );
      })}
    </ul>
  );
}

export default ReviewList;
```



- 배열을 렌더링할 때 Key prop을 지정해야 하는 이유
  - 요소들의 순서가 바뀌면 엉뚱한 위치에 렌더링 될 수 있기 때문에 반드시 고유의 key값을 설정해야 한다.
  - 예를 들어, key 값을 지정하지 않고 input 태그를 삽입해 어떤 값을 입력하고 삭제하면 input 태그에 입력된 값이 엉뚱한 곳에 가게 되는 것을 확인 할 수 있다. 그래서 꼭 Key를 지정해야 한다. 
  - **배열에 Key를 지정하지 않거나, Key를 지정하더라도 배열의 인덱스 같이 데이터를 구분할 수 없는 고유하지 않은 값으로 Key를 지정하면 rendering이 잘못될 수 있다.**

- 예시 코드

```react
function ReviewList({ items, onDelete }) {
  return (
    <ul>
      {items.map((item) => {
        return (
          <li key={item.id}>
            <ReviewListItem item={item} onDelete={onDelete} />
          </li>
        );
      })}
    </ul>
  );
}
```



#### React에서 fetch 사용하기

- 비동기로 데이터를 가져올 때 async await 구문을 활용해서 가져온다. 
- 예시 코드

```js
// api.js
export async function getReviews() {
  const response = await fetch(url);
  const body = await response.json();
  return body
}

// App. js
const handleLoadClick = async () => {
  // destructuring으로 데이터를 꺼내온다.
   const { reviews } = await getReviews();
  // setState에 넣어준다.
   setItems(reviews);
};
```



#### useEffect로 초기 데이터 가져오기

- component가 처음 렌더링될 때 request를 보내고 싶다면 useEffect를 사용한다. 
- 실행할 콜백 함수와 빈 배열을 넘겨주게 되면 리액트는 콜백 함수를 맨 처음 렌더링할 때만 실행한다. 
  - 그래서 무한루프를 막을 수 있다. 
- 배열은 dependency list라고 부른다.
  - dependcy list에 있는 값들을 앞에서 기억한 값이랑 비교한다.
  - 빈 배열인 경우에는 콜백 함수를 예약하지 않는다. 
- useEffect를 호출하면 react는 콜백 함수를 바로 실행하는 게 아니라 콜백 함수를 예약해 뒀다가 rendering이 끝나고 나면 실행해 준다. 

```react
function App() {
  useEffect(() => {
    handleLoad();
 }, []); 
}
```

