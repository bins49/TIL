#### 알고리즘 문제 풀이

- 숫자를 문자로 변환하는 메서드

  - `.split("")`
  - `[...num_str]`
- 합산하는 메서드

  - `.reduce((a, b) => a + b, a의 초기값)`

  - `.map((a) => b += a)`
- 문자에서 숫자로 변환하는 메서드
  - `parseInt(문자)` 
    - 정수형
  - `parseFloat(문자)`
    - 실수형
  - `Number(문자)`
    - 정수형 & 실수형
- 숫자에서 문자로 변환하는 메서드
  - `String(숫자)`
  - `숫자.toString(진법)`
  - `숫자.toFixed(소수자리수)`
- 정렬하는 메서드

  - `배열.sort()`
    - 원본 객체를 바꾸는 메서드
    - `배열.sort((a,b) => a - b)`  => 오름차순
    - `배열.sort((a,b) => b - a)`  => 내림차순

  - `배열.sorted()`
    - 원본 객체를 지키고 새로운 복사본을 반환하는 메서드
- 배열의 특정 부분을 자르는 메서드

  - `배열.slice(시작 지점, 끝 지점)`
    - 시작 지점부터 끝 지점-1까지의 값을 복제한다.
    - 인자에 아무것도 넣지 않으면 처음부터 끝까지 모든 값들을 복사한다.
  - `배열.splice(시작 지점, 삭제할 개수)`
    - 특정 범위를 삭제하거나 새로운 값을 추가 또는 기존의 값을 대체 가능하다. 
      - `배열.splice(10, 2, -10, -11)`
        - 배열 10의 인덱스부터 2개의 값을 -10, -11로 변경
    - 첫번째 인자만 넘기면 첫번째 인자부터 배열의 마지막까지 삭제된다. 



- 배열의 길이의 따라 다른 연산

  - 배열의 길이가 홀수이면 배열의 짝수 인덱스 더하기 반대도 존재

  ```js
  function solution(arr, n) {
    if (arr.length % 2 == 0) {
      return arr.map((number, idx) => idx % 2 === 1 ? number + n : number);
    } else {
      return arr.map((number, idx) => idx % 2 === 0 ? number + n : number);
    }
  }
  ```




- 자바스크립트 template literal(문자열 표기법)
  - `${변수}`



- 만약에 배열의 원소의 수만큼 새로운 배열을 만들어야 한다면 다음과 같이 reduce 메서드를 사용해서 간편하게 구현 가능하다.

  - 예시 1

  ```js
  return arr.reduce((list, num) => [...list, ...new Array(num).fill(num)], []);
  ```

  - 예시 2

  ```js
  return arr.reduce((list, num) => list.concat(Array(num).fill(num)), []);
  ```



- 특정 문자열 치환

  - 특정 문자열을 치환하려면 `replace` 메서드를 많이 사용한다. 

    - 한개만 치환하는 경우

    ```javascript
    return rny.replace("m", "rn");
    ```

    - 모든 요소 치환하는 경우

    ```js
    return rny.replaceAll("m", "rn");
    ```

    

- 배열 반복문

  - `for ~ of` vs `for - in`

    - **for ~ of는 배열에서 사용되고 for ~ in은 객체에서 사용된다.**

- 문자열 포함 여부

  - `string.indexOf()`
    - 문자열에 어떤 문자열이 포함되어있는지 확인
    - 인자로 전달된 문자열이 존재하면 그 문자열이 위치한 index를 리턴한다. 존재하지 않으면 -1을 리턴
  - `string.includes()`
    - 인자로 전달된 문자열이 문자열 안에 존재한다면 true가 리턴, 존재하지 않는다면 false를 리턴한다. 

- 필터링 메서드

  - `list.filter((요소, 인덱스))`
    - 만약 조건에 해당하는 값만 return하고 싶다면 map보다는 filter가 낫다. map은 if else문을 사용해야 한다. 

  - 참고로 false인 친구들을 찾으려면 `!`을 앞에 사용하면 된다. 

- 리스트끼리 합치기

  - `list1.concat(list2)`
  - unshift를 사용해도 된다.
    - 새로운 요소를 앞에 추가한다.
    - `list.unshift(...list.splice(n))`

- 비구조화 할당

  - `...list`
    - 나머지 원소를 가리킨디.

- **for문과 forEach문의 차이**

  - 가장 큰 차이는 동기와 비동기의 차이이다.
    - `for`문은 동기 방식이기 때문에 for문 안에 오류가 나면 에러 위치 이후의 동작은 멈춰버린다
    - `forEach`문은 ES6문법이고 콜백함수를 뿌린다 비동기 방식이기 때문에 for문 안에 오류가 발생하더라도 멈추지 않고 동작한다. 
    - `forEach`문은 향상된 for문이라고 칭하고 가변적인 배열이나 리스트 크기를 구할 필요가 없어 복잡한 반복문에 적합하며, 인덱스를 생성하여 접근하는 for문보다 수행속도가 더 빠르다.
      - 내장 함수이기 때문에 인위적으로 인덱스를 생성하여 접근하는 것보다 빠를 수밖에 없다. 
  - 그러나 `forEach`단점도 존재한다.
    - 배열이나 리스트의 값을 변경하거나 추가할 수 없다. 
      - 쉽게 말해 배열의 `push`, `pop`, `unshift`, `shift` 등과 같은 메서드 사용 불가

