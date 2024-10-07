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



#### 네트워크 로딩 처리하기

- 데이터를 다 불러오기 전에 더 보기 버튼을 누르는 것을 방지해야 한다. 그렇지 않으면 중복된 데이터를 불러올 수도 있다.

- 코드

```react
function App() {
  const [isLoading, setIsLoading] = useState(false);
  
  let result;
  // 더보기 버튼을 누르면 데이터를 가져오니까 true해주고
  try {
    setIsLoading(true);
    result = await getResult(options);
    // 오류가 나면 error를 출력하고 return
  } catch (error) {
    console.log(error)
    return;
  } finally {
    // 오류가 나서 return하더라도 네트워크 request가 끝나면 항상 false처리를 하게 된다.
    setIsLoading(false);
  }
  
  return (
  	<div>
    	<button disabled={isLoading}>더 보기</button>
    </div>
  )
}
```



#### 네트워크 에러 처리하기

- 네트워크 에러가 발생할 시에 state를 활용해서 에러를 처리하는 작업을 진행했다.

- 코드

```react
//App.js
function App() {
  const [isLoading, setIsLoading] = useState(false);
  // 초깃값은 null로 설정한다. 
  const [loadingError, setLoadingError] = useState(null);
  
  const handleLoad = async (options) => {
    let result;
    try {
      setIsLoading(true);
      result = await getReviews(options);
    } catch (error) {
      console.error(error);
      return;
    } finally {
      setIsLoading(false);
    }
  
  //optional chaining을 활용해서 loading error가 있을때만 message property를 참조하게 된다. 
  return (
  	<div>
    	<button disabled={isLoading}>더 보기</button>
      {loadingError?.message && <span>{loadingError.message}</span>}
    </div>
  )
}
```

```react
// api.js
// response의 ok라는 메소드를 통해 오류가 나는 경우에는 Error 던진다. 
if (!response.ok) {
  throw new Error("리뷰를 불러오는 데 실패했습니다.")
}
```



#### 검색 기능

- 간단한 검색 기능을 만들어보자.

```react
function App() {
  const [search, SetSerach] = useState("");
  
  
  const handleSearch = (e) => {
    e.preventDefault();
    SetSearch(e.target['search'].value)
  };
  
  const handleLoadMore = () => {
    handleLoad({
      order,
      cursor,
      search,
    })
  }
  
  useEffect(() => {
    handleLoad({
      order,
      search
    })
  }, [order, search])
  return (
  	<div>
       <button type="submit">검색</button>
    </div>
  )
}
```



#### 입력 폼 만들기

- HTML에서는 사용자가 input을 입력할 때마다 `onInput`이라는 이벤트가 발생했다.
  - `onChange` 이벤트는 사용자 입력이 끝났을 때 발생하는 이벤트다.
- 리액트에서 `onChange`이벤트는 JS에서 `onChange`와 다르게 동작한다. 
  - **`onInput`처럼 사용자가 값을 입력할 때마다 `onChange` 이벤트가 발생한다.**

- 사용 예시

```react
import {useState} from "react";

function reviewForm() {
  const [title, setTitle] = useState("");
  const [rating, setRating] = useState(0);
  const [content, setContent] = useState("");
  
  const handleTitle = (e) => {
    const nextTitle = e.target.value;
    setTitle(nextTitle);
  }
  
  const handleRating = (e) => {
    const nextRating = e.target.value;
    setRating(rating);
  }
  
  const handlContent = (e) => {
    const nextContent = e.target.value;
    setContent(nextTitle);
  }
  
  return (
  	<form>
     <input value={title} onChange={handleTitle}></input>
      <input type="number" value={rating} onChange={handleRating}></input>
      <input value={content} onChange={handleContent}></input>
    </form>
  )
  
}

export default ReviewForm;
```



#### onSubmit을 써보자

- onSubmit함수를 사용해서 입력값을 전송할 수 있다. 
- 사용 예시 

