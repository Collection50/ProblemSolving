# LeetCode/1971/Find if Path Exists in Graph

#그래프

## Want

양방향 그래프에 `0 ~ n - 1` 값을 소유하는 `n`개의 정점이 존재한다  
`edges[i] = [u, v]`는 `u`와 `v`가 간선으로 연결되어 있다는 것을 의미하며  
모든 정점의 쌍은 최대 한 개의 간선으로 연결되어 있다  
출발점과 목적지가 주어졌을 때, 출발점에서 목적지로 연결되었는지 확인하여 불리언 값으로 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @param {number[][]} edges
 * @param {number} source
 * @param {number} destination
 * @return {boolean}
 */
```

## Solving Strategies

무한 루프에 돌지 않도록 해야 함
큐를 사용해야 하나..?

1. 노드의 연결 객체화
2. 1번을 토대로 시작점에서 목적지까지 갈 수 있는지 확인한다
3. `return`

### solve 1

1. `edges` 배열을 통해 각 노드 연결하기
2. 큐 생성한 후 시작 노드 넣기
3. 방문한 노드를 확인하기 위한 `visited` 생성하여 시작 노드 넣기
4. `queue`에서 노드 하나 꺼낸 후 인접한 노드를 큐에 넣고 방문처리
5. `queue.length`가 `0`일때까지 or 목적지에 도달할 때까지 반복

### solve 1 Code

```js
// Runtime: 755 ms, faster than 56.42%
// Memory Usage: 158.7 MB, less than 48.88%
const validPath = function (n, edges, source, destination) {
  const nodes = {};
  const queue = [source];
  const visited = new Set().add(source);

  edges.forEach(([node1, node2]) => {
    nodes[node1] = nodes[node1] || [];
    nodes[node1].push(node2);

    nodes[node2] = nodes[node2] || [];
    nodes[node2].push(node1);
  });

  while (queue.length) {
    const node = queue.shift();

    if (node === destination) {
      return true;
    }

    nodes[node].forEach((value) => {
      if (!visited.has(value)) {
        visited.add(value);
        queue.push(value);
      }
    });
  }

  return false;
};
```

## 배운 점 or 주의할 점

그래프는 실생활에서 굉장히 많이 사용되고 중요한 알고리즘으로 알고 있는데  
재밌고 유용하다니..!!