- 조건에 해당하는 값의 인덱스 찾기
  - `list.findIndex(요소 => 조건문)`



- 기본형은 독립적인 값을 가지고, 참조형은 독립적인 값을 가지지 않는다. 
- 기본 객체의 고유값을 유지하면서 새로운 객체의 값을 변경하려면 복사를 해야한다. 

- 상수는 대문자로 사용하고 스네이크 방식을 사용해서 작명한다. 
- **배열이나 객체의 경우 const로 사용되는 경우 배열, 객체의 주소값을 변경하는 것이 아니기 때문에 변수의 값이 충분히 변할 수 있다. **





#### 정규표현식

- 문자열.replace("/패턴/g", "")

  - 보통 slash(/)와 slash(/) 사이에 검색할 문자열 패턴을 넣고, 순서에 필요에 따라 플래그를 추가한다. 

- 플래그

  - 순서와 상관없이 하나 이상의 플래그를 동시에 설정 가능하다. 

  - **플래그를 사용하지 않는 경우 검색 대상이 1개 이상이더라도 첫번째 조건 대상만 검색하고 종료하게 된다**

  - 대표적인 플래그

    `i(ignore case): 대소문자를 구별하지 않고 검색한다`

    `g(global): 문자열 내의 모든 패턴을 검색한다.`

    `m(multi line): 문자열의 행이 바뀌더라도 검색은 계속한다.`

- 패턴

  - 기본적으로 literal 문자(정규 문자)와 meta 문자(정규 표현식의 구문 문자)로 구분된다. 

  - 메타 문자

    - 매칭 패턴이라고 부르고 다음과 같이 구성되어 있다. 
    - 조금 더 복잡하고 다양한 케이스를 다룰 때 사용된다.

    `a-zA-Z: 영어알파벳(-으로 범위 지정)`

    `ㄱ-ㅎ가-힣: 한글 문자(-으로 범위 지정)`

    `0-9: 숫자(-으로 범위 지정)`

    `.: 모든 문자열(숫자, 한글, 영어, 특수기호, 공백 모두! 단 줄바꿈X)`

    `\d: 숫자`

    `\D: 숫자가 아닌 것`

    `\w: 영어 알파벳, 숫자, 언더스코어(_)`

    `\W: /w가 아닌 것`

    `\s: space 공백`

    `\S: space 공백이 아닌 것`

    `\특수기호: 특수기호`

    `g: 모든 문자 검색`

     `i: Ignore Case 대소문자 구분 안함`

  - 검색 패턴

    `[]: 괄호안의 문자들 중 하나`

    `[^문자]: 괄호안의 문자를 제외한 것`

    `^문자열: 특정 문자열로 시작(괄호 없음!)`

    `문자열$: 특정 문자열로 끝나다`

    `(): 그룹 검색 및 분류(match 메서드에서 그룹별로 묶어준다)`

    `(?: 패턴): 그룹 검색(분류 X)`

    `\b: 단어의 처음/끝`

    `\B: 단어의 처음/끝이 아님`

- RegExp 생성사 함수 방식

`const regexp = new RegExp(/^abc/i)`



- 정규표현식의 method 

  - exec

    - 대상을 검색하여 조건에 부합하는 결과를 배열로 반환한다.
    - 조건에 부합하는 결과가 1개 이상이라도 무조건 부합하는 결과의 첫번째 값을 반환한다.

    ```javascript
    console.log(/S/.exec("RegExp Study Start"));
    // 결과값 : ["S", index: 7, input: "RegExp Study Start"]
    ```

  - test

    - 대상의 매치 여부를 boolean 값으로 반환한다.

    ```javascript
    console.log(/S/.exec("RegExp Study Start"));
    // 결과값: true
    ```

    

- 문자열의 method

  - match

    - 정규식의 부합하는 문자열을 배열 형태로 반환해 준다. 없으면 null 반환

    ```javascript
    console.log("RegExp Study".match(/Study/))
    // 결과값: ["Study", index:7, input: "RegExp Study"]
    ```

  - search

    - 정규식 조건에 부합하는 문자열의 index 번호를 반환해준다 없으면 -1을 반환한다.

    ```javascript
    console.log("RegExp Study".search(/Study/));
    // 결과값: 6
    ```

  - replace

    - 조건에 부합하는 문자열을 찾아, 다른 텍스트로 변환시킨다.

    ```js
    console.log("RegExp Study".replace("Study", "Test"));
    // 결과값: RegExp Test
    ```

  - split

    - 조건에 부합하는 값을 기준으로 대상을 자른 후, 배열로 저장한다.

    ```js
    console.log("RegExp Study".split(" "));
    // 결과값: ["RegExp", "Study"]
    ```

    