```react
import { useState } from 'react';
import './ReviewForm.css';

function ReviewForm() {
  const [title, setTitle] = useState('');
  const [rating, setRating] = useState(0);
  const [content, setContent] = useState('');

  const handleTitleChange = (e) => {
    setTitle(e.target.value);
  };

  const handleRatingChange = (e) => {
    const nextRating = Number(e.target.value);
    setRating(nextRating);
  };

  const handleContentChange = (e) => {
    setContent(e.target.value);
  };

  const handleSubmit = (e) => {
    // HTML에서 submit 버튼을 사용하면 기본적으로 Get request를 한다. 그래서 기본동작을 막아줘야한다. 
    e.preventDefault();
    console.log({
      title,
      rating,
      content,
    });
  };

  return (
    <form className="ReviewForm" onSubmit={handleSubmit}>
      <input value={title} onChange={handleTitleChange} />
      <input type="number" value={rating} onChange={handleRatingChange} />
      <textarea value={content} onChange={handleContentChange} />
      <button type="submit">확인</button>
    </form>
  );
}

export default ReviewForm;

```





#### 하나의 state로 form 구현하기

- 이벤트 객체에서 name값을 가져올 수 있다는 점을 활용하자.
- 코드

```react
import { useState } from 'react';
import './ReviewForm.css';

function ReviewForm() {
  const [values, setValues] = useState("");

  const handleChange = (e) => {
    const [ name, value ] = e.target;
    setValues((prevValues) => ({
      ...prevValues,
      [name] : value,
    }));
  }

  const handleSubmit = (e) => {
    // HTML에서 submit눌렀을 때 페이지를 이동하는 동작이 나타난다.. 그래서 기본동작을 막아줘야한다. 
    e.preventDefault();
    console.log(values)
  };

  return (
    <form className="ReviewForm" onSubmit={handleSubmit}>
      <input names="title" value={valeus.title} onChange={handleChange} />
      <input type="number" names="rating" value={values.rating} onChange={handleChange} />
      <textarea names="content" value={values.content} onChange={handleChange} />
      <button type="submit">확인</button>
    </form>
  );
}

export default ReviewForm;
```





#### 제어 컴포넌트와 비제어 컴포넌트

- 제어 컴포넌트(Controlled Component)
  - **인풋의 value 값을 React에서 지정**
  - React에서 사용하는 값과 실제 인풋 값이 항상 일치
  - **주로 권장되는 방법**

- 예시 코드
  - 아래처럼 value를 대문자로 설정하게 되면 사용자가 소문자를 입력해도 대문자로 변환된 value가 state로 전달되어 변경되기 때문에 계속해서 대문자가 value로 보여지게 된다. 

```react
function MyComponent() {
  const [value, setValue] = useState("");
  
  const handleChange = (e) => {
    const nextValue = e.target.value.toUpperCase();
    setValues(nextValue);
  }
  
  return <input value={value} onChange={handleChange}/> 
}
```

- 다른 예시 코드

```react
function MyComponent({value, onChange}) {
  
  const handleChange = (e) => {
    const nextValue = e.target.value.toUpperCase();
    onChange(nextValue);
  }
  
  return <input value={value} onChange={handleChange}/> 
}

function App() {
    const [value, setValue] = useState("");
  	
  	const handleClear = () => SetValue("");
  
    return (
    	<div>
      	<MyComponent value = {value} onChange = {setValue}/>
        <button onClick={handleClear}>지우기</button>
      </div>
    )
}
```



- 비제어 컴포넌트(Uncontrolled Component)
  - **인풋의 value 값을 React에서 지정하지 않음**
  - 경우에 따라서 필요한 방법

- 예시 코드
  - 아래의 경우, value prop을 제거했다. 
    - React에서 사용하는 값이랑 실제 input 값이랑 달라진다.

```react
function MyComponent() {
  const [value, setValue] = useState("");
  
  const handleChange = (e) => {
    const nextValue = e.target.value.toUpperCase();
    setValues(nextValue);
  }
  
  return <input onChange={handleChange}/> 
}
```





#### 파일 인풋

