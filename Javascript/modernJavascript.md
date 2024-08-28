### 데이터 타입의 특징과 종류

- 자바스크립트는 다른 프로그래밍 언어에 비해서 데이터 타입이 상황에 따라 변할 수 있다.
  - 파이썬의 경우 서로 다른 데이터 타입끼리 연산할 수 없고
  - C, Java의 경우 변수에도 데이터 타입을 지정해야 한다. 타입이 정해진 변수에 다른 타입은 할당 불가하다.
- 기본형
  - Number, String, Boolean, Null, undefined, Symbol, BigInt(Es2020)
    - Symbol은 유일한 값을 만들 때 사용하는 타입
    - BigInt는 엄청 큰 숫자를 다룰 때 
- 참조형
  - Object



#### Symbol && BigInt

- Symbol

  - 코드 내에서 유일한 값을 가진 변수 이름을 만들 때 사용한다. 
  - 아래의 코드처럼 Symbol 값을 담게 된 user라는 이름의 변수는 다른 어떤 값과 비교해도 true가 될 수 없는 고유한 변수가 된다. 

  ```js
  const user = Symbol("this is a user");
  
  user === 'this is user'; // false
  user === 'user'; // false
  user === 'Symbol'; // false
  user === true; // false
  user === false; // false
  user === 123; // false
  user === 0; // false
  user === null; // false
  user === undefined; // false
  ```

  

- BigInt

  - 자바스크립트에서 아주 큰 정수를 표현하기 위해 등장한 데이터 타입이다.
  - 자바스크립트에서는 안전한 정수 표현의 한계가 존재한다.
    - `2**53 -1`이 그렇다. 약 9000조 정도의 정수 표현의 한계가 존재한다.
  - 만약 아주 큰 숫자를 다루거나 굉장히 정확한 연산이 필요한 상황에서 이보다 더 큰 숫자가 필요할 수 있다. 그럴 때 사용한다.
  - **일반 정수 마지막에 알파벳 n을 붙이거나 BigInt라는 함수를 사용하면 된다**

  ```js
  console.log(9007199254740993n); // 9007199254740993n
  console.log(BigInt('9007199254740993')); // 9007199254740993n
  ```

  - 주의할 점이 있다. BigInt는 큰 정수를 표현하기 위한 데이터 타입이기 때문에 소수 표현에는 사용할 수 없다.

  ```js
  1.5n; // syntaxError
  ```

  - 그래서 소수 형태의 결과가 리턴되는 연산은 소수점 아랫부분은 버려지고 정수 형태로 리턴된다.

  ```js
  10n / 6n // 1n
  ```

  - BigInt 타입끼리만 연산할 수 있고, 서로 다른 타입끼리의 연산은  명시적으로 타입 변환을 해야 한다.

  ```js
  3n * 2 // TypeError
  3n * 2n // 6n
  Number(6n) * 2 // 12
  ```

  

### typeof 연산자

- 우리가 사용하는 값이 어떤 데이터 타입을 가지고 있는지 확인하려면 `typeof`연산자를 사용해야 한다.

```js
typeof 'Codeit'; // string
typeof Symbol(); // symbol
typeof {}; // object
typeof []; // object
typeof true; // boolean
typeof(false); // boolean
typeof(123); // number
typeof(NaN); // number
typeof(456n); // bigint
typeof(undefined); // undefined
```

- null은 object가 리턴된다. 이유는 특별한 문법 설계 때문이다. 현재까지 수정되지 않고 있따.

- 자바스크립트에서 함수는 객체로 취급된다. 그렇다고 해서 typeof 연산자를 활용하면 객체가 리턴되지 않고 function이 리턴된다.

```js
function sayHi() {
  console.log('Hi!?');
}

typeof sayHi; // function
```



#### 불린인 듯 불린 아닌 불린같은 값

- Falsy값
  - false, null, undefined, NaN, 0
- Truthy
  - 빈 배열, 빈 객체 및 나머지 값들 



#### AND와 OR의 연산 방식

- AND
  - 왼쪽이 true일 때 오른쪽 값을  return 한다.
  - 왼쪽이 false일 때 왼쪽 값을 return 한다.

```js
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false
console.log(false && false); // false
console.log("Code" && "JavaScript")// 왼쪽에 truthy하기 때문에 오른쪽 JS가 출력
```

- OR

```js
// OR
console.log(true || true); // true
console.log(true || false); // true
console.log(false || true); // true
console.log(false || false); // false
```

- 예시

```js
console.log(null & undefined); // null
console.log(0 || true); // true
console.log("0" && NaN); // NaN
console.log({} || 123 ); // {}
```

- 응용

```js
function print(value) {
     const message = value || 'Code';

     console.log(message);
}
// 함수 호출했을 때 빈 값이면 코드를 출력
print();
// 값이 존재하면 value를 출력한다.
print('JavaScript');
```



- **AND와 OR 연산자 사이에서는 AND 연산자의 우선순위가 더 높다**

```js
console.log(true || false && false); // true
console.log((true || false) && false); // false
```



- null 연산자(`??`)의 경우 **연산자 왼편의 값이 null 이나 undefined라면 연산자의 오른편이 값이 되고 왼편의 값이 null, undefined가 아니면 연산자 왼편의 값이 리턴된다.**

```js
const example1 = null ?? "I";
const example2 = undefined ?? "love";
const example3 = "You" ?? "JavaScript";

console.log(exmample1, example2, example3) // I love You
```

- null연산자랑 OR연산자는  약간의 차이가 있다. null은 undefined와 null이면 오른쪽 값이 되는 것이고, OR연산자는 falsy값만 확인하기 때문에 결괏값이 다르다.

```js
const title1 = false || 'code';
const title2 = false ?? 'code';

console.log(title1); // code
console.log(title2); // false

const width1 = 0 || 150;
const width2 = 0 ?? 150;

console.log(width1); // 150
console.log(width2); // 0
```



