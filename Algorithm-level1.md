#### 수박수박수박수박수?

- 문제
  - 길이가 n이고 "수박수박수박"와 같은 패턴을 유지하는 문자열을 리턴하는 함수를 solution을 완성해라

- 처음 코드

```js
function solution(n) {
    let str = "";

    for (let i = 0; i < n; i++) {
        if (i % 2 == 0) {
            str += "수"
        } else {
            str += "박"
        }
    }
    return str;
}
```

- 개선 코드

```js
function solution(n) {
  return "수박".repeat(n).slice(0, n);
}
```



#### 약수의 개수와 덧셈

- 두 정수 left와 right가 매개변수로 주어진다. left부터 right까지의 모든 수들 중에서, 약수의 개수가 짝수인 수는 더하고, 약수의 개수가 홀수인 수는 뺀 수를 return해라

- 나의 코드

```js
function gcd(number) {
    let cnt = 0;
    for (let i = 1; i <= number; i++) {
        if (number % i == 0) {
            cnt++;
        }
    }
    return cnt;
}

function solution(left, right) {
    let answer = 0;
    
    for (let i = left; i <= right; i++) {
        if (gcd(i) % 2 == 0) {
            answer += i;
        } else {
            answer -= i;
        }
    }
    return answer;
}
```

- 다른 사람의 코드
  - **제곱근이 정수면 약수의 개숫가 홀수이다.**

```js
function solution(left, right) {
  let answer = 0;
  
  for (let i = left; i <= right; i++) {
    if (Number.isInteger(Math.sqrt(i))) {
      answer -= i;
    } else {
      answer += i;
    }
  }
  return answer;
}
```



#### 문자열 내림차순으로 배치하기

- 난 아스키코드를 활용해서 풀었다. 
  - 그러나 map을 중복해서 사용하기 때문에 별로 좋지 않은 거 같다.

- 초기 코드

```js
function solution(s) {
  return s.split("").map(e => e.charCodeAt()).sort((a, b) => b - a).map((e) => String.fromCharCode(e)).join("");
}
```

- 수정된 코드

```js
function solution(s) {
  return s.split("").sort().reverse().join("")
}
```





#### 문자열 다루기 기본

- 문자열 길이가 4 혹은 6이고, 숫자로만 구성돼있는지 확인해주는 함수
  - 조건에 부합하지 않으면 False, 부합하면 true를 리턴
- 이 문제는 정규표현식을 활용하면 코드가 간단하다

- 코드
  - `$` 이 문자는 문자열의 끝을 가리킨다. `^`은 문자열의 시작을 나타낸 것이니 그 사이에 `\d`는 숫자를 의미한다.

```js
function solution(n) {
  return (n.length == 4 && n.length == 6) && /^\d+$/.test(s);
}
```





#### 행렬의 덧셈

- 두 행령의 같은 행, 같은 열의 값을 서로 더한 결과 됩니다.

```js
function solution(arr1, arr2) {
    let arr = [];
    
    for (let i = 0; i < arr1.length; i++) {
        arr.push(arr1[i].map((_, idx) => arr1[i][idx] + arr2[i][idx]));
    }
    return arr;
}
```





#### 같은 숫자는 싫어

- 배열 안에 반복적인 숫자가 등장하면 하나만 제외하고 나머지는 배열에서 제외시킨다. 단 순서는 유지해야 한다.
- 내 코드
  - for 반복문을 활용해서 해결했다.

```js
function solution(arr) {
   let answer = [arr[0]];
    
   for (let i = 0; i < arr.length -1; i++) {
       if (arr[i] !== arr[i+1]) {
           answer.push(arr[i+1]);
       }
   }
   return answer;
}
```

- 다른 사람 코드

```js
function solution(arr) {
  return arr.filter((num, idx) => num !== arr[idx+1])
}
```



#### 예산

- 부서에서 요청한 물품에 대한 금액을 주어진 예산 안에서 최대로 구매할 수 있는 물품의 개수를 return하는 문제이다.

- 내 코드

```js
function solution(d, budget) {
    let answer = 0;
    d = d.sort((a, b) => a - b);
    
    let cnt = 0, request = 0;;
    
    for (let i = 0; i < d.length; i++) {
        if (request + d[i] <= budget) {
            request += d[i];
            cnt++;
        }
    }
    return cnt;
}
```

- 다른 사람 코드
  - `reduce`메서드를 활용해서 boolean을 활용한다. 남은 예산이 0이상이면 1 아니면 false가 반환되어 0이 된다. 

```js
function solution(d, budget) {
  return d.sort((a, b) => a - b).reduce((count, price) => {
    return count + ((budget -= price) >= 0);
  }, 0);
}
```



#### 3진법 뒤집기

- n을 3진법 상에서 앞뒤로 뒤집은 후 이를 다시 10진법으로 표현한 수를 return 해라
- 내 코드

```js
function solution(n) {
  return parseInt(n.toString(3).split("").reverse().join(""), 3);
}
```





#### 크기가 작은 부분문자열

