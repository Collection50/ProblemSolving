# LeetCode/104/Maximum Depth of Binary Tree

#이진트리

## Want

이진 트리의 루트가 주어진다  
최대 depth를 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @return {number}
 */
```

## Solving Strategies

1. `root === null` 이면 `0`반환
2. 왼쪽, 오른쪽 나눠서 재귀 호출
3. 호출된 노드가 `null`일 경우(부모 노드가 leaf 노드일 경우) `Math.max(num1, num2)` 반환 (왼쪽 자식과 오른쪽 자식의 depth 중 큰 depth 반환)

### solve 1

아쉽게도(?) (로직을 틀려 아쉽다는 표현이 맞을지 모르겠지만) 틀린 문제다  
로직은 잘 작성했다고 생각했는데 로직을 **반대로 작성**하여 Fail을 받았다

### solve 1 Code

```js
const maxDepth = function (root) {
  return root ? 0 : Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
};
```

## Refactoring

### solve 2

로직을 다시 수정하여 작성하였다  
`root`가 존재할 때 재귀 함수가 호출되어야 하고,  
`null`일때 0을 반환하여 큰 값이 되지 못하도록 한다

### solve 2 Code

```js
// Runtime: 124 ms, faster than 18.98%
// Memory Usage: 45.2 MB, smaller than 60.86%
const maxDepth = function (root) {
  return root ? Math.max(maxDepth(root.left), maxDepth(root.right)) + 1 : 0;
};
```

## 배운 점 or 주의할 점

조금 더 고민하고, 신중하게 생각하자