#### 최댓값 만들기 문제 



```js
function solution(numbers) {
  numbers.sort((a, b) => a - b);
  
  let n = numbers.length;
  return Math.max(numbers[0] * numbers[1], numbers[n-1] * numbers[n-2]);
}
```





#### 접두사 찾는 문제

```js
function solution(my_string, is_prefix) {
  // +를 사용하면 문자를 숫자로 변경 (true => 1)
  return +my_string.startsWith(is_prefix)
}
```





#### 문자열 뒤집기 

`문자열.reverse()`



#### 부분 문자열 반환

```js
문자열.substring(시작범위, 끝범위)
```



#### 특정 원소 찾기

```js
// 옵션으로 앞에 + 기호를 붙이면 true가 1이 된다. 
해당 배열.includes(원소) 
```



#### 부분 문자열 이어 붙여 문자열 만들기

```js
function solution(my_strings, parts) {
    return parts.map(([a, b], i) => {
        return my_strings[i].slice(a, b+1)
    }).join("")
}
```





#### 이차원배열 만들기

```js
function solution(n) {
  let answer = Array.from(Array(n), () => Array(n).fill(0));
  
  for (let i = 0, i < n; i++) {
    answer[i][i] = 1;
  }
  
  return answer;
}
```



#### 간단한 식 계산하기

```js
const ops = {
  "+": (a, b) => a + b,
  "-": (a, b) => a - b,
	"*": (a, b) => a * b,
}
function solution(binomial) {
  const [a, op, b] = bionomial.split(" ");
  return ops[op](+a, +b);
}
```





#### 재귀 방식

```js
function solution(n, arr = []) {
  arr.push(n);
  if (n === 1) return arr;
  if (n % 2 === 0) return solution(n / 2, arr)
  return solution(3 * n + 1, arr)
}
```



#### 문자열 변환

```js
function solution(n) {
  return n.toString().split("").map((e) => "abcdefghij"[e]).join("")
}
```



#### 가까운 1 찾기

```js
function solution(arr, idx) {
    for(let i = idx ; i < arr.length; i++) {
        if (arr[i] === 1) return i
    }
   // 위에서 만약에 해당하는 값이 없으면 -1이다. 위에서 if else 문으로 하면 그냥 1이 나오기전에 0이 나오면 false.
    return -1
}
```



#### findIndex() vs indexOf()

- `findIndex()`는 배열에서 일치하는 인덱스를 찾을때, `indexOf()`는 배열, 문자열에서 일치하는 인덱스를 찾을때 사용한다. 





#### 수 조작하기2

```js
function solution(numLog) {
  const convert = {
    "1" : "w", "-1" : "s", "10" : "d", "-10": "a"
  }
  
  return numLog.slice(1).map((v, i) => {
    return convert[v - numLog[i]]
  }).join("")
}
```



#### 줄 변경

```js
for(i = 0; i < a; i++) {
    for(j = 0; j <= i; j++) {
       star += '*';
    }
    // 줄변경
    star += '\n';
  }
console.log(star);
```



#### 특정 인덱스 변경

- 구조분해할당을 사용하자

```js
function solution(my_string, num1, num2) {
    let a = my_string.split("");
    
    [a[num1], a[num2]] = [a[num2], a[num1]];
    return a.join("");
}
```



#### 빈 배열에 추가, 삭제하기

- 어떤 한 수만큼 배열에 계속 추가하고 싶으면 2중 for문을 고려해야 한다.

```js
function solution(arr, flag) {
  let answer = [];
  
  for (let i = 0; i < flag.length; i++) {
    if (flag[i]) {
      for (let j = 0; j < arr[i] * 2; j++) {
        answer.push(arr[i])
      }
    } else {
      for (let j = 0; j < arr[i]; j++) {
        answer.pop()
      }
    }
  }
  return answer
}
```



#### 글자 지우기

- Filter 기능을 사용해서 지우려고 하는 문자열의 인덱스가 지우려고 하는 인덱스에 포함되어 있는지 체크 필요

```js
function solution(my_string, indices) {
  return [...my_string].filter((_, i) => !indices.includes(i)).join("")
}
```





#### 문자열 뒤집기

- 특정 문자열을 뒤집으려면 뒤집을 영역을 변수로 빼놓고, 영역을 뒤집은 후에 replace 메서드를 사용해서 전환시킨다.

```js
function solution(my_string, s, e) {
  let str = my_string.substring(s, e+1);
  let reverse = [...str].reverse().join("");
  
  answer = my_string.replace(str, reverse);
  return answer;
}
```



