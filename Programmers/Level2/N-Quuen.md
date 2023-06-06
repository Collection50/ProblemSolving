# Programmers/N-Queen

#DFS#백트래킹

## Want

체스판의 가로 세로의 세로의 길이 `n`이 주어진다  
`n`개의 퀸이 조건에 만족 하도록 배치할 수 있는 방법의 수를 반환하라  
[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/12952)

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @return {number}
 */
```

## Solving Strategies

유명한 `N-Queen` 문제

1. 1개 행과 1개 열에는 1개 `Queen`만 올 수 있다
2. 행에 1개씩 `Queen`을 넣는다
3. 유효성 검사
4. `Queen`을 놓을 수 있다면 놓는다
5. 놓을 수 없다면 `return` (백트래킹)
6. 마지막 행까지 도달했다면 `count++`

### solve 1

`Solving Strategies`의 코드화  
이 문제의 핵심은 `isSafe`에서 이뤄지는 최적화이다

1. 위쪽 `Queen`들에 대한 검사 (아래 `Queen`은 필요 없음)
2. 대각선에 있는 `Queen`을 검사할 때 사용하는 `Math.abs` (대각선 길이 검사)

### solve 1 Code

```js
function solution(n) {
  const row = Array.from({ length: n }, () => 0);
  let count = 0;

  const isSafe = (r, c) => {
    for (let i = 0; i < r; i++) {
      if (c === row[i] || Math.abs(c - row[i]) === r - i) {
        return false;
      }
    }
    return true;
  };

  const dfs = (r) => {
    if (r === n) {
      count++;
      return;
    }
    for (let c = 0; c < n; c++) {
      if (isSafe(r, c)) {
        row[r] = c;
        dfs(r + 1);
      }
    }
  };
  dfs(0);
  return count;
}
```

## 배운 점 or 주의할 점

백준에서는 백트래킹을 소개할 때 이 문제를 알려준다  
근데 소개라는 단어와는 어울리지 않는 난이도  
예전에는 손도 못댔었는데

`row`에 대한 질문을 조금 더 쉽게 해결하고자 간략한 해설을 작성한다  
2차원 배열이 아니라 1차원 배열만 사용하여 해결하는 방법을 표현하자면 아래와 같다

1. 1차원 배열 `row`의 각 인덱스는 행을 나타낸다
2. `row[i]`는 각 행에 존재하는 `Queen`의 열을 나타낸다

쉽게 말해서 `row[2] = 3`의 의미 => **2행 3열에 `Queen`이 존재**함을 의미한다

따라서 `r`행 `i`열에 `Queen`을 놓을 수 있는 경우(`isSafe === true`)  
해당 위치에 `Queen`을 할당한다

대각선에서 공격해오는 퀸에 대하여도 비교적 쉽게 확인할 수 있다  
행의 인덱스, 해당 인덱스가 가리키는 값이 각각 행, 열을 의미하므로  
뺄셈의 비교값을 확인한다면 같은 대각선에 있는지 확인할 수 있다

`행의 뺄셈값 === 열의 뺄셈값`이라면 대각선에 위치함을 의미하기 때문