- FileInput은  보안 문제 때문에 value prop을 지정할 수 없다. 
  - 반드시 비제어 컴포넌트로 만들어야 한다.
- 예시 코드
  - 잘못된 방식

```react
import { useState } from "react";

function FileInput() {
  const [value, setValue] = useState();

  const handleChange = (e) => {
    const nextValue = e.target.files[0];
    setValue(nextValue);
  };

  return <input type="file" value={value} onChange={handleChange} />;
}

export default FileInput;
```

- 수정 후 

```react
import { useState } from "react";

function FileInput() {
  const [value, setValue] = useState();

  const handleChange = (e) => {
    const nextValue = e.target.files[0];
    setValue(nextValue);
  };

  return <input type="file" onChange={handleChange} />;
}

export default FileInput;
```



- 비제어 컴포넌트로 file input 수정하기

```react
// Fileinput.js
function FileInput({name, value, onChange}) {

  const handleChange = (e) => {
    const nextValue = e.target.files[0];
    onChange(name, nextValue);
  };

  return <input type="file" onChange={handleChange} />;
}

export default FileInput;
```

```react
// ReviewForm.js
import { useState } from 'react';
import FileInput from './FileInput';

function ReviewForm() {
  const [values, setValues] = useState({
    title: '',
    rating: 0,
    content: '',
    imgFile: null,
  });
  
  const handleChange = (name, value) {
    setValues((prevValues) => ({
      ...prevValues,
      [name] : value.
    }))
  }
  
  const handleInputChange = (e) => {
    const { name, value } = e.target;
    handleChange(name, value);
  }
  
  return (
  	<FileInput name="imgFile" value={values.imgFile} onChange ={handleChange}/>
  )
  
}  
```





### 파일인풋

- 파일 인풋은 반드시 **비제어 Input으로 만들어져야 한다.** 
  - Stack trace라는 오류 추적을 통해서 알 수 있다.
- 파일 인풋의 value 속성은 사용자만 직접 바꿀 수 있고 자바스크립트로 바꿀 때는 빈 문자열로만 바꿀 수 있다.
- 문제코드 
  - e.target.value를 출력하면 실제 파일 경로가 아니라 이상한 문자열이 출력되는데 이것은 보안 문제 때문에 웹브라우저에서 파일 경로를 숨겨준다.
  - 이러한 문제 때문에 Fileinput은 value prop을 지정할 수 없다. 
  - 반드시 비제어 컴포넌트로 만들어야 한다. 

```react
//FileInput.js
import { useState } from "react";

function FileInput() {
  const [value, setValue] = useState();
  
  const handleChange = (e) => {
    const nextValue = e.target.files[0];
    setValue(nextValue);
  }
  
  return <input type="file" value={value} onChange={handleChange} />
}

export default FileInput;
```

- 수정 후 코드
  - value를 지워주면 오류없이 State값이 잘 변경된다. -> 비제어 컴포넌트

```react
//FileInput.js
import { useState } from "react";

function FileInput() {
  const [value, setValue] = useState();
  
  const handleChange = (e) => {
    const nextValue = e.target.files[0];
    setValue(nextValue);
  }
  
  return <input type="file" onChange={handleChange} />
}

export default FileInput;
```

- State 대신에 props를 사용하는 법 
  - State Lifting
    - 현재 FileInput의 State를 props로 바꾸고 ReviewForm 컴포넌트에 있는 State를 props로 내려준다.

```react
//FileInput.js
import { useState } from "react";

function FileInput({ name, value, onChange }) {
  const handleChange = (e) => {
    const nextValue = e.target.files[0];
    onChange(name, nextValue);
  }
  
  return <input type="file" onChange={handleChange} />
}

export default FileInput;
```