#### 숨어있는 숫자의 덧셈

- 키포인트
  - 문자가 아닌 것을 기준으로 split을 하면 숫자만 배열에 남는다. 

```js
function solution(my_string) {
    var answer = 0;
    return my_string.split(/\D+/).reduce((prev, now) => prev += Number(now), 0);
}
```



#### 가까운 수

- array 안에 있는 수 중에서 정수 n과 가장 가까운 수 
  - Tip 정렬할 때 절댓값 메서드를 활용해서 계산해준다.
  - 절댓값을 활용해서 계산한 결과가 같으면 숫자가 작은대로 정렬해줘야 하니 or 연산자를 활용해서 정렬시킨다.

```js
function solution(array, n) {
  return array.sort((a, b) => Math.abs(n - a) - Math.abs(n - b) || a - b);
}
```





#### 배열의 길이를 2의 거듭제곱으로 만들기

- 먼저 2의 거듭제곱이 되기 위해 필요한 수를 `log2`라는 메서드를 통해 반올림해서 구한다
- 현재에 길이에서 부족한 만큼 `new Array`메서드를 활용해서 숫자를 채운다. 

```js
function solution(arr) {
  let len = arr.length;
  const totalLength = 2 ** Math.ceil(Math.log2(len));
  
  return [...arr, ...new Array(totalLength - len).fill(0)];
}
```





#### 진료순서 정하기

- 반드시 배열을 정렬할 때 배열을 복사하는 방식인 `slice`를 활용하자
- console.log를 반드시 찍어보자.

```js
function solution(emergency) {
    // 순서를 매긴다. 내림차순
    // [3, 76, 24] => [76, 24, 3]
    // 현재 배열에서 해당 바뀐 배열에서 자신의 번호의 인덱스를 찾는다. + 1;
    let new_sort = emergency.slice().sort((a,b) => b - a);
    return emergency.map((e) => new_sort.indexOf(e)+1)
}
```



#### 한 번만 등장한 문자

- 2중 for문을 돌려서 중복 문자를 찾고 만약 숫자가 1개라면 문자열을 더한다.
- 이것보다 빠른 방법은 `indexOf` 와 `lastIndexOf`를 활용한다. 만약 두개의 인덱스가 같다면 1개밖에 존재하지 않는다고 보고 해당 값을 배열에 추가해서 `join` 메서드를 활용해서 더해준다.

```js
function solution(s) {
    let answer = [];
    
    for (let c of s) if (s.indexOf(c) === s.lastIndexOf(c)) answer.push(c);
    
    return answer.sort().join("");
}
```



### 세 개의 구분자

- a, b,c를 제외한 나머지 알파벳을 찾아야 한다. 

- 기존의 코드

```js
function solution(myStr) {
    let answer = [];

    let str = myStr.replace(/[abc]/g, " ").split(" ").filter((e) => e !== "");

    if (str.length === 0) {
        str.push("EMPTY")
    }

    return str;
}
```

- RegExp를 잘 사용했지만 뭔가 복잡하다 그래서 간단하게 줄일 수 있다. `match` 메서드를 활용하자.
  - Or 연산자 활용해서 없으면 EMPTY로 한 것도 인상적이다. 

```js
const solution = s => s.match(/[^a-c]+g/) || EMPTY
```



#### 문자열 묶기

- 문자열들끼리 그룹을 묶었을 때 가장 개수가 많은 그룹의 크기를 return해야 한다. 
- 이럴 때 가장 좋은 방법은 `Map()`을 활용하는 것이다. 

```js
function solution(strArr) {
  const counter = new Map();
  for (const str of strArr) {
    counter.set(str.length, (counter.get(str.length) || 0) + 1);
  }
  return Math.max(...counter.values());
}
```

### 리스트 자르기 

- 대놓고 n = 1, n = 2 일 때 등 조건을 줬기 때문에 이런 경우에 `switch`문을 활용하면 좋다.

```js
function solution(n, slicer, num_list) {
  let [a, b, c] = [...slicer];
  
  switch(n) {
    case 1:
      return num_list.slice(0, b + 1);
    case 2:
      return num_list.slice(a);
    case 3:
      return num_list.slice(a, b+1);
    case 4:
      return num_list.slice(a, b+1).filter((_, idx) => !(idx % c));
  }
}
```



#### 조건에 맞게 수열 변환하기2

- 여기서 핵심은 문제 설명대로 코드를 짜야하고 내가 몰랐던 부분은 while문의 조건이었고, 이 문제의 경우 arr가 변할 때까지니까 무한루프로 설정하고 현재 배열과 값을 새로 입힌 배열이 같을 때 return한다.

- 여기서 새로 알게 된 점은 두 배열을 같은지 비교할 때 `toString()`메서드를 활용하면 좋다. 
- `toString(2)`의 경우 10진수에서 2진수로 변환하는 법이다.

