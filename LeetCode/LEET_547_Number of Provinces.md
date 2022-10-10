# LeetCode/547/Number of Provinces

#그래프

## Want

`n`개의 도시가 주어지며 몇 개의 도시는 연결되어 있다  
`a` 도시가 `b` 도시와 연결되어 있고 `b` 도시와 `c` 도시가 직접 연결되어 있다면  
`a` 도시는 `c` 도시와 간접적으로 연결 되어있음을 의미한다  
프로방스는 1개는 직간접적으로 연결된 도시들의 집합을 의미하고  
`isConnected[i][j] = 1`의 의미는 i도시와 j도시가 연결되어있다는 것을 의미한다  
프로방스의 개수를 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[][]} isConnected
 * @return {number}
 */
```

## Solving Strategies

어떠한 노드에 연결된 다른 노드를 찾는 것이므로(**합집합**과 유사), **유니온 파인드** 사용  
다른 노드들과 연결되어 있는 노드들을 1개로 간주하고  
연결되어 있지 않은 노드를 1개로 간주하여 프로방스의 개수 반환  
[유니온 파인드란?](https://github.com/Collection50/Algorithm-DataStructure/blob/master/Union%20Find.md)

### solve 1

유니온 파인드를 통한 해결

1. `isConnected[i][j]`의 각 연결이(값이) `1`이라면 그 노드는 다른 노드와 연결되어 있다
2. 따라서 `unionByRank`를 통해 두 노드를 한 노드로 연결
3. 이때 두 노드가 한 노드로 연결된다면, 프로방스의 총 개수가 1개씩 줄어드는 것이다
4. 따라서 반환값인 `this.result`를 1개씩 줄여나간다

### solve 1 Code

```js
// Runtime: 152 ms, faster than 12.22%
// Memory Usage: 45.1 MB, less than 36.57%
class UnionFind {
  constructor(size) {
    this.parent = Array.from({ length: size }, (_, i) => i);
    this.rank = Array.from({ length: size }, () => 1);
    this.result = size;
  }

  find(x) {
    return this.parent[x] === x ? x : this.find(this.parent[x]);
  }

  unionByRank(x, y) {
    const node1 = this.find(x);
    const node2 = this.find(y);

    if (node1 === node2) return;

    this.result--;

    if (this.rank[node1] > this.rank[node2]) {
      this.parent[node2] = node1;
    } else {
      this.parent[node1] = node2;
    }

    if (this.rank[node1] === this.rank[node2]) {
      ++this.rank[node1];
    }
  }
}

const findCircleNum = function (isConnected) {
  const { length } = isConnected;
  const union = new UnionFind(length);

  for (let i = 0; i < length; i++) {
    for (let j = 0; j < length; j++) {
      if (isConnected[i][j]) {
        union.unionByRank(i, j);
      }
    }
  }

  return union.result;
};
```

## 배운 점 or 주의할 점

유니온 파인드를 활용하여 문제를 해결하였다  
하지만 **실무**에서는 어떻게 코드를 사용할까?  
**실전 알고리즘** 문제를 풀 때도 `class`를 만들어 해결할까?  
다른 방법이 더 있을지 고민해봐야겠다
