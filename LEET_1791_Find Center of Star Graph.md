# LeetCode/1791/Find Center of Star Graph

#그래프

## Want

`1 ~ n` 값을 갖고 있는 `n`개 노드를 소유한 **별** 형태의 무방향 그래프가 존재한다  
별 그래프의 중앙 노드는 `n - 1`개의 간선으로 다른 모든 노드와 연결되어 있다  
간선을 나타내는 2차원 배열이 주어졌을 때,  
`edges[i] = [u, v]`는 `u`와 `v`가 간선으로 연결되어 있다는 것을 의미한다  
중앙 노드의 값을 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[][]} edges
 * @return {number}
 */
```

## Solving Strategies

노드의 최대값은 노드의 개수를 의미한다  
따라서 (`노드 개수의 최대값 - 1 === 중앙 노드와 연결된 간선의 수` 조건을 만족하는 노드의 값 반환

1. 각 간선을 나타내는 배열의 최대값 구하여 노드의 개수 구하기
2. 각 간선을 나타내는 배열의 값 개수를 구하여 중앙 노드 구하기
3. 중앙 노드 값 반환

### solve 1

`Solving Strategies`를 그대로 코드화하였다  
하지만 내가 한 가지 간과한 것이 있었다  
별 형태의 그래프는 중앙 노드를 제외한 나머지 노드들은 서로 연결이 안되는 형태였다  
이것을 알지 못한 채 다른 간선이 있을 때를 제외하기 위해  
노드의 최대값을 통하여 노드의 개수를 구해주었다 (`nodeCount`)  
**시간 복잡도 O(N)**

### solve 1 Code

```js
// Runtime: 305 ms, faster than 16.64%
// Memory Usage: 78.7 MB, less than 16.64%
const findCenter = function (edges) {
  const edgeObj = {};
  let nodeCount = -Infinity;
  let result;

  edges.forEach((nodes) => {
    nodeCount = Math.max(nodeCount, ...nodes);

    nodes.forEach((node) => {
      edgeObj[node] = (edgeObj[node] || 0) + 1;
    });
  });

  Object.entries(edgeObj).forEach(([node, edgeCount]) => {
    if (edgeCount === nodeCount - 1) {
      result = +node;
    }
  });

  return result;
};
```

### solve 2

별 형태의 그래프의 특징을 파악한 후에는 훨씬 더 간단하게 해결할 수 있었다  
두 개의 간선만 비교하여 **공통적으로 가리키는 노드**를 찾으면 되었다  
왜냐하면 별 형태 그래프의 특성상 `edges`의 요소에 존재하는 모든 노드 중  
**하나는 무조건 중앙 노드를 가리키고 있어야** 하기 때문이다  
**시간 복잡도 O(1)**

### solve 2 Code

```js
// Runtime: 124 ms, faster than 85.28%
// Memory Usage: 57 MB, less than 83.94%
const findCenter = (edges) => {
  const [[a, b], [c, d]] = edges;
  return a === c || b === c ? c : d;
};
```

## 배운 점 or 주의할 점

어쩌면 문제를 풀면서 배우는 것도 좋은 방법이겠지만,  
모르는 개념이나 모델이 등장했을 땐 **검색 후 특징을 잘 파악**하는 것이 문제 풀이에 도움이 될 수 있다고 여겨졌다
