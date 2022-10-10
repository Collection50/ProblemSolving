# LeetCode/77/Combinations

#백트래킹#DFS#조합

## Want

정수 `n`, `k`가 주어진다  
조합 nCk를 배열 형태로 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
```

## Solving Strategies

조합은 **순서 상관없이 n개 중 k개를 뽑는 것**을 의미한다  
단순 순열이 아니라 조합인데, 어떻게 생각해야 할까?  
더하여 **반복문은 절대 사용할 수 없어**  
반복문을 사용할 경우 **동적으로 `k`개의 반복문을 사용**해야 하기 때문이다  
즉, **재귀를 활용하여 해결**해야 한다

1. `n === k` 혹은 `k === 0` 일 경우 예외 처리
2. 방문 처리를 위한 객체 생성
3. 반복문 사용, 1부터 시작, 1을 방문처리, 나머지 2, 3, 4 배열에 담기
4. 다음 반복문, 2 방문처리, 1,2가 방문처리 되어 있으므로 3, 4 배열에 담기
5. 반복

Fail...  
해결하지 못하여 다른 사람의 풀이를 참조하였다

### solve 1

풀이를 설명하자면 아래와 같다  
1 ~ n까지의 수 중 k가지를 뽑는 nCk를 구해야 한다

1. 조합할 수를 배열에 담고, 수 + 1을 `start` 매개변수로 전달한다
2. `current` 배열에 k개가 담겼다면 `result`에 값을 `push`
3. `k`개가 담기지 않았다면, `current` 배열에 `start` 값을 추가하여 다시 `dfs` 재귀 함수를 호출한다
4. `result` 반환

위 과정에서 자연스럽게 중복이 제거된다  
1을 시작으로 하는 조합을 구할 땐 1 + 1의 값이 `start`로 주어지고  
2를 시작으로 하는 조합을 구할 땐 2 + 1의 값이 `start`로 주어지기 때문이다

### solve 1 Code

```js
// Runtime: 310 ms, faster than 12.26%
// Memory Usage: 51.7 MB, smaller than 13.50%
const combination = (n, k) => {
  const result = [];

  function dfs(current, start) {
    if (current.lenht === k) {
      result.push(current);
      return;
    }

    for (let i = start; i <= n; i++) {
      dfs([...current, i], i + 1);
    }
  }

  dfs([], 1);
  return result;
};
```

### solve 2

위의 방법은 단순히 `DFS`를 활용한 완전 탐색 풀이이지 **백트래킹**을 활용한 풀이는 아니다  
이 경우가 조건에 부합하는지 확인하지 않고 모든 경우의 수를 탐색하기 떄문이다  
따라서 오랜 시간이 소요된 걸 볼 수 있는데  
나는 완전 탐색이 아닌 백트래킹을 활용하여 문제를 풀고 싶었다

백트래킹을 가능케할 조건은 아래와 같다

**조합으로 사용할 수 있는 남은 숫자 개수의 합이 `k`보다 작다면 `return`**
`size`: 현재 배열에 담긴 숫자의 개수  
`n`: 전체 숫자의 개수  
`start + 1`: 조합에 담으려고 하는 숫자의 현재 위치
즉, `size + (n - start + 1) < k`라면 아무리 재귀와 반복문을 실행해도 `k`개의 값을 얻을 수 없다  
결과적으로 두 배와 가까운 시간을 절약하며 빠르게 문제를 해결할 수 있었다

### solve 2 Code

```js
// Runtime: 159 ms, faster than 64.83%
// Memory Usage: 51.4 MB, smaller than 19.72%
const combine = function (n, k) {
  const result = [];

  function dfs(current, start) {
    const size = current.length;
    if (size === k) {
      result.push(current);
      return;
    }
    if (size + (n - start + 1) < k) return;

    for (let i = start; i <= n; i++) {
      dfs([...current, i], i + 1);
    }
  }
```

## 배운 점 or 주의할 점

재귀는 컴퓨팅 사고력을 향상시키기에도 좋은 방법인 것 같아 더 잘하고 싶게 만든다  
백트래킹 문제같이 점점 고난도 재귀 문제들이 출몰하고 있다  
파이팅
