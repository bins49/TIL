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
- 처음 한 번만 실행하기

```react
function App() {
  useEffect(() => {
    // 실행할 코드
 }, []); 
}
```

- 값이 바뀔 때마다 실행하기

```react
function App() {
  useEffect(() => {
    // 실행할 코드
 }, [dep1, dep2, dep3, ...]); 
}
```









#### 서버에서 정렬한 데이터 받아오기

- useEffect에서 dependency list의 변화가 있을때 재랜더링이 된다. 

- 예시
  - 위에서 정의한 order state에 변화가 있을 때 변경된다. 

```react
function App() {
  const [order, setOrder] = useState("createdAt");
  
  useEffect(() => {
    handleLoad();
  }, [order])
}
```

- 서버에서 데이터를 받아올 때 내가 원하는 값을 기준으로 정렬하려면 query 문을 수정해오면 된다.

- 예시

```react
function App() {
  const [order, setOrder] = useState("createdAt");
  
  // 변경된 state를 받아오고 이것을 서버에 데이터를 받아오는 getReviews함수에 넣어준다.
  const handleload = async(orderQuery) => {
    const {reviews} = await getReviews(orderQuery);
    setItems(reviews);
  }
  
  const handleNewestClick = () => setOrder('createdAt');
  const handleBestClick = () => setOrder('rating');
  
  // 최신순, 베스트순을 누르면 state가 변경된다.
  // useEffect에서 데이터를 받아오는 handleload에 order state의 값을 넣어 보내준다. 
  useEffect(() => {
    handleLoad(order);
  }, [order])
  
   return (
    <div>
      <div>
        <button onClick={handleNewestClick}>최신순</button>
        <button onClick={handleBestClick}>베스트순</button>
      </div>
      <ReviewList items={sortedItems} onDelete={handleDelete} />
    </div>
  );
}
```

```react
// getReviews에 state의 값을 받아서 사용할 수 있다. 
export async getReviews (order = "createdAt") {
  const query = `order=${order}`;
  const response = await fetch (`주소 ${query}`);
  const body = await response.json();
  return body;
}
```





#### Pagination

- **책의 페이지처럼 데이터를 나눠서 제공하는 것**

- 많은 양의 데이터를 제공할 때 사용하는 방법이다. 

- 오프셋 기반 Pagination

  - **받아온 데이터 개수를 기준**으로 데이터를 나눈다.
  - 예시
    - 지금까지 20개 받았으니까 10개 더 보내달라는 뜻

  - `GET https://example.com/posts?offset=20&limit=10`
  - 오프셋 기반은 데이터가 바뀌면 내가 원하는 데이터를 정확히 가져오지 못한다는 단점이 존재한다.
    - 데이터의 중복이나 빠지는 현상이 발생한다. 

- 커서 기반 Pagination

  - **커서는 특정 데이터를 가리키는 값이다.**
  - **데이터를 가리키는 커서 기준으로 데이터를 가져온다.**
  - **지금까지 받은 데이터를 표시한 책갈피**
  - 예시

  `GET https://example.com/posts?cursor=WerZxc&limit=10`

  - 서버에 request를 보내면 서버는 response로 페이지네이션 정보를 보내준다.
  - 거기에 다음 커서값도 같이 넘겨준다. 다음 페이지를 불러 올 땐 아까 받아 온 커서값으로 request를 보낸다
  - 커서를 기준으로 서버에 데이터를 요청한다. 
  - **커서를 사용하면 만약에 데이터가 바뀌더라도 커서가 가리키는 데이터는 변하지 않는다.**

- 결론

  - 서버 입장에서 커서 기반이 오프셋 기반보다 만들기 까다롭고 데이터가 자주 바뀌는 게 아니라도 오프셋 기반도 충분하다.



#### 데이터 더 불러오기

- 더 보기 버튼을 눌렀을 때 내가 원하는 개수만큼 데이터를 불러오고 싶다면 아래의 코드처럼 설정하면 된다.

- 코드

```react
// api.js 
export async getReviews ({order = "createdAt", offset:0, limit:6}) {
  const query = `order=${order}&offset=${offset}&limit=${limit}`;
  const response = await fetch (`주소 ${query}`);
  const body = await response.json();
  return body;
}
```

