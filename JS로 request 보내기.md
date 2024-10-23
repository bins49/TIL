### fetch 

- ES6 표준에 소개된 문법
- 쉽게 request를 보낼 수 있다.
- Promise 기반이다.
- `res.json()`메소드는 바디의 JSON 문자열을 파싱해서 JS 객체로 변환
  - `res.text()`메소는 바디의 내용을 문자열 그대로 가져온다.

```js
// response를 가져오려면 await문을 가져와야 한다. 
const res = await fetch(URL);
// json 문자열을 돌려받기 위해 json 메소드를 사용한다. 
// json 메소드 역시 Promise를 리턴하는 비동기 함수라서 await를 사용해야 한다. 
const data = await res.json();
// Destructuring을 사용해서 프로퍼티들을 쉽게 가져올 수 있다.
const { result } = data;
```

- `fetch`는 **기본적으로 get 리퀘스트를 보내고 Promise를 리턴한다**.
  - 시간이 지나면 response가 도착하면, Promise는 Fulfilled 상태가 되고 

- offset은 데이터를 몇 개를 건너뛰고 요청할 것인지 그리고 limit은 데이터 몇 개를 요청할 것인지를 뜻한다.

```js
const url = new URL(url);
url.searchParams.append("offset", 10);
url.searchParams.append("limit", 10);

const res = await fetch(url);
const data = await res.json();

const res1 = await fetch("url/?offset=5&limit=10")

const { result } = data;
```



- POST request 보내고 데이터를 request body에 포함해라

```js
const res = await fetch("https://learn.codeit.kr/api/avatars", {
  method : 'POST',
  // JS 객체를 body data로 보내고 싶으면 JSON 문자열로 변환해야 한다. 
  body: JSON.stringify(avatarData),
  headers: {
    "Content-Type": "application/json"
  }
});

let result = await res.json();

console.log(result);
```



#### API 함수 만들기

```js
// api.js
export async function getColorSurveys(params = {}) {
  const url = new URL("https://learn.codeit.kr/api/color-surveys");
  Object.keys(params).forEach((key) => 
     url.searchParams.append((key, params[key]))
  );
  
  const res = await fetch(url);
  const data = await res.json();
  return data;
}

export async function getColorSurvey(id) {
  const res = await fetch(`https://learn.codeit.kr/api/color-surveys/${id}`);
  const data = await res.json();
  console.log(data);
}

export async function createColorSurvey(survey) {
  const res = await fetch('https://learn.codeit.kr/api/color-surveys', {
    method: "POST",
    body: JSON.stringify(surveyData),
    headers: {
      "Content-Type" : "application/json",
    }
  });
  const data = await res.json();
  return data;
}
```

```js
//main.js
import {getColorSurveys, getColorSurvey, createColorSurvey} from "./api.js";

const data1 = await getColorSurveys({ offset: 20, limit: 20});
console.log(data1);

const data2 = await getColorSurvey(10);

const surveyData = {
  mbti: "ENFJ",
  colorCode: "#ABCD00",
  password: "0000"
}

const data = await createColorSurvey(surveyData);
console.log(data);
```



- PATCH, DELETE 사용하기

```js
// main.js
// PATCH
export async function patchAvatar(id, avatarData) {
  const res = await fetch(`https://learn.codeit.kr/api/avatars/${id}`, {
    method: "PATCH",
    body: JSON.stringify(avatarData),
    headers: {
      "Content-Type" : "application/json",
    },
  });
  const data = await res.json();
  return data;
}

// DELETE
export async function deleteAvatar(id) {
  const res = await fetch(`https://learn.codeit.kr/api/avatars/${id}`)
  const data = await res.json();
  return data;
}
```

```js
// api.js
import { createAvatar, patchAvatar, deleteAvatar } from './api.js';

let avatar = await createAvatar({
  hairType: 'long1',
  hairColor: 'black',
  skin: 'tone300',
  clothes: 'collarBasic',
  accessories: 'headset'
});
avatar = await patchAvatar(avatar.id, {
  hairType: 'short3',
  hairColor: 'blonde',
});

console.log(avatar);
await deleteAvatar(avatar.id);
```



- 오류 처리하기

  - 유효하지 않는 주소나, 헤더 이름을 사용, 헤더 값이 이상하면 request 자체가 실패하여 **fetch가 리턴하는 Promise는 Rejected 상태가 된다.**
  - 그러나 **400이나 500 같은 에러 response가 돌아오는 경우에는 Promise는 Fulfilled 상태가 된다. **
    - 그래서 response 상태 코드가 성공을 나타내지 않으면 오류를 발생시키면 된다. 
      - `response.ok`메소드를 사용해서 200 성공 코드가 뜨지 않으면 error를 내보내면 된다. 

  ```js
  export async function getColorSurveys(params = {}) {
    const url = new URL("https://learn.codeit.kr/api/color-surveys");
    Object.keys(params).forEach((key) => 
       url.searchParams.append((key, params[key]))
    );
    
    const res = await fetch(url);
    
    // response의 상태 코드가 2로 시작하면 true 그렇지 않으면 false를 리턴한다.
    if (!res.ok) {
      throw new Error("데이터를 불러오는 데 실패했다.")
    }
  }
  ```

  


### axios

- fetch와 비슷한 문법
- 실무에 유용한 기능 제공한다. 

- `npm install axios` 설치



- fetch 사용(변경 전)

```js
export async function getColorSurvey(id) {
  const res = await fetch(`https://learn.codeit.kr/api/color-surveys/${id}`);
  const data = await res.json();
  console.log(data);
}

