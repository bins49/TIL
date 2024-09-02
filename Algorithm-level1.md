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

