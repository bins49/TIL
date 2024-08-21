#### React project 시작하기

- react 프로젝트 생성하기 
  - `npm init react-app <폴더 이름>`을 입력하면 react 프로젝트가 생성된다.
  - `npx create react-app`와 똑같다. npx는 npm을 좀 더 편하게 사용하기 위해 npm에서 제공해주는 하나의 도구이다. 
  - `npm run start`를 하면 개발 모드가 실행된다.
  - `Ctrl + c`: 개발 모드 종료 



#### 인덱스 파일에서 하는일

```react
//index.js

import ReactDOM from "react-dom";

ReactDOM.render(<h1>안녕 리액트!</h1>, document.getElementById("root"));
```

- render는 화면을 그리는 역할인데 첫 번째 argument를 바탕으로 새로운 HTML 요소를 만들고, 그 요소를 두 번째 argument 값에다가 집어넣는 방식으로 동작한다. 
- 이게 index.html파일에 root에 h1태그를 넣어주는 형식이 된다. 



#### JSX

- 리액트에서 JS와 HTML을 섞어서 쓸 수 있는 자바스크립트의 확장된 문법이라고 보면 된다. 
- HTML과 똑같이 태그의 속성을 추가할 수도 있다. 

```react
import ReactDOM from "react-dom";

ReactDOM.render(<p id="hello">안녕 리액트</p>, document.getElementById("root"));
```

- **하지만 JSX는 JS의 확장 문법이기 때문에 HTML 문법을 완전히 그대로 사용할 수 없다.**

  - 1.class의 경우 자바스크립트에서 객체 지향의 개념으로 클래스를 선언할 때 사용한다.
    - 방법: `className`을 사용해야 한다.

  ```react
  // 예시
  class Dice() {
   	roll() {
      console.log("Roll!");
    }
  }
  
  ReactDOM.render(<p className="hello">안녕 리액트</p>, document.getElementById("root"));
  ```

  - 2.html에서 form태그를 사용할 때 label과 같이 사용하는데 그때 for을 사용하는데 for의 경우 자바스크립트에서 반복문을 돌 때 사용한다. 

    - 방법:`htmlfor`을 사용한다.

    ```react
    ReactDOM.render(
      <form>
      	<label htmlfor="name">이름</label>
      </form>,
     document.getElementById("root"));
    ```

    

- 이벤트 핸들러 작성도 좀 다르다. html에서 사용했던 onblur, onfocus, onmousedownr과 같은 것은 그대로 사용하면 안된다. 

  - 방법: JSX문법에서는 Camel Case 방식으로 작성된다. 

  ```react
  ReactDOM.render(
    <form>
    	<label htmlfor="name">이름</label>
    	<input onBlur="" onFocus=""/>
    </form>,
   document.getElementById("root"));
  ```



#### Fragment

- JSX문법에서 두 개의 태그를 동시에 작성하면 오류가 뜬다. 
- div 태그로 해도 되지만 굳이 쓰고 싶지 않은 경우 `<Fragment>`를 사용하면 된다.
  - 이름 없는 태그 = `<></>` 축약형 문법을 주로 사용한다. 

```react
// 예시
import ReactDOM from "react-dom";

ReactDOM.render(
  <Fragment>
  	<p>안녕</p>
    <p>리액트</p>
  </Fragment>, 
  document.getElementById("root")
);
```