```react
// ReviewForm.js
function ReviewForm() {
  const [values, setValues] = useState({
    title: "",
    rating: 0,
    content: "",
    imgFile: null,
  });
  
  const handleChange = (name, value) => {
    const {name, value} = e.target;
    setValues((prevValues) => ({
      ...prevValues,
      [name]: value,
    }));
  };
  // FileInput에서 onChange를 name, value로 호출했다. 그래서 이런 경우를 처리하기 위해 좀 더 추상화된 함수를 만든다.
  const handleInputChange = (e) => {
    const {name, value} = e.target;
    handleChange(name, value);
  }
  
  return (
  	<FileInput name="imgFile", value={values.imgFile} onChange={handleChange} />
  )
}
```





### ref로 DOM 노드 가져오기 

- ref를 쓰면 실제 DOM 노드를 직접 참조할 수 있다.
- **DOM 노드는 반드시 렌더링이 끝나야 생기니까 ref 객체의 current 값도 화면에 컴포넌트가 렌더링됐을 때만 존재한다. **
  - 조건부 렌더링으로 컴포넌트가 사라지거나 하는 경우에는 이 값이 존재하지 않을 수도 있다.
  - 그래서 항상 **inputRef.current**값이 존재하는지 확인한다. 

```react
import { useEffect, useRef } from "react";

function FileInput({ name, value, onChange }) {
  const inputRef = useRef();
  
  const handleChange = (e) => {
    const nextValue = e.target.files[0];
    onChange(name, nextValue);
  }
  
  useEffect(() => {
    if (inputRef.current) {
      console.log(inputRef.current);
    }
  }, []);
  
  return <input type="file" onChange={handleChange} ref={inputRef} />;
}

export default FileInput;
```



### 파일 인풋 초기화

```react
import { useRef } from "react";

function FileInput({ name, value, onChange }) {
  const inputRef = useRef();
  
  const handleChange = (e) => {
    const nextValue = e.target.files[0];
    onChange(name, nextValue);
  }
  
  const handleClearClick = () => {
    const inputNode = inputRef.current;
    if (!inputNode) return;
    
    inputNode.value = "";
    onChange(name, null);
  }
  
  
  return (
    <div>
   		<input type="file" onChange={handleChange} ref={inputRef} />
      {value && <button onClick={handleClearClick}>X</button>}
    </div>
   );
}

export default FileInput;
```

- Ref를 활용해도 이미지 크기 구할 수 있다.

```react
import { useRef } from "react";

function Image({ src }) {
  const imgRef = useRef();
  
  const handleSizeClick = () => {
    const imgNode = imgRef.current;
    if (!imgNode) return;
    
    const { width, height } = imgNode;
    console.log(`${width} x ${height}`);
  };
  
  return (
  	<div>
    	<img src={src} ref={imgRef} alt="크기를 구할 이미지" />
      <button onClick={handleSizeClick}>크기 구하기</button>
    </div>
  );
}
```





### 이미지 파일 미리보기

- 인터넷에 올린 파일 링크 같이 사용자 컴퓨터에 있는 파일을 주소로 사용할 수 있다. 
- `URL.createObjectURL()`을 사용하면 문자열을 리턴하고 이 문자열을 해당 파일의 주소처럼 쓸 수 있는 값이다. 
  - 웹 브라우저에 메모리를 할당한다. 
- 컴포넌트 함수에서 외부의 상태를 바꾸는 것을 **side effect**라고 한다.
  - 네트워크 request도 일종의 side Effect이다.
- 리액트에서는 side Effect를 다루는 경우에 주로 useEffect를 사용한다. 

```react
import { useEffect, useRef, useState } from 'react';

function FileInput({ name, value, onChange }) {
  const [preview, setPreview] = useState();
  const inputRef = useRef();

  const handleChange = (e) => {
    const nextValue = e.target.files[0];
    onChange(name, nextValue);
  };

  const handleClearClick = () => {
    const inputNode = inputRef.current;
    if (!inputNode) return;

    inputNode.value = '';
    onChange(name, null);
  };

  useEffect(() => {
    if (!value) return;
    const nextPreview = URL.createObjectURL(value);
    setPreview(nextPreview);
  }, [value]);
  
  return (
  	<div>
      <img src={preview} alt="이미지 미리보기" />
      <input type="file" accept="image/png, image/jpeg" onChange={handleChange} ref={inputRef} />
      {value && <button onClick={handleClearClick}>X</button>}
    </div>
  );
}
```



