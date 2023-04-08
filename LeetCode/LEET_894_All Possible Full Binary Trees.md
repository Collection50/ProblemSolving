# LeetCode/894/All Possible Full Binary Trees

#트리#DP#재귀

## Want

`n`이 주어진다  
`n`개 노드로 만들 수 있는 모든 완전 이진 트리를 반환하라  
모든 완전 이진 트리의 루트 노드를 배열에 담아 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @return {TreeNode[]}
 */
```

## Solving Strategies

완전 이진트리를 만들기 위해선  
자식 노드의 개수가 `0 or 2`여야 한다  
그렇다면 총 노드 수 `n`이 홀수여야 한다  
즉 홀수의 경우만 생각하여 계산하면 된다  
따라서 `n - 2`, `n - 4` ... 형식으로 `n`이 홀수일 때를  
가정하여 계산하면 된다

또한 **모든** 경우의 수를 생각해야 하므로  
왼쪽으로 이어진 경우와 오른쪽으로 이어진 경우로 나뉜다  
이 경우 증가하는 `i`에 맞춰 `n`이 작아지는 형식으로 구현하였다

DP를 활용하여 해결하였기에  
이전에 사용되었던 노드를 재사용할 수 있었다  
3개로 구현된 `FBT`, 5개로 구현된 `FBT` 등을 재사용하였다

### solve 1

`Solving Strategies`의 코드화

### solve 1 Code

```js
function TreeNode(val) {
  this.val = val;
  this.left = null;
  this.right = null;
}

const memo = new Map();

function allPossibleFBT(n) {
  if (memo.has(n)) {
    return memo.get(n);
  }
  if (n === 1) {
    return [new TreeNode(0)];
  }
  if (n % 2 === 0) {
    return [];
  }

  const results = [];

  for (let i = 2; i < n; i += 2) {
    const leftSubtrees = allPossibleFBT(i - 1);
    const rightSubtrees = allPossibleFBT(n - i);

    for (const leftSubtree of leftSubtrees) {
      for (const rightSubtree of rightSubtrees) {
        const newTree = new TreeNode(0);
        newTree.left = leftSubtree;
        newTree.right = rightSubtree;
        results.push(newTree);
      }
    }
  }
  memo.set(n, results);
  return results;
}
```

## 배운 점 or 주의할 점

`DP`적 사고가 생각보다 꽤 까다롭다  
구현은 차치하고  
점화식을 도출하거나, `DP` 문제인지 구별하여  
최적 부분 구조를 찾아내는 것이 가장 어렵다

어쩌면 현재는 해결 방법을 떠올리는 **문제 해결 능력**이 부족한 것일지도  
그럼에도 불구하고
