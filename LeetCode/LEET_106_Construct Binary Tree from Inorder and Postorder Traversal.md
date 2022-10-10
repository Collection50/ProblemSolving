# LeetCode/106/Construct Binary Tree from Inorder and Postorder Traversal

#Tree

## Want

inorder로 순회한 결과가 주어지고  
postorder로 순회한 결과가 주어진다  
이진 트리를 생성하라  
[inorder? postorder?](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tree.md)

## INPUT && OUTPUT

```js
/**
 * @param {number[]} inorder
 * @param {number[]} postorder
 * @return {TreeNode}
 */
```

## First Understanding & Seperating

**재귀?** or **규칙성** 찾는 문제?
두 순회 결과로 유추할 수 있는 3가지

1. `postorder`의 마지막 값 == `root`
2. `inorder` 배열의 `root` 다음 나오는 값 == `root.right`
3. `inorder` 배열의 `root` 전에 나오는 값 == `root.left`의 자식 노드

그렇다면 `rootIndex`를 기준으로 `left`, `right`로 나눠야 하겠군

### solve 1

**_Fail..._**  
나름의 가설을 세워 **규칙성**을 발견하려고 노력하였고 3가지를 발견하여  
`root`를 기준으로 왼쪽, 오른쪽 가지로 구분할 수 있었다  
하지만 아쉽게도 해결하지 못한 문제였다

### solve 1 Code

```js
// Fail..
const buildTree = function (inorder, postorder) {
  if (!inorder.length) return;

  const root = new TreeNode(postorder.pop(), null, null);
  const rootIndex = inorder.indexOf(root.val);
  const left = inorder.slice(0, rootIndex);
  const right = inorder.slice(rootIndex + 1);

  return root;
};
```

## Refactoring

### solve 2

**분할, 정복**을 활용하여 문제를 해결하는 방법이 존재했다  
이 풀이의 핵심은

1. `inorder`, `postorder`의 특징을 활용하여 `root`의 위치를 찾고, 위치를 기준으로 **왼쪽 가지, 오른쪽 가지를 나눈 것**
2. 재귀 호출되는 `traverse`의 인수에 `node`의 위치(`index`)를 기준으로 배열을 분할하여 전달한 것
3. `postorder`의 특징을 감안하여 `node.right`를 먼저 순회한 것

즉 `postorder`에서 얻은 `val`에 따라 노드의 위치(`index`)가 구해진다  
그 위치를 부모 노드로 가정하여 왼쪽, 오른쪽 자노드를 구하며  
부모 노드가 아닌 경우 `null`을 반환하도록 하였다

### solve 2 Code

```js
// Runtime: 105 ms, faster than 78.67%
// Memory Usage: 84 MB, smaller than 37.32%
const buildTree = function (inorder, postorder) {
  const traverse = function (array) {
    if (!array.length) return null;

    const val = postorder.pop();
    const index = array.indexOf(val);
    const node = new TreeNode(val);

    node.right = traverse(array.slice(index + 1));
    node.left = traverse(array.slice(0, index));
    return node;
  };
  return traverse(inorder);
};
```

### solve 3

`Map`을 활용한 최적화  
`inorder`의 값, 인덱스를 저장한다  
이후 `traverse` 함수에 **인덱스**를 전달하며 `solve 2`와 동일하게 **분할 정복**을 활용한 방법 사용  
매번 인덱스를 구할 때 시간 복잡도 O(N)이 소요되는 `Array.indexOf()` 대신  
O(1)이 소요되는 **해시**를 사용하여 시간 복잡도를 크게 단축시킬 수 있었다  
더하여 `traverse` 함수에 인덱스만 전달하므로, `Array.slice()` 함수에 걸리는 시간도 단축할 수 있었다

### solve 2 Code

```js
// Runtime: 71 ms, faster than 99.68%
// Memory Usage: 45.1 MB, smaller than 61.87%
const buildTree = function (inorder, postorder) {
  const map = new Map();

  for (let i = 0; i < inorder.length; i++) {
    map.set(inorder[i], i);
  }

  const traverse = function (start, end) {
    if (start > end) return null;

    const val = postorder.pop();
    const index = map.get(val);
    const node = new TreeNode(val);

    node.right = traverse(index + 1, end);
    node.left = traverse(start, index - 1);
    return node;
  };

  return traverse(0, inorder.length - 1);
};
```

## 배운 점 or 주의할 점

`solve 1`을 보면, 문제를 해결할 수 있었던 `solve 2`, `solve 3`의 방향과 굉장히 비슷하다  
이는 **전체적인 로직을 머릿속에 그릴 수 있었고 방향도 맞았지만 완전히 해결할 수 없었던 것**을 의미한다  
어찌보면 조금 실력이 상승된 것이 아닌가 하는 생각도 든다

문제의 해결을 보면 `quickSort`와 비슷한 개념이 사용되는 것처럼 보인다  
나도 `left`, `right`로 나누면서 로직을 작성했었는데, 분할 정복을 생각하지 못했었다  
따라서 나중에 다른 문제에서 `left`, `right`처럼 양쪽을 나누는 경우가 발생했을 때  
**분할 정복을 적용**하여 문제를 해결할 수 있도록 해보아야겠다