### 사이드 이펙트 정리하기

- 다른 파일을 선택하거나 파일 선택을 해제했을 때 메모리도 같이 해제해 줘야 한다. 
  - 이 때 사용하는 함수가 `revokeObjectURL`이다.
- 아래의 코드처럼 ObjectURL를 만들면서 웹 브라우저가 할당한 메모리가 바로 side effect이다..

```react
useEffect(() => {
    if (!value) return;
    const nextPreview = URL.createObjectURL(value);
    setPreview(nextPreview);
  	
   // 더 이상 사용하지 않는 ObjectURL을 해제해 주는 코드가 실행된다.
   	return () => {
      setPreview();
      URL.revokeObjectURL(nextPreview);
    }
  return (
  	<div>
      <img src={preview} alt="이미지 미리보기" />
      <input type="file" accept="image/png, image/jpeg" onChange={handleChange} ref={inputRef} />
      {value && <button onClick={handleClearClick}>X</button>}
    </div>
  );
}, [value]);
```

- accept는 파일 인풋을 이미지 파일 하나만 선택하는데 사용한다.



### 사이드 이펙트와 useEffect

- Side Effect는 말 그대로 외부에 부수적인 작용을 하는 걸 말한다.
- 예시
  - add 함수를 실행하면 함수 외부의 상태가 바뀌기 대문에, 이런 함수를  side effect가 있다고 한다. 

```js
let count = 0;

function add(a, b) {
  const result = a + b;
  count += 1; // 함수 외부의 값을 변경
  return result;
}

const val1 = add(1, 2);
const val2 = add(-4, 5);
```

- useEffect는 sideEffect를 실행하고 싶을 때 사용하는 함수이다. 
- 주로 react 외부에 있는 데이터나 상태를 변경할 때 사용한다. 



- useEffect말고 핸들러 함수만 사용했을 때 

```react
import { useState } from 'react';

const INITIAL_TITLE = 'Untitled';

function App() {
  const [title, setTitle] = useState(INITIAL_TITLE);

  const handleChange = (e) => {
    const nextTitle = e.target.value;
    setTitle(nextTitle);
    document.title = nextTitle;
  };

  const handleClearClick = () => {
    const nextTitle = INITIAL_TITLE;
    setTitle(nextTitle);
    document.title = nextTitle;
  };

  return (
    <div>
      <input value={title} onChange={handleChange} />
      <button onClick={handleClearClick}>초기화</button>
    </div>
  );
}

export default App;
```

- useEffect를 사용했을 때 

```react
import { useEffect, useState } from 'react';

const INITIAL_TITLE = 'Untitled';

function App() {
  const [title, setTitle] = useState(INITIAL_TITLE);

  const handleChange = (e) => {
    const nextTitle = e.target.value;
    setTitle(nextTitle);
  };

  const handleClearClick = () => {
    setTitle(INITIAL_TITLE);
  };
  // useEffect를 사용하면 반복하는 코드를 줄일 수 있다. 
  useEffect(() => {
    document.title = title;
  }, [title]);

  return (
    <div>
      <input value={title} onChange={handleChange} />
      <button onClick={handleClearClick}>초기화</button>
    </div>
  );
}

export default App;
```



- 정리 함수가 실행되는 시점
  - 콜백을 한 번 실행했으면, 정리 함수도 반드시 한 번 실행된다
  - 새로운 콜백 함수가 호출되기 전에 실행되거나, 컴포넌트가 화면에서 사라지기 전에 실행된다.

```react
import { useEffect, useState } from "react"

function Timer() {
  const [second, setSecond] = useState(0);
  
  useEffect(() => {
    const timerId = setInterval(() => {
      console.log('타이머 실행중 ...');
      setSecond((prevSecond) => prevSecond + 1);
    })
  }, 1000);
  console.log("타이머 시작");
  
  return () => {
    clearInterval(timerId);
    console.log("타이머 멈춤")
  }
}, []);
```