```js
function solution(arr) {
  let x = 0;
  while(true) {
    let next = [];
    for (let a of arr) {
      if (a >= 50 && a % 2 == 0) {
        next.push(a / 2)
      } else if (a < 50 && a % 2 == 1) {
        next.push(a * 2 + 1)
      } else {
        next.push(a)
      }
    }
    if (arr.toString() === next.toString()) {
      return x;
    } else {
      x++;
      arr = next;
    }
  }
}
```



#### 공 던지기

- 구차하게 길게 필요없이 나머지 연산자를 활용해서 쉽게 다음 순서를 예측할 수 있다.

- 다음으로 어떻게 갈 것인가를 생각하지 말고 최종 값에만 집중하자. 

```js
function solution(numbers, k) {
  return numbers[(2 * (k -1)) % numbers.length]
}
```





#### 영어가 싫어요

- 영어 단어를 숫자로 치환하는 문제다. 
- obj을 만들어서 해당 문자가 있으면 숫자로 바꾸고 변수를 다시 빈 문자열로 바꾸는 코드를 구성했다. 
- 그러나 다른 사람의 코드에서 더 간단한 방법을 찾았다. replace를 활용하는 것이다.
- **replace에 두 번째 인자에 콜백이 들어가도 된다.**

```js
function solution(numbers) {
  const obj = {zero: 0, one: 1, two: 2, three:3, four: 4, five:5, six:6, seven:7, eight:8 nine:9}
  
  const num = numbers.replace(/one|two|three|four|five|six|seven|eight|nine/g, (v) => {
    return obj[v]
  })
  
  return Number(num);
}
```



#### 두 수의 합

- `BigInt` 은 숫자 타입 Number가 표현할 수 없는 범위를 넘어서는 정수를 표현할 때 사용한다. 



#### 문자열 여러 번 뒤집기

- 이 문제의 핵심은 결국 원소의 구간을 활용해서 배열의 위치를 변환하는 것이다.

```js
function solution(my_string, queries) {
  let str = my_string.split("");
  for (let query of quries) {
    let [a, b] = query;
    while (a < b) {
      [[str[a], str[b]]] = [[str[b], str[a]]];
      start++;
      end--;
    }
  }
  return str.join("");
}
```





#### 구슬을 나누는 경우의 수

- 이 문제의 경우 팩토리얼 재귀함수를 활용해야 하는 문제다. 
- 재귀 함수의 가장 큰 핵심은 만든 함수가 끝나는 조건을 잘 설정해야 한다. 
- 위의 경우 나누는 볼이 없거나 구슬이나 나누는 볼의 수가 같은 경우를 고려해서 구성했다.

```js
function key(balls, share) {
  if (share == 0 || balls == share) {
  	return 1  
  } else {
    return key(balls-1, share-1) + key(balls-1, share);
  }
}

function solution(balls, share) {
  return key(balls, share);
}
```



### 삼각형의 완성조건

- 이거 패턴을 찾아야해.. 
- 가장 간단한 답은 배열의 최솟값에서 2를 곱하고 -1을 하면 답이 나온다.

```js
function solution(sides) {
  return Math.min(...sides) * 2 - 1;
}
```

#### 무작위로 K개의 수 뽑기

- Set => array로 변환하려면 `[...]`을 사용하면 된다. 

```js
function solution(arr, k) {
  let answer = [...new Set(arr)].slice(0, k);
  while (answer.length < k) answer.push(-1);
  return answer;
}
```

#### 수열과 구간 쿼리2

- 이 문제에서 구간  slice를 하고 정렬 혹은 최솟값 구하는 함수를 사용해서 구한다. 

```js
function solution(arr, queries){
  let answer = [];
  
  return queries.map(([s, e, k]) => {
    const filtered = arr.slice(s, e + 1).filter(num => num > k);
    return filtered.length ? Math.min(...filtered) : -1;
  }
}
```



#### 정사각형으로 만들기

- 행과 열 중에서 더 큰 값을 찾아내고 같아질 때까지 0을 추가한다. 
- 사실 열 크기가 max랑 같아질 때까지 0을 추가하면 답이 된다. 

```js
function solution(arr) {
  let column = arr.length;
  let row = arr[0].length;
  
  const max = Math.max(column, row);
  
  for (let i = 0; i < max; i++) {
    arr[i] = arr[i] || []
    while (arr[i].length < max) {
      arr[i].push(0)
    }
  }
  return arr;
}
```



#### 외계어 사전

- 사전에 담긴 배열이 알파벳을 포함하고 있는 것이 있으면 1 아니면 2를 출력하는 문제
- 이건 모든 수의 조합을 찾아서는 답이 없다... 알파벳 배열의 크기가 많으면 답이 없기 때문이다.
- 여기서 `some`을 활용하면 된다. 어차피 1개라도 있으면 1을 return하면 되기 때문이다. 

