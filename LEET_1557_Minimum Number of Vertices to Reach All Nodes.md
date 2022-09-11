# LeetCode/1557/Minimum Number of Vertices to Reach All Nodes

#그래프#배열

## Want

`0 ~ n - 1` 값을 가지는 `n`개의 Directed acyclic graph인 `edges`가 배열로 주어진다  
`edges[i] = [from, to]`를 의미하며 각 간선을 의미한다  
그래프의 **모든 노드에 연결할 수 있는 가장 작은 정점 집합**을 찾아 반환하라  
오직 하나의 정답만 존재한다

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {number[]}
 */
```

## Solving Strategies

모든 노드를 방문하기 위한 가장 적은 노드 집합을 구하는 것은  
어떤 노드에서 연결되는 노드를 포함하면 안 된다(중간 노드)  
다시 말해서, 방향이 시작되는 노드들을 모두 포함하여야 하므로  
**방향이 시작되는(다른 노드를 가리키기만 하는) 노드들**을 구해주면 된다

### solve 1

1. `edges`를 순회하며 각 노드가 가리키는 `to`만을 `paths` 배열에 저장한다
2. `paths` 배열을 순회하며 아무것도 가리키지 않는 값들을 `result` 배열에 저장한다(배열의 값이 0인 인덱스)
3. `return`

### solve 1 Code

```js
const findSmallestSetOfVertices = function (n, edges) {
  const paths = new Array(n).fill(0);
  const result = [];

  edges.forEach(([_from, to]) => paths[to]++);
  paths.forEach((path, index) => !path && result.push(index));

  return result;
};
```

## 배운 점 or 주의할 점

**사고의 유연함은 경험과 지식에서** 나온다