- 숫자로 이루어진 문자열 t와 p가 주어졌을 때, t에서 p의 길이와 같은 부분문자열 중에서 이 부분문자열이 나타내는 수가 p가 나타내는 수보다 작거나 같은 것이 나오는 횟수를 return해라
- 정답 코드

```js
function solution(t, p) {
    let answer = 0;
    // p크기만큼 슬라이스해야 한다. 
    for (let i = 0; i <= t.length - p.length; i++) {
        let a = t.slice(i, i+p.length);
        if (a <= p) answer++;
    }
    return answer;
}
```



#### 이상한 문자 만들기

- 문자열 s는 한 개 이상의 단어로 구성되어 있다. 각 단어의 짝수번째 알파벳은 대문자로, 홀수번째 알파벳은 소문자로 바꾼 문자열을 리턴해라
- 문자열 전체의 짝/홀수 인덱스가 아니라, 단어(공백을 기준)별로 짝/홀수 인덱스를 판단해야한다.

- 내 코드

```js
function solution(s) {
    let str = "";
    // 공백 문자열 기준으로 split
    let string = s.split(" ");
    for (let i = 0; i < string.length; i++) {
      // 처음부터 공백이 들어가면 정답이 나오지 않기 때문에 다음 순서부터 넣는다.
        if (i !== 0) str+= " ";
        for (let j = 0; j < string[i].length; j++) {
          // 짝수면 대문자
            if (j % 2 == 0) {
                str += string[i][j].toUpperCase();
              // 홀수면 소문자
            } else {
                str += string[i][j].toLowerCase();
            }
        }
    }
    return str;
}
```

- 수정된 코드
  - `map` 메서드를 활용해서 코드를 더 줄일 수 있다.

```js
function solution(s) {
  return s
  		.split(" ")
      .map(word =>
          word)
  					.split("")
            .map((char, idx) => 
                   idx % 2 == 0 ? char.toUpperCase() : char.toLowerCase()
            )
    				.join("")
  ).join(" ")
}
```



#### 삼총사

- 학교 학생 3명의 정수 번호를 더했을 때 0이 되면 삼총사라고 한다. 학생들 중 삼총사를 만들 수 있는 방법의 수를 return해라
- 충분히 이 문제는 for문을 활용해서 해결할 수 있다. 하지만 재귀 함수 방식을 이용해서 해결했다.
- 내 코드
  - 백트래킹 방법이 시간복잡도에서도 좋은 거 같다.
    - 함수 조건 불만족시 이전 호출 상태로 돌아간다.

```js
function solution(number) {
    let cnt = 0;
    
    function findCombinations(start, num, sum) {
       // 3개의 숫자를 골랐으면
        if (num == 3) {
           // 그 중에서 합계가 0이면 count하고 종료한다.
            if (sum == 0) {
                cnt++;
            }
            return;
        }
        // 시작지점에서 number의 길이만큼 돈다.
        for (let i = start; i < number.length; i++) {
           // 재귀 방식으로 조건에 만족할 때까지 돈다.
            findCombinations(i+1, num+1, sum+number[i]);
        }
    }
    findCombinations(0, 0, 0);
    return cnt;
}
```

- for문을 활용한 코드

```js
function solution(number) {
  let answer = 0;
  
  for (let i = 0; i < number.length - 2; i++) {
    for (let j = i+1; j < number.length -1; j++) {
      for (let k = j+1; k < number.length; k++) {
        const sum = number[i] + number[j] + number[k];
        if (sum == 0) answer++;
      }
    }
  }
  return answer;
}
```



#### 가장 가까운 같은 글자

- 문자열 s가 주어졌을 때, s의 각 위치마다 자신보다 앞에 나왔으면서, 자신과 가장 가까운 곳에 있는 같은 글자가 어디에 있는지 찾는 문제

- 내 코드

```js
function solution(s) {
    let temp = [], answer = [];
    let alphabet = s.split("");
    for (let alpha of alphabet) {
        // 현재 순회하는 알파벳의 index가 존재하는지 파악한다.  
        let current = temp.indexOf(alpha)
        if (current < 0) {
            answer.push(-1);
        } else {
            // 앞에 있는 글자의 경우 현재 배열의 길이 - 자신보다 앞에 있는 원소의 인덱스
            answer.push(temp.length - temp.lastIndexOf(alpha))
        }
        temp.push(alpha);
    }
    return answer;
}
```

- 다른 사람의 코드
  - `map` 메서드를 순회하면서 객체에 삼항연산자를 통해서 값이 없으면 -1 아니면 현재 인덱스 - 존재하는 알파벳 인덱스 처리하면 된다.
  - 참고로 destructuring array를 사용하면 문자열을 배열로 변환시켜 준다. 

```js
function solution(s) {
  let hash = {};
  
  return [...s].map((v, i) => {
    let result = hash[v] !== undefined ? i - hash[v] : -1;
    hash[v] = i;
    return result;
  })
}
```



#### 숫자 문자열과 영단어(카카오 2021 채용연계형 인턴십 문제)

