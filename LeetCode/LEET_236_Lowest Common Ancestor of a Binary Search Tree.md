# LeetCode/236/Lowest Common Ancestor of a Binary Search Tree

#BinarySearchTree

## Want

이진 탐색 트리(BST)와 두 개의 노드가 주어진다  
`lowestCommonAncestor(LCA)`를 찾아 반환하라  
`LCA`: 두 노드가 공통적으로 갖고 있는 가장 낮은 레벨의 조상 노드를 의미한다

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
```

## Solving Strategies

3가지 경우의 수로 구분할 수 있음  
이진 탐색이라는 것을 잘 생각해야 함

1. 현재 값이 `p`, `q` 사이에 존재하는 경우: 현재 노드 반환
2. 현재 값이 `p`, `q` 보다 작은 경우: 오른쪽 노드를 재귀 함수 인수로 전달
3. 현재 값이 `p`, `q` 보다 큰 경우: 왼쪽 노드를 재귀 함수 인수로 전달

### solve 1

`LCA`는 가장 낮은 레벨에 존재하는 공통 조상을 의미하는데  
이진 탐색 트리의 특징을 잘 알고 있다면 어렵지 않게 해결할 수 있는 문제이다

`Solving Strategies`의
1번의 경우: 현재 값을 기준으로 큰 값, 작은 값이 구분되어 있으므로 조상은 현재 노드가 된다
2번의 경우: 현재 값이 `p`, `q`의 값보다 작으므로 현재 값보다 큰 범위를 탐색해야 하므로 오른쪽 노드를 재귀 함수에 전달한다
3번의 경우: 2번의 경우와 반대

### solve 1 Code

```js
// Runtime: 104 ms, faster than 76.97%
// Memory Usage: 52.2 MB, smaller than 63.97%
const lowestCommonAncestor = function (root, p, q) {
  const isCenter = (value) =>
    (value >= p.val && value <= q.val) || (value <= p.val && value >= q.val);

  const getLCA = (node) => {
    const value = node.val;

    if (isCenter(value)) return node;
    if (value >= p.val && value >= q.val) return getLCA(node.left);
    return getLCA(node.right);
  };

  return getLCA(root);
};
```

## Refactoring

### solve 2

로직을 간소화하여 가독성을 증가시켰다

1. 추가적인 함수를 생성하지 않도록 변경 (`getLCA`)
2. `if, else` 조건 최적화를 통해 `solve 1`의 `isCenter`를 사용하지 않고 `return`을 사용할 수 있도록 함

### solve 2 Code

```js
// Runtime: 156 ms, faster than 11.91%
// Memory Usage: 51.9 MB, smaller than 82.13%
const lowestCommonAncestor = function (root, p, q) {
  const value = root.val;

  if (value > p.val && value > q.val) {
    return lowestCommonAncestor(root.left, p, q);
  }
  if (value < p.val && value < q.val) {
    return lowestCommonAncestor(root.right, p, q);
  }

  return root;
};
```

## 배운 점 or 주의할 점

재귀 함수를 다룰 때 조건을 간소화하여 호출 및 반환할 수 있도록 더 주의해야겠다고 생각했다  
불리언 값을 반환할 때 간소화가 중요한 것처럼, 다른 코드를 작성할 때도 더 보기 좋은 코드를 작성하려고 노력하자