```react
// App.js

// 새로 가져올 데이터는 고정적으로 5개라고 정하겠다.
const LIMIT = 5;

function App() {
  const [items, setItems] = useState([]);
  const [order, setOrder] = useState("createdAt");
  const [offset, setOffset] = useState(0);
  const [hasNext, setHasNext] = useState(false);
  
  const handleLoad = async (options) => {
    const {reviews, paging} = await getReviews(options);
    // 현재 가지고 있는 데이터 개수가 0이라면 처음 useEffect를 해서 받아 온 데이터를 state에 넣어준다.
    if (offset === 0) {
      setItems(reviews);
    } else {
      // 만약에 기존에 데이터가 있는 상황이라면, 새로운 데이터를 받아들일 때 두 데이터를 합친다.
      setItems([...items, ...reviews]);
      // 다음 데이터가 존재하는지 판별하기 위해 hasNext 속성을 계속 넣어준다.
      setHasNext(paging.hasNext);
    }
  }
  // 더 보기 버튼을 누르면 현재 offset이 계속 업데이트가 되어서 새로운 글이 생성된다. 
  const handleLoadMore = () => {
    handleLoad({ order, offset, limit:LIMIT});
  }
  
  // 처음에 데이터 렌더링 할 때 초깃값 설정해서 handleLoad로 보내준다. 
  useEffect(() => {
    handleLoad({ order, offset: 0, limit: LIMIT});
  }, [order])
  
  return (
    // 조건부 렌더링을 사용한다. hasNext가 true 이면 오른쪽 코드가 실행된다.  
  	<div>
      {hasNext && <button onClick={handleLoadMore}>더 보기</button>}
    </div>
  )
}
```



- 조건부 렌더링 꿀팁

  -  AND 연산자
    - show값이 true이면 렌더링 하고, false이면 렌더링 하지 않는다. 

  ```react
  import { useState } from 'react';
  
  function App() {
    const [show, setShow] = useState(false);
  
    const handleClick = () => setShow(!show);
  
    return (
      <div>
        <button onClick={handleClick}>토글</button>
        {show && <p>보인다 👀</p>}
      </div>
    );
  }
  ```

  - OR 연산자
    - hide가 true이면 렌더링 하지 않고, false이면 렌더링 한다.

  ```react
  import { useState } from 'react';
  
  function App() {
    const [hide, setHide] = useState(true);
  
    const handleClick = () => setHide(!hide);
  
    return (
      <div>
        <button onClick={handleClick}>토글</button>
        {hide || <p>보인다 👀</p>}
      </div>
    );
  }
  ```

  - 삼항 연산자 
    - 참, 거짓일 경우에 다르게 렌더링해줄 수 있다.

  ```react
  import { useState } from 'react';
  
  function App() {
    const [toggle, setToggle] = useState(false);
  
    const handleClick = () => setToggle(!toggle);
  
    return (
      <div>
        <button onClick={handleClick}>토글</button>
        {toggle ? <p>✅</p> : <p>❎</p>}
      </div>
    );
  }
  ```

  - 렌더링되지 않는 값들

  ```react
  function App() {
    const nullValue = null;
    const undefinedValue = undefined;
    const trueValue = true;
    const falseValue = false;
    const emptyString = '';
    const emptyArray = [];
  
    return (
      <div>
        <p>{nullValue}</p>
        <p>{undefinedValue}</p>
        <p>{trueValue}</p>
        <p>{falseValue}</p>
        <p>{emptyString}</p>
        <p>{emptyArray}</p>
      </div>
    );
  }
  
  export default App;
  ```

  

- **비동기 상황에서 state를 변경할 때 이전 state값을 사용하려면 setter함수에서 콜백을 사용해서 이전 state를 사용한다.**

  - 어떤 값을 삭제한다고 가정하면, 값을 삭제하고 난 후 비동기 함수에서 호출하고 나고 함수 내에서는 삭제되기 전에 state값이 있기 때문에 지웠던 데이터가 다시 살아난 것처럼 렌더링 된다. 
  - 그래서 setter 함수에서 콜백을 사용해서 이전 state를 사용하면 문제가 해결된다. 
    - react가 현재 state값을 전달해주기 때문에, 

  ```react
  setItems((prevItems) => [...prevItems, ...reviews]);
  ```

  



- 참조형 State 사용의 잘못된 예 

```react
const [state, setState] = useState({ count: 0 });

const handleAddClick = () => {
  state.count += 1; // 참조형 변수의 프로퍼티를 수정
  setState(state); // 참조형이기 때문에 변수의 값(레퍼런스)는 변하지 않음
}
```

- 참조형 State 사용의 올바른 예

```react
const [state, setState] = useState({ count: 0 });

const handleAddClick = () => {
  setState({ ...state, count: state.count + 1 }); // 새로운 객체 생성
}
```

- 콜백으로 State 변경
  - 이전 State 값으로 새로운 State를 만드는 경우엔 항상 콜백 형태로 사용하는 습관을 들이자
  - 올바른 State 값을 가져와서 사용할 수 있다. 

```react
const [count, setCount] = useState(0);

const handleAddClick = async () => {
  await addCount();
  setCount((prevCount) => prevCount + 1);
}
```