- 주어진 문자열에서 문자를 해당하는 숫자로 바꾸는 문제

- 내 코드

```js
function solution(s) {
    let result = "";
    let string = { "zero" : "0", "one" : "1", "two" : "2", "three" : "3", "four" : "4", "five" : "5", "six" : "6", "seven" : "7", "eight" : "8", "nine" : "9"};
    let temp = ""
    for (let str of s.split("")) {
       // 숫자가 아닌경우 문자열에 계속 문자를 더한다.
        if (/\D/.test(str)) {
            temp += str;
          // 만약에 객체에서 해당 값을 찾게 되면 결괏값에 해당 value를 더해준다.
            if (string[temp]) {
                result += string[temp];
                temp = "";
            }
          // 숫자인 경우 그냥 더한다. 
        } else {
            result += str;
        }
    }
    return Number(result)
}
```

- 다른 사람 코드

```js
function solution(s) {
   let numbers = ["zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"];
   let answer = s;
   for (let i = 0; i < numbers.length; i++) {
     // 숫자를 기준으로 split한다. 
     let arr = answer.split(numbers[i]);
     // i를 join한다. 문자열을 붙인다. 
     answer = arr.join(i);
   }
}
```





#### 정렬

- 배열의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, K번째 있는 수를 구해라.

- 내 코드

```js
function solution(array, commands) {
    let answer = [];
    for (let command of commands) {
       // destructuring array 활용
        let [start, end, num] = command;
       // slice 한 후에 정렬
        let subArr = array.slice(start-1, end).sort((a, b) => a - b);
        answer.push(subArr[num-1]);
    }
    return answer;
}
```



#### 두 개 뽑아서 더하기

-  numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해라

- 내 코드

```js
function solution(numbers) {
    let answer = new Set();
    for (let i = 0; i < numbers.length-1; i++) {
        for (let j = i+1; j < numbers.length; j++) {
            answer.add(numbers[i] + numbers[j]);
        }
    }
   // Set은 sort 메서드가 없으니 배열로 다시 바꾸는 작업을 진행
    return [...answer].sort((a, b) => a - b);
}
```





#### 푸드 파이트 대회

- 1대 1로 경기를 진행하면서 균등하게 음식을 먹어야 한다. 음식이 1개이면 버려야 한다. 대회를 위한 음식의 배치를 나타내는 문자열을 return해라
- 내 코드

```js
function solution(food) {
  // food[1]부터 순회하면서 음식의 개수를 / 2로 나눈 것을 내림차순하고 그것만큼 문자열을 반복해라
    let one = food.map((e, i) => i > 0 ? i.toString().repeat(Math.floor(e / 2)) : "").join("");
  // 상대방의 경우 reverse를 하면 된다. 
    return one + "0" + [...one].reverse().join("");
}
```





#### 문자열 내 마음대로 정렬하기

- 문자열로 구성된 리스트 strings가 주어졌을 때 인덱스 n번째 글자를 기준으로 오름차순 정렬한다. 

```js
function solution(strings, n) {
    return strings.sort((a, b) => {
        const chr1 = a.charAt(n);
        const chr2 = b.charAt(n);

        if (chr1 == chr2) {
            return (a > b) - (a < b);
        } else {
            return (chr1 > chr2) - (chr1 < chr2);
        }
    })
}
```





#### 비밀지도 - 2018 KaKao Blind recruitment

- 원래의 비밀지도를 해독하여 `#`, 공백으로 구성된 문자열 배열로 출력하라.
- 내 코드 
  - 불필요한 코드도 많다.

```js
function solution(n, arr1, arr2) {
    let answer = [];
    
    let arr = Array.from(Array(n), () => Array(n).fill(0));
     
    // 1. 각 숫자를 이진수로 변경
    let one = arr1.map((e) => (n == e.toString(2).length ? e.toString(2) : "0".repeat(n - e.toString(2).length) + e.toString(2)));
    // 2.
    let two = arr2.map((e) => (n == e.toString(2).length ? e.toString(2) : "0".repeat(n - e.toString(2).length) + e.toString(2)));
    
    
    for (let i = 0; i < one.length; i++) {
        let row = ""
        for (let j = 0; j < one[0].length; j++) {
            if (one[i][j] == 1 || two[i][j] == 1) {
                row += "#";
            } else {
                row += " ";   
            }
        }
        answer.push(row);
    }
    return answer;
}
```

- 개선된 코드
  - 비트연산자를 사용하고 `padStart`메서드와 `replace` 메서드를 사용하면 해결할 수 있다. 
    - **비트연산자를 활용하면 두 숫자를 비트 단위로 비교해, 두 비트 중 하나라도 1이면 1을 반환한다. **

```js
function solution(n, arr1, arr2) {
    let answer = [];
    
    return arr1.map((e, i) => 
                   (e | arr2[i])
                   .toString(2)
                   .padStart(n, 0)
                   .replace(/1/g, "#")
                   .replace(/0/g, " ")
                   )
}
```

