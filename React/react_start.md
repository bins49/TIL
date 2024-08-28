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

  - 예시 코드

  ```react
  const [gameHistory, setGameHistory] = useState([]);
  
  const handleRollClick = () => {
      const nextNum = random(6);
      gameHistory.push(nextNum);
      setGameHistory(gameHistory); // state가 제대로 변경되지 않는다!
  };
  ```

  - 위의 코드가 작동하지 않는 이유는 `setGameHistory`의 state는 **배열 값 자체를 가지고 있는 게 아니라 배열의 주솟값이 변경된 것이 아니다.**
  - 따라서 React 입장에서는 주솟값이 여전히 똑같기 때문에 상태가 바뀌었다고 판단하지 않는다.
  - 참조형 state를 활용할 때는 반드시 새로운 참조형 값을 만들어 state를 변경해야 한다.
  - 가장 간단한 방법은 **Spread 문법(...)** 활용하는 것이다.

  - 예시 코드

  ```react
  const [gameHistory, setGameHistory] = useState([]);
  
  const handleRollClick = () => {
      const nextNum = random(6);
      setGameHistory(...gameHistory, nextNum);
  };
  ```



#### 컴포넌트의 활용

- 컴포넌트의 장점

  - 반복적인 개발이 줄어든다.
  - 오류를 고치기 쉽다.
  - 일을 쉽게 나눌 수 있다.

- 컴포넌트 재사용

  - 자식 컴포넌트의 State를 부모 컴포넌트로 올려주는 걸 **state lifting**이라고 한다. 
    - 각 컴포넌트를 한 곳에서 관리하고 싶으면 부모 컴포넌트로 옮겨 props로 내려줄 수도 있다.
  - 예시

  ```react
  // Board.js
  import Dice from './Dice';
  
  function Board({ name, color, gameHistory }) {
    const num = gameHistory[gameHistory.length - 1] || 1;
    const sum = gameHistory.reduce((a, b) => a + b, 0);
    return (
      <div>
        <h1>{name}</h1>
        <Dice color={color} num={num} />
        <h2>총점</h2>
        <p>{sum}</p>
        <h2>기록</h2>
        <p>{gameHistory.join(', ')}</p>
      </div>
    );
  }
  
  export default Board;
  ```

  ```react
  // App.js
  import { useState } from 'react';
  import Board from './Board';
  import Button from './Button';
  
  function random(n) {
    return Math.ceil(Math.random() * n);
  }
  
  function App() {
    const [myHistory, setMyHistory] = useState([]);
    const [otherHistory, setOtherHistory] = useState([]);
  
    const handleRollClick = () => {
      const nextMyNum = random(6);
      const nextOtherNum = random(6);
      setMyHistory([...myHistory, nextMyNum]);
      setOtherHistory([...otherHistory, nextOtherNum]);
    };
  
    const handleClearClick = () => {
      setMyHistory([]);
      setOtherHistory([]);
    };
  
    return (
      <div>
        <Button onClick={handleRollClick}>던지기</Button>
        <Button onClick={handleClearClick}>처음부터</Button>
        <div>
          <Board name="나" color="blue" gameHistory={myHistory} />
          <Board name="상대" color="red" gameHistory={otherHistory} />
        </div>
      </div>
    );
  }
  
  export default App;
  ```



### 리액트가 렌더링하는 방식

- 기존에 자바스크립트에서는 어떤 값이 변경되어 있을때 아래의 코드처럼 주사위의 어떤 값이 변경되었을 때 새로운 값을 업데이트 하는 코드를 작성하게 된다.

```js
const handleRollClick = () => {
  roll();
  diceElement.setAttribute("src", `./images/dice-${num}.svg`);
  sumElement.innerText = sum;
  historyElement.innerText = gameHistory.join(", ");
}
```

- 위의 코드처럼 작성하게 된다면 번거롭고, 실수로 작성하면 일부만 업데이트 되서, 버그가 발생하기도 쉽다. 
- 리액트는 그래서 쉽고 간단한 방법을 사용하는데, 새로 rendering을 해버린다.
- 예시코드(변경된 코드)