```js
function solution(spell, dic) {
  return dic.some(e => spell.sort().toString() === [...s].sort().toString()) ? 1 : 2;
}
```



#### 그림 확대

- 이 문제에서는 문자열 배열을 가로 세로 k배 만큼 늘리는 문제다.
- 나는 2중 for문까지 잘 접근했는데 padStart도 잘 떠올렸다. 중요한 문제가 있다. 
- 반복한 부분 성공했는데 이후에 해당 문자열을 k만큼 늘려야 한다. 
- 하지만 이것 보다 더 간단한 방법이 있다. 
- `flatmap`을 활용해서 1차원 배열로 평탄화 시키고 spread 연산자 활용해서 배열을 풀어주고 `map` 을 활용해서 각 원소에 `repeat`하고 join한다. 
- 그리고 새로 배열을 만드는 과정이기에 굳이 push 메서드 사용하지말고 `Array().fill()`을 활용한다. 

```js
function solution(picture, k) {
  return picture.flatMap(p => {
    let str = [...p].map((e) => e.repeat(k).join(""));
    return Array(k).fill(str);
  })
}
```



#### 로그인 성공

- 주어진 배열의 id와 password가 db에 있는지 확인하기 
- map에 있는지 확인하려면 `has` 메서드 
- map에 value 확인하려면 `get` 메서드 

```js
function solution(id_pw, db) {
  let [id, pass] = id_pw;
  
  const map = new Map(db);
  
  return map.has(id) ? (new.get(id) === pass ? "login" : "wrong pw") : "fail"
}
```



#### 전국 대회 선발 고사

- 이 문제는 등수가 높은 애들 3명이 출전이 가능하면 점수를 환산하는 문제이다.
- 앞에서 3명을 뽑으면 되기 때문에 분해 구조 할당을 사용해서 3개만 뽑는다.
- `map`메서드를 활용해서 rank 배열을 순회하면서 나중에 해당 번호의 기존 인덱스 파악을 위해 indexf를 인자로 받아온다.  
- filter 메서드 활용해서 참석 여부가 가능한지 파악한다. **arr[i] 이 뜻은 boolean 연산자를 활용하는 것이다.**
- 마지막으로 배열을 정렬한다. [등수, 인덱스]이기 때문에 배열로 a, b를 묶어야 한다. 

```js
function solution(rank, attendance) {
  const [a,b,c] = rank
  	.map((e, i) => [e, i])
    .filter((_, i) => attendance[i])
  	.sort(([a], [b]) => a - b);
  
  return 10000 * a[1] + 100 * b[1] + c[1];
}
```



#### 등수 매기기

- 주어진 score가 2중 배열이고, 원소가 2개의 수로 이루어진 배열이다. 
- 배열의 평균을 내야하고, 배열을 하나 복사해서 내림차순으로 정렬한다.
- 높은 점수 순서로 정렬을 진행한 배열의 숫자의 기존 배열에서 인덱스를 찾는다. 

```js
function solution(score) {
  let answer = [];
  const avg = score.map((e) => (e[0] + e[1] / 2));
  let arr = avg.slice().sort((a, b) => b - a);
  
  for (let i = 0; i < arr.length; i++) {
    answer[i] = arr.indexOf(avg[i]) + 1;
  }
  return answer;
}
```



#### 치킨 쿠폰

- 치킨 한 마리 당 쿠폰을 한 장 발급하는데, 10장 모으면 치킨 한 마리이다. 
- 서비스 치킨에도 쿠폰이 발급되는 문제다. 
- 내가 문제를 풀었는데 실수한 부분은 주문하고 남은 쿠폰의 수를 한 번에 계산하는 방식으로 했어야 했다. 
- while 조건문에 쿠폰 개수가 10이 넘지 않으면 종료되게 했어야 불필요한 계산을 멈출 수 있다.
- 항상 어떤 수를 나눌 때 정수로 변환하는 작업을 진행하자.  
- 수정 전 코드

```js
function solution(chicken) {
    let answer = 0;
    
    // 100마리를 주문하면 쿠폰 100장 발급되므로 서비스 치킨 10마리 주문 가능
    // 10마리 주문하면 쿠폰 10장 => 쿠폰 10장은 치킨 한마리로 교체 가능
    
    let coupon = chicken;

    // 가장 작은 unit => 더이상 주문할 치킨이 없다(쿠폰 x) => 최종
    
    while (true) {
        // 만약 쿠폰도 10개 아래이고 주문할 치킨이 없으면 정답 출력
        if (coupon < 10 && chicken == 0) {
            return answer;
        } 
        
        // 쿠폰은 주문하고 나머지
        coupon += chicken % 10;
        
        // coupon이 10장이면 치킨 한마리 추가;
        if (coupon == 10) {
            // 정답 1을 추가
            answer ++;
            // 쿠폰 리셋
            coupon = 0;
        }
        
        
        // 다음 주문할 치킨의 수를 업데이트
        chicken = parseInt(chicken / 10);
        // 정답에 추가
        answer += chicken;
        
    }
    return answer;
}
```