export async function getColorSurveys(params = {}) {
  const url = new URL("https://learn.codeit.kr/api/color-surveys");
  Object.keys(params).forEach((key) => 
     url.searchParams.append((key, params[key]))
  );
  
  const res = await fetch(url);
  const data = await res.json();
  return data;
}
```

- axios 사용(변경 후)

```js
export async function getColorSurvey(id) {
  const res = await axios(`https://learn.codeit.kr/api/color-surveys/${id}`);
  return res.data;
}

export async function getColorSurveys(params = {}) {
  const res = await axios.get(
    'url', 
    { params }
  );
  return res.data;
}
```

- axios를 사용하면 데이터 파싱(JSON)이 필요없이 data property로 가져오고 바로 리턴하면 된다.

- params 쿼리 파라미터를 담고 있는 객체를 전달하 객체에 있는 프로퍼티들로 알아서 쿼리 스트링을 만들고 URL 뒤에 붙여 리퀘스트를 보내준다.
  - 만약에 프로퍼티가 null이면, 프로퍼티를 무시하고 query string을 만들어 준다. 

- axios.post
  - post 메소드는  **body로 전달할 데이터를 두 번째 argument로 받는다. 자바스크립트 객체를 그대로 사용가능**
  - axios가 알아서 **자바스크립트 객체를 JSON 문자열로 변환해 주기 때문 JSON.stringify 같은 메소드를 사용하지 않아도 된다. **
  - header도 body data를 보고 알아서 설정해 준다. 우리가 설정하지 않아도 된다. 
- 변경 전(fetch)

```js
export async function createColorSurvey(survey) {
  const res = await fetch('https://learn.codeit.kr/api/color-surveys', {
    method: "POST",
    body: JSON.stringify(surveyData),
    headers: {
      "Content-Type" : "application/json",
    }
  });
  const data = await res.json();
  return data;
}
```

- 변경 후(axios)

```js
export async function createColorSurvey(survey) {
  const res = await axios.post(
    'https://learn.codeit.kr/api/color-surveys', 
     surveyData,                      
   );
  return res.data;
}
```

- request의 **body가 필요 없는 GET이나 DELETE는 옵션을 두 번째 파라미터로 받고, body가 필요한  POST, PATCH, PUT은 body 데이터를 두 번째 argument로 받고 옵션을 세 번째 argument로 받는다.**

- axios에서는 인스턴스라는 걸 만들 수 있다.

  - 예를 들어, 앞에 비슷한 공통적인 URL을 설정하고, timeout도 설정할 수 있다.
  - 참고로 baseURL은 URL이 모두 대문자이다.

  ```js
  const instance = axios.create({
    baseURL: "https://learn.codeit.kr/api",
    timeout: 3000,
  })
  
  export async function getColorSurveys(params = {}) {
    const res = await instance.get('url', {params}
    );
    return res.data;
  }
  ```

  

#### axios error

- `fetch`함수는 400이나 500 에러 코드가 response가 되어도 promise가 fullfilled되는 문제가 있었다. 
- `axios`함수는 반대로 400이나 500 에러 코드가 response가 되면 promise가 reject가 된다. 

- try catch 구문을 사용하면 axios가 객체의 메시지를 알아서 설정해 준다.

```js
//main.js
import {getColorSurveys, getColorSurvey, createColorSurvey} from "./api.js";

try {
  const survey = await getColorSurvey(123);
  console.log(survey);
} catch (e) {
  console.log(e.message);
  // response를 접근할 수 있다.
  console.log(e.response);
  // status와 data 프로퍼티에 접근 가능하다.
  console.log(e.response.status);
  console.log(e.response.data);
}
```

- **주의점**
  - `response.status` or `response.data`의 경우 response가 돌아올 때만 response 객체를 반환해준다. 따라서 미리 확인하는 것이 중요하다.
- 반영된 코드

```js
try {
  const survey = await getColorSurvey(123);
  console.log(survey);
} catch (e) {
	if (e.response) {
    console.log(e.response.status);
  	console.log(e.response.data);
  } else {
    console.log("리퀘스트가 실패했습니다.")
  }
  
}
```