```react
const handleRollClick = () => {
  const nextNum = Math.ceil(Math.random() * 6);
  setNum(nextNum);
  setSum(sum + nextNum);
  setGameHistory([...gameHistory, nextNum])
}
```

- 위의 코드처럼 직접 요소가 변경되는 것이 아니라, state값이 변경되면서 함수를 실행하면서 react가 새롭게 적용된 element를 return한다.

- 여기서 의문점 아무 변화 없는 요소도 rendering되는 거 아닌가?
  - **React는 Virtual Dom(가상돔)이라는 것을 활용한다.**
    - 기본적으로 HTML 요소들은 DOM 트리라고 하는 자료구조로 저장되어 있다.
    - React 내부에서는 어떤 element가 변경되면 실제 DOM에 바로 반영하는 것이 아니라 가상돔에 먼저 적용하고, 변경 전의 가상돔과 변경 후의 가상돔을 비교한다.
    - 실제로 바뀐 부분만 찾아내고, 각각의 해당하는 실제 DOM 노드를 변경한다.
  - 가상돔의 장점
    - 개발자가 직접 DOM 노드를 신경써야 할 이유가 없어서 **단순하고 깔끔한 코드를 작성할 수 있다. **
      - 무슨 데이터를 어떻게 보여줘야 하는지만 신경쓰면 된다.
    - React가 변경사항을 적당히 나눠서 브라우저에 전달한다. 그래서 **변경사항들을 효율적으로 처리가능하다.**



### 인라인 스타일

- html 속성처럼 React에서도 인라인 스타일은 `style` 속성을 지정하면 된다.
- 그러나 html 속성과 다르게 React에서는 문자열이 아닌 객체로 style 속성 값을 지정할 수 있다.

- 예시

```react
const style = {
  backgroundColor: "pink",
}

function Button({ color, children, onClick }) {
  return (
    <button style={style} onClick={onClick}>
      {children}
    </button>
  );
}

export default Button;
```

- React에서는 CSS 속성은 **camel case로 적어줘야 한다. **

- 예시

```react
const style = {
  backgroundColor: "pink",
}

function Button({ color, children, onClick }) {
  return (
    <button style={{ backgroundColor: "yellow"}} onClick={onClick}>
      {children}
    </button>
  );
}

export default Button;
```

- 위처럼 객체 형태의 값을 속성값으로 작성할 수 있다. 그러나 이런 방식은 적용해야 할 style이 많아질 수록 복잡해진다.

- 예시

```react
//Button.js
const baseButtonStyle = {
  padding: '14px 27px',
  outline: 'none',
  cursor: 'pointer',
  borderRadius: '9999px',
  fontSize: '17px',
};

const blueButtonStyle = {
  ...baseButtonStyle,
  border: 'solid 1px #7090ff',
  color: '#7090ff',
  backgroundColor: 'rgba(0, 89, 255, 0.2)',
};

const redButtonStyle = {
  ...baseButtonStyle,
  border: 'solid 1px #ff4664',
  color: '#ff4664',
  backgroundColor: 'rgba(255, 78, 78, 0.2)',
};

function Button({ color, children, onClick }) {
  const style = color === 'red' ? redButtonStyle : blueButtonStyle;
  return (
    <button style={style} onClick={onClick}>
      {children}
    </button>
  );
}

export default Button;


// App.js
function App() {

  return (
    <div>
      <Button color="blue" onClick={handleRollClick}>던지기</Button>
      <Button color="red" onClick={handleClearClick}>처음부터</Button>
    </div>
  );
}

```

- 위의 코드처럼 공통 css 요소는 따로 style로 만들어 주고 차이점이 있는 부분은 추가로 만들고 spread문법을 사용해서 펼쳐줘서 서로 다르게 적용되어야 하는 style만 각 객체에 따로 추가해준 것이다.

