# LeetCode/112/Path Sum

#Tree

## Want

이진 트리의 루트와 target 정수가 주어진다  
만약 루트부터 리프 노드까지 값을 더한 것이 target과 동일한지 여부를 불리언 값으로 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @param {number} targetSum
 * @return {boolean}
 */
```

## First Understanding & Seperating

이건 dfs로 풀면 되면 되겠는걸?  
노드의 자식이 하나라도 존재하는 경우 이전까지의 합 `sum`을 재귀 함수의 인수로 전달
만약 자식이 없다면 이전 합과 현재 값을 더하여 `targetSum`이 되는지 확인

1. 자식이 하나라도 존재하는 경우, 자식 노드와 이전 합을 재귀 함수의 인수로 전달
2. 호출된 재귀 함수는 이전 합 + 현재 값을 더한 후 `targetSum`과 비교하여 값 반환
3. 만약 반환값이 하나라도 `true`라면 `true` 반환, 아니면 `false` 반환

### solve 1

생각보다 쉽게 풀렸던 문제였다  
결국 우리가 확인해야 할 중요 조건은 아래와 같다

1. 리프 노드인가?
2. 리프 노드까지의 값 합이 `targetSum`과 일치하는가?

리프 노드 확인에는 `isLeaf` 함수를 사용하였고  
리프 노드까지의 합이 `targetSum`과 일치 여부는 `isFind` 변수를 활용하였다  
따라서 트리의 모든 리프 노드까지의 합을 확인하여 `isFind`의 값을 반환하도록 전체 로직을 작성하였다

### solve 1 Code

```js
const hasPathSum = function (root, targetSum) {
  const isFind = false;
  const isLeaf = (node) => !node.left && !node.right;

  function traverse(node, sum) {
    if (!node || isFind) return;
    const tempSum = sum + node.val;

    if (isLeaf(node) && tempSum === targetSum) isFind = true;
    traverse(node.left, tempSum);
    traverse(node.right, tempSum);
  }
  traverse(root, 0);
  return isFind;
};
```

## Refactoring

### solve 2

조금이나마 더 효율적인 코드를 발견하였다  
위 `solve 1`의 경우 변수 `isFind`, 함수 `isLeaf`를 선언하여 해결하였지만  
아래 `solve 2`는 변수나 함수를 선언하지 않으면서 효율성을 유지할 수 있다  
로직은 간단하다 각 트리의 가지들의 `return` 값에 따라 `hasPathSum`의 반환값도 결정된다  
리프 노드까지의 합이 2번째 if문을 충족하지 못해 `null`을 참조한다면 `false`를 반환하며  
한 가지(트리의 가지)라도 리프 노드까지의 합과 `targetSum`이 같다면 `true`를 반환하도록 하였다  
내가 생각했을 때 `solve 2`의 코드가 효율적일 수 있는 이유는  
마지막 `return`문에서의 `OR(||)` 연산자이지 않을까 싶다

### solve 2 Code

```js
const hasPathSum = (root, targetSum) => {
  if (!root) return false;
  if (root.val === targetSum && !root.left && !root.right) return true;

  return (
    hasPathSum(root.left, targetSum - root.val) ||
    hasPathSum(root.right, targetSum - root.val)
  );
};
```

## 배운 점 or 주의할 점

함수 자체가 불리언 값을 반환하는 `has-`, `is-`류 함수의 경우  
바로 불리언 값이 반환되도록 하는 로직을 작성한다면 더 효율적인 코드를 작성할 수 있을 것 같다