- 수정 후 코드

```js
function solution(chicken) {
  let answer = 0;
  let coupon = chicken;
  
  while (coupon >= 10) {
    let leftCoupon = coupon % 10;
    answer += parseInt(coupon / 10);
    coupon = parseInt(leftCoupon + coupon / 10);
  }
  return answer;
}
```



#### 코드 처리하기

- Code[idx]가 1일 때, 문자열일 때 분기 처리, mode가 0일때, 1일때 분기 처리하는 문제이다. 
- 내 코드는 동작이 잘 된다. 효율성도 괜찮다. 다만 중복된 코드가 있어 줄일 필요가 있다.
- 내 코드

```js
function solution(code) {
    // 시작할 때 mode는 0이며, ret이 빈 문자열이라면 EMPTY를 return한다.
    let ret = "";
    let mode = 0;
    // mode는 0과 1이 있으며, idx를 0 ~ code.length -1까지 +1해서 code[idx]의 값에 따라 다음과 같이 행동한다.
    for (let idx = 0; idx <= code.length-1; idx++) {
        if (mode == 0) {
            if (code[idx] !== "1") {
                if (idx % 2 === 0) ret += code[idx];
            } else if (code[idx] === "1") {
                mode = 1;
            }
        } else if (mode == 1) {
            if (code[idx] !== "1") {
                if (idx % 2 === 1) ret += code[idx];
            } else if (code[idx] === "1") {
                mode = 0;
            }
        }
    }

    return ret.length === 0 ? "EMPTY" : ret;
}
```



- 베스트 코드

```js
function solution(code) {
  let ret = "";
  let mode = 0;
  
  for (let i = 0; i < code.length; i++) {
    if (Number(code[i]) === 1) {
      code = code === 1 ? 0 : 1;
    }
    // 각 mode가 홀수 or 짝수일 때 값을 추가하는 경우라면 아래왁 같이 코드를 줄일 수 있다.
    if (Number(code[i]) !== 1 && i % 2 == mode) {
      ret += code[i];
    }
  }
  return ret.length > 0 ? ret : "EMPTY"
}
```



#### 배열 조각하기

- 이 문제는 짝수일 때 배열의 뒷 부분을 버리고 홀수일 때 앞부분을 자르는 문제이다. 
- 내가 실수한 부분은 slice할 때 arr에서 query의 값을 인덱스 하지 않고 바로 query의 값을 인덱스해도 되는 문제였다. 
- 내 코드

```js
function solution(arr, query) {
    let answer = 0;
    let i = 0;
    while (query.length > 0) {
        if (i % 2 == 0) {
            arr = arr.slice(0, arr[query[0]]);
        } else {
            arr = arr.slice(arr[query[0]]);
        }
        i++;
        query.shift();
    }
    return arr;
} 
```

- 수정된 코드

```js
function solution(arr, query) {
  let answer = 0; 
  for (let i = 0; i < arr.length; i++) {
    if (i % 2 == 0) {
      arr = arr.slice(0, query[i] + 1);
    } else {
      arr = arr.slice(query[i])
    }
  }
  return arr;
}
```



### 유한소수 판별하기

- 유한소수는 2와 5로만 구성이 되어 있어야 한다. 그 이외에 숫자로 구성된 경우 무한소수이다.
- 문제의 핵심은 분모가 최대공약수로 나눴을 때 2, 5로만 구성되어 있는지 판별하는 것이 중요하다. 
- 최대공약수로 분모를 나누는 데 까지 성공했으나, 그 다음에 2, 5로만 구성되어 있는지를 작성한 코드에서 문제가 발생했다.
- 아래의 코드에서 2와 5이외의 수가 남아있는지 확인하는 코드가 없어 유한소수 무한소수 판별이 어렵다.
- 초기 코드

```js
function solution(a, b) {
    let answer = 0;
    let gcd = 0;
    let decimals = [];
    let finit = [2, 5];
    // 최대 공약수 만들기
    for (let i = 1; i < b; i++) {
        if (b % i == 0 && a % i == 0) {
            gcd = i;
        }
    }
    // 분모를 통해 유한소수 및 무한소수 판별하기 때문에 최대공약수로 약분
    b /= gcd;
    // 소인수 찾기
    for (let i = 1; i <= b; i++) {
        if (b % i == 0) decimals.push(i); 
    }
    return decimals.some(v => finit.includes(v)) ? 1: 2;
}
```

- 수정된 코드