- 이미지 사용
  - import로 가져온 값을 template literal 문자열로 넣어준다.

```react
import backgroundImg from "./assets/purple.svg";

const style = {
  backgrounImage = `url('${backgroundImg}')`
}
```





### CSS 클래스네임

- CSS 속성을 담은 파일을 외부 파일로 만들고 나서 import해서 사용한다. 
  - import하고 바로 파일 경로를 적어준다.

```react
// index.css
body {
  background-color: "blue";
  color: "#fff";
}

// index.js
import "./index.css"
```



- 예시 코드

```css
/*Button.css*/
.Button {
  padding: 14px 27px;
  border-radius: 9999px;
  outline: none;
  font-size: 17px;
  cursor: pointer;
}

.Button.blue {
  border: solid 1px #7090ff;
  color: #7090ff;
  background-color: rgba(0, 89, 255, 0.2);
}

.Button.red {
  border: solid 1px #ff4664;
  color: #ff4664;
  background-color: rgba(255, 78, 78, 0.2);
}
```

```react
// Button.js
import './Button.css';

function Button({ className = '', color = 'blue', children, onClick }) {
  const classNames = `Button ${color} ${className}`;
  return (
    <button className={classNames} onClick={onClick}>
      {children}
    </button>
  );
}

export default Button;
```

- 위에 Button 뒤에 공백이 없으면 하나의 className으로 인식한다.

- 예시

```css
.App .App-button {
  margin: 6px;
}
```

```react
import { useState } from 'react';
import Board from './Board';
import Button from './Button';
import './App.css';

function App() {

  return (
    <div className="App">
      <Button className="App-button" color="blue">
        던지기
      </Button>
      <Button className="App-button" color="red">
        처음부터
      </Button>
    </div>
  );
}

export default App;
```

- 요소에 외부적으로 영향을 끼치는 스타일 속성은 App.js에서 하는 것이 좋다. 
  - button 내부의 스타일은 button 내부에서 작성하는 게 좋지만 margin과 같이 스타일 외부에 영향을 끼치는 경우에 요소 주변에 어떤 요소들이 배치되면 좋을지 생각해야 한다.



- CSS 클래스를 사용하는 쪽에서 className을 집어넣는 식으로 하면 컴포넌트의 사용성이 좋아진다.

- 예시

```react
//App.js
import HandIcon from './HandIcon';
import "./HandButton.css"

function HandButton({ value, onClick }) {
  const handleClick = () => onClick(value);
  return (
    <button onClick={handleClick} className="HandButton">
      <HandIcon value={value} className="HandButton-icon"/>
    </button>
  );
}

export default HandButton;
```

```react
import rockImg from './assets/rock.svg';
import scissorImg from './assets/scissor.svg';
import paperImg from './assets/paper.svg';

const IMAGES = {
  rock: rockImg,
  scissor: scissorImg,
  paper: paperImg,
};

// className prop을 추가했다.
function HandIcon({ value, className }) {
  const src = IMAGES[value];
  return <img src={src} alt={value} className="className"/>;
}

export default HandIcon;
```



#### 편리하게 클래스네임을 쓰는 방법

- 일반 배열을 사용한 예

```react
function Button({ isPending, color, size, invert, children }) {
  const classNames = [
    'Button',
    isPending ? 'pending' : '',
    color,
    size,
    invert ? 'invert' : '',
  ].join(' ');
  return <button className={classNames}>{children}</button>;
}

export default Button;
```

- 위의 코드처럼 작성하면 지저분하고 매번 반복되는 코드를 작성한다는 번거로움이 존재
- 그래서 `classnames`라는 라이브러리를 사용한다.

- classNames를 사용한 예

```react
import classNames from 'classnames';

function Button({ isPending, color, size, invert, children }) {
  return (
    <button
      className={classNames(
        'Button',
        isPending && 'pending',
        color,
        size,
        invert && 'invert',
      )}>
     { children }
   </button >
  );
}

export default Button;
```

