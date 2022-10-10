# LeetCode/797/All Paths From Source to Target

#그래프#재귀

## Want

`0 ~ n - 1` 값을 가지는 `n`개의 Directed acyclic graph가 주어진다  
**Directed acyclic graph란?**  
방향성은 존재하지만, 순환되지는 않는 그래프를 의미한다  
`graph[i]`에 존재하는 수들은 `i`값을 가지는 그래프에서 이동할 수 있는 노드를 의미한다  
`0`번 노드부터 `n - 1`번째 노드까지의 모든 경로를 배열 형태로 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[][]} graph
 * @return {number[][]}
 */
```

## Solving Strategies

주어지는 그래프는 2차원 배열 형태로 주어진다  
따라서 각 값을 순회하며 다시 `graph`의 노드를 찾는 형식으로 하면 좋을 듯하다

1. 0번째 노드를 순회하며 연결된 노드들의 인덱스를 찾아간다
2. `n - 1`번째 노드를 가리킨다면 `result`에 `push`
3. 2번 조건을 만족하지 않는다면 현재 값 `graph[node]`를 다시 순회
4. 3번의 각 값인 `value`를 `current`에 넣으며 다시 재귀 호출

콜백함수나 재귀함수를 사용하는 것이 제일 좋아보임

### solve 1

문제의 요지는 아래 2가지로 정의된다

1. 경로 저장하여 기억, `result`에 추가
2. 이전과 다른 방법으로 경로 탐색 시, 이전 저장값 삭제

위 2개를 구현하기 위해선 재귀 함수가 제일 적합한 방법이었다  
따라서 위의 1번은 첫번째 조건문에서 해결해주었고  
2번은 스프레드 문법을 활용하여 재귀 호출이 종료되었을 때도 원본 배열이 손상되지 않도록 하였다  
`Array.prototype.push()`와 같은 메서드를 사용하면 원본 배열이 변경되어  
재귀 호출 종료 시에도 추가된 값이 저장되어 있기 때문이다

### solve 1 Code

```js
// Runtime: 125 ms, faster than 82.62%
// Memory Usage: 53.2 MB, less than 11.03%
const allPathsSourceTarget = function (graph) {
  const result = [];

  const findPath = (current, node) => {
    if (node === graph.length - 1) {
      result.push([...current, node]);
      return;
    }
    graph[node].forEach((vertex) => findPath([...current, node], vertex));
  };

  findPath([], 0);
  return result;
};
```

## 배운 점 or 주의할 점

본 문제와 같이 경로를 저장하여 기억할 때는 `Array.prototype.push()`가 아닌  
**스프레드 문법**을 사용했다는 게 주요점이었다  
처음에 `push`를 사용해 원하는 결과가 나오지 않았고, 그 원인을 찾아보니 원본 배열의 변경이 문제가 되었었다  
아마 이 문제를 제일 깔끔하게 해결한 코드이지 않을까 싶다

자바스크립트가 정말 재밌는 점은 **언어에 대한 이해도가 높아지면 높아질수록**  
재밌는 코드들을 다양하게 작성할 수 있다는 점이다

[Discuss in Leetcode](https://leetcode.com/problems/all-paths-from-source-to-target/discuss/2556536/Javascript-Easiest-Solve)