```js
function solution(a, b) {
  let gcd = 0;
  
  // 최대공약수 찾기
  for (let i = 1; i < b; i++) {
    if (a % i == 0 && b % i == 0) {
      gcd = i;
    }
  }
  // 분모를 최대공약수로 나누기
  b /= gcd;
  
  // 분모가 2와 5로만 구성되어 있는지 확인하기
  while (b % 2 === 0) {
    b /= 2;
  }
  while (b % 5 === 0) {
    b /= 5;
  }
  // 분모가 1이면 2와 5로만 구성되어 있는 유한소수라고 알 수 있다.
  return b == 1 ? 1 : 2;
}
```



### 다항식 더하기

- x는 x끼리, 숫자는 숫자끼리 더하는 문제이다. 
- 핵심은 x끼리 분리하고, 상수는 상수끼리 분리해서 합산해서 문자열을 만드는 문제

- 기존 코드

```js
function solution(polynomial) {
    let answer = [];
    // 동류항끼리 계산하기 위해 X의 합계
    let xSum = 0;
    // 상수의 합
    let numberSum = 0;
    
    // x가 포함되어 있는지 여부를 판단하기 위해, 주어진 입력을 + 기준으로 split
    let polys = polynomial.split(" + ");
    
    // x가 포함되어 있는지 안 되었는지 분기를 통해서 값을 합산한다.
    for (let poly of polys) {
        if (poly.includes("x")) {
            // x가 1이면 그냥 더하고
            if (poly === "x") {
                xSum ++;
                // x가 2 이상이라면 x를 제거해주고 상수만 더하기
            } else {
                xSum += parseInt(poly.replace("x", ""));
            }
            // 상수는 상수끼리
        } else {
            numberSum += parseInt(poly);
        }
    }
    
    // x를 뒤에 붙이기 0x는 넣으면 안되기 때문에 분기처리 필요!
    if (xSum) {
        if (xSum === 1) {
            answer.push("x")
        } else {
            answer.push(`${xSum}x`)
        }
    }
    
    // 상수는 문자열로 변환하기
    if (numberSum) {
        answer.push(numberSum.toString());
    }
    
    return answer.join(" + ");
}
```



#### 최빈값 구하기

- 배열에서 같은 수의 개수를 count해주고 거기 중에서 최빈값을 출력하고 혹시 최빈값이 2개이면 -1을 출력하는 문제
- 수정 전 코드(맞는 코드)
  - 배열을 제한사항에 명시된 대로 1000개를 0으로 채워서 만들고 각 숫자의 개수를 세준다.
  - 거기서 최빈값이 1이면 최댓값을 업데이트 하고 그렇지 않으면 -1을 리턴한다.

- 원래 코드

```js
function solution(array) {
  let arr = new Array(1000).fill(0);
  let answer = 0;
  let max = Math.max(...arr)
  
  for (let i = 0; i < array.length; i++) {
    arr[array[i]]++;
  }
  
  for (let i = 0; i < arr.length; i++) {
    if (max < arr[i]) {
      max = arr[i];
      answer = i;
    } else if (max == arr[i]) {
      answer = -1;
    }
  }
  return answer;
}
```

- 수정된 코드
  - ES6부터 나온 Map을 활용하면 충분히 코드를 줄일 수 있다. 

```js
function solution(array) {
  const m = new Map();
  for (let n of array) m.set(n, (m.get(n) || 0) + 1);
  m = [...m].sort((a, b) => b[1] - a[1]);
  return m.length === 1 || m[0][1] > m[1][1] ? m[0][0] : -1;
}
```



#### OX 퀴즈

- 주어진 배열에서 X [연산자] Y = Z 식으로 들어있는데 이게 맞는 수식이면 "O" 틀리다면 "X"를 리턴해야 한다.
- 초기코드
  - 맞췄으나 너무 중복된 코드가 많다. 

```js
function solution(quiz) {
    let answer = [];
    
    
    for (let i = 0; i < quiz.length; i++) {
        let str = quiz[i].split(" ");
        if (str[1] == "+") {
            if (Number(str[0]) + Number(str[2]) == Number(str[4])) {
                answer.push("O")
            } else {
                answer.push("X")
            }
        } else if (str[1] == "-") {
            if (Number(str[0]) - Number(str[2]) == Number(str[4])) {
                answer.push("O")
            } else {
                answer.push("X")
            }
        }
    }
    return answer;
} 
```

- 수정된 코드

```js
function solution(quiz) {
  return quiz.map((q) => {
    const [a, op, b, equal, result] = q.split(" ");
    const cal = op === "+" ? Number(a) + Number(b) : Number(a) - Number(b);
    return cal == result ? "O" : "X"
  })
}
```



#### 다음에 올 숫자

- 등차수열, 등비수열의 배열이 주어질 때 마지막 원소 다음에 올 숫자를 리턴하는 문제
  - 앞의 3 원소의 패턴만 살피면 풀 수 있는 문제

```js
function solution(common) {
  let [a, b, c] = common;
  // 마지막 원소
  let lastElement = common[common.length - 1];
  
  return b - a = c - b ? lastElement += (b - a) : lastElement *= (b / a);
}
```

