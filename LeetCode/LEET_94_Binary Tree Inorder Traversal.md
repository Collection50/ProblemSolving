# LeetCode/94/Binary Tree Inorder Traversal

#Stack#Tree#DFS

## Want

Binary Tree가 주어진다  
**중위 순회**한 결과를 반환하라  
[_트리의 순회 방법은?_](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tree.md)

## INPUT && OUTPUT

INPUT: TREE NODE  
OUTPUT: ARRAY

## Recursion Understanding & Seperating

1. `traverse` 함수를 통해 재귀 함수 활용
2. 중위 순회는 왼쪽 자노드, 부모 노드, 오른쪽 자노드 순으로 순회하는 방식이므로  
   왼쪽 자노드가 존재하지 않을 때까지 왼쪽 자노드를 탐색
3. 순서대로 왼쪽 자노드 `result`에 추가
4. 부모 노드 `result`에 추가
5. 오른쪽 자노드 탐색 후 `result`에 추가
6. 결과 `return`

### Recursion Code

```js
// Runtime: 102 ms, faster than 22.59%
// Memory Usage: 41.8 MB, smaller than 87.22%
const inorderTraversal = function (root) {
  const result = [];
  if (!root) return result;

  const traverse = function (node) {
    if (!node) return;

    traverse(node.left);
    result.push(node.val);
    traverse(node.right);
  };
  traverse(root);
  return result;
};
```

## Iteration Understanding & Seperating

문제 설명 중 재귀를 활용하는 것은 **_trivial_** 하다고 했다 (trivial: 하찮은..)  
**자존심이 허락하지 않아** 반복문을 활용한 풀이도 해결했다  
`stack` 활용

1. `current`를 `stack`에서 `pop`하여 구한다
2. `visited`에 존재한다면 방문한 노드이므로 `result`에 `push`
3. 존재하지 않는다면 방문 처리 수행한다
4. 이후 `current`를 기준으로 오른쪽 노드, `current`, 왼쪽 노드 순서대로 `stack`에 `push`
5. `stack`에 값이 존재하는 한 반복
6. 결과 `return`

**_오른쪽, 현재 노드, 왼쪽 노드 순으로 `stack`에 넣는 이유_**

1. 원본 배열의 변경 X
2. `stack`의 특징인 후입선출 극대화
3. 따라서 `stack`을 사용하는 DFS 방법에 대한 가독성 증가 및 효율성 증대

### Iteration Code

```js
// Runtime: 110 ms, faster than 13.70%
// Memory Usage: 42.4 MB, smaller than 21.00%
const inorderTraversal = function (root) {
  if (!root) return [];

  const result = [];
  const stack = [root];
  const visited = new Set();

  while (stack.length) {
    const current = stack.pop();

    if (visited.has(current)) result.push(current.val);
    else {
      visited.add(current);
      if (current.right) stack.push(current.right);
      stack.push(current);
      if (current.left) stack.push(current.left);
    }
  }
  return result;
};
```

## 배운 점 or 주의할 점

큐, 스택과 관련된 문제를 풀면서 왜 **_DFS는 스택_** 이 사용되는지, 왜 **_BFS는 큐_** 가 사용되는지 몸으로 체감하는 중이다  
반복문을 사용한 풀이 방법과 재귀를 이용한 풀이 방법 모두 잘 숙지하고 있어야겠다
