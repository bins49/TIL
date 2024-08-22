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



### React element

- JSX 문법으로 작성한 요소는 결과적으로 자바스크립트 객체가 된다.
  - 이런 객체를 React element라고 부른다.
- React element를 ReactDOM.render 함수의 argument로 전달하게 되면, 리액트가 객체 형태의 값을 해석해서 HTML 형태로 브라우저에 띄워준다.
- React element는 리액트로 화면을 그려내는데 가장 기본적인 요소다.

```react
import ReactDom from "react-dom";

const element = <h1>안녕 리액트!</h1>;
console.log(element);
ReactDOM.render(element, document.getElementById("root"));
```



#### React Component

- React component는 React element를 조금 더 자유롭게 다루기 위한 하나의 문법
- JavaScript의 함수를 활용하면 쉽게 만들 수 있다. 

```react
//Dice.js

import diceBlue01 from "./assets/dice-blue-1.svg";

function Dice() {
  <img src={diceBlue01} alt="주사위"/>
}

export default Dice;
```

```react
//App.js
import Dice from "./Dice";

function App() {
  return (
  	<div>
      <Dice />
    </div>
  );
}

export default App;
```



#### props

- component에 지정한 속성을 props라고 부른다.
  - Properties의 약자다. 
- props는 컴포넌트에 전달된 속성을 모두 가리키기 때문에 각각의 속성은 prop이라고 부른다.
- **컴포넌트 태그에 지정해준 prop은 하나의 객체 형태로 component 함수의 첫 번째 파라미터로 전달된다.**

- 예시 코드

```react
//App.js
import Dice from "./Dice";

function App() {
  return (
  	<div>
      <Dice color="blue" num={2}/>
    </div>
  );
}

export default App;
```

```react
import diceBlue01 from "./assets/dice-blue-1.svg";

function Dice(props) {
  console.log(props.color)
  <img src={diceBlue01} alt="주사위"/>
}

export default Dice;
```

- 그러나 만약에 속성이 여러개라면 `props.전달속성이름` 이렇게 하면 너무 복잡하다.

```react
import diceBlue01 from './assets/dice-blue-1.svg';
import diceBlue02 from './assets/dice-blue-2.svg';
import diceBlue03 from './assets/dice-blue-3.svg';
import diceRed01 from './assets/dice-red-1.svg';
import diceRed02 from './assets/dice-red-2.svg';
import diceRed03 from './assets/dice-red-3.svg';

const DICE_IMAGES = {
  blue: [diceBlue01, diceBlue02, diceBlue03],
  red: [diceRed01, diceRed02, diceRed03],
};

function Dice(props) {
  const src = DICE_IMAGES[props.color][props.num-1];
  <img src={diceBlue01} alt="주사위"/>
}

export default Dice;
```

- 그래서 아래처럼 중괄호를 활용해서 작성하면 좋다.

```react
function Dice({color = "blue", num = 1}) {
  const src = DICE_IMAGES[color][num-1];
  <img src={diceBlue01} alt="주사위"/>
}
```



#### children

- component의 자식들을 값으로 갖는 prop이다.

- 예시

```react
// Button.js
function Button({ text }) {
  return <button>{text}</button>
}

export default Button
```

```react
// App.js
import Button from './Button';
import Dice from './Dice';

function App() {
  return (
    <div>
      <Button text="던지기"/>
      <Button text="가져오기"/>
      <Dice color="red" num={4} />
    </div>
  );
}

export default App;
```

- React에서는 이렇게 단순히 바로 보여 주기만 할 값을 다룰 땐 prop을 활용하는 것보다 children prop을 사용하면 코드를 좀 더 직관적으로 구성하는 데 도움이 된다.

- 변경된 코드

```react
//Button.js

function Button({ children }) {
  return <button>{children}</button>
}

export default Button
```

```react
//App.js
import Button from './Button';
import Dice from './Dice';

function App() {
  return (
    <div>
      <Button>던지기</Button>
      <Button>보여주기</Button>
      <Dice color="red" num={4} />
    </div>
  );
}

export default App;
```



### State

- 만약 던지기 버튼을 누르면 주사위의 숫자가 바뀌면서 HTML 이미지 요소가 변하는 기능이 있다면 React를 사용하지 않는다면 어떻게 구현할까? 
  - 주사위마다 HTML 페이지를 만들고 이동시키거나 혹은 자바스크립트로 HTML 요소 노드의 속성만 새롭게 바꿀 수도 있다.

- **State는 React에서 변수 같은 건데 State를 바꾸면 React가 알아서 화면을 새로 rendering 해준다.**
- 예시 코드

```react
import { useState } from 'react';

function App() {
  const [num, setNum] = useState(1);

  const handleClearClick = () => {
    setNum(1);
};
```

- useState 사용법
  - 먼저 import 해온다.
  - 처음에 `const [현재 변수의 값, setter함수] = useState(초깃값)`의 형태로 설정한다.
  - setter함수를 정의한다.

- 예시 코드

```react
import { useState } from 'react';
import Button from './Button';
import Dice from './Dice';

function random(n) {
  return Math.ceil(Math.random() * n);
}


function App() {
  const [num, setNum] = useState(1);
  
  const handleRollClick = () => {
    const nextNum = random(6);
    setNum(nextNum)
  };
  
  const handleClearClick = () => {
    setNum(1);
  }

  return (
    <div>
      <div>
        <Button onClick={handleRollClick}>던지기</Button>
        <Button onClick={handleClearClick}>처음부터</Button>
      </div>
      <Dice color='red' num={num} />
    </div>
  );
}

export default App;
```



#### 참조형 state

- 배열이나 객체 같은 참조형 State를 사용할 때는 메소드나 할당 연산자로 값을 바꾸는 게 아니라  반드시 새로운 값을 만들어서 변경해야 한다. 
