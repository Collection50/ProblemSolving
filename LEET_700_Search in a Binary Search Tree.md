# LeetCode/700/Search in a Binary Search Tree

#재귀#트리

## Want

BST(이진 탐색 트리)와 `target`이 주어진다  
`target`을 값으로 갖고 있는 노드의 서브트리를 반환하라  
만약 값이 존재하지 않는다면 `null` 반환  
[이진 탐색 트리란?](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tree.md)

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @param {number} val
 * @return {TreeNode}
 */
```

## First Understanding & Seperating

재귀 활용하면 되겠는걸?

1. `node.val`과 `target`의 비교에 따라 왼쪽, 오른쪽 노드 탐색
2. 현재 노드가 `null`이라면 `null` 반환
3. `node.val === target`이라면 노드 반환

재귀 함수

1. 변화 인수: 노드
2. 종료 조건: `node.val === val` or `node == null`

### solve 1

**이진 탐색 트리가 무엇인지** 알고 있다면 굉장히 쉽게 풀 수 있는 문제다  
값을 찾거나, 찾지 못했을 때 반환된 값이 바로 결과로 직결되므로  
반환값을 저장할 필요 없이 재귀 함수의 반환 값을 바로 `return`하면 정답이 출력된다

### solve 1 Code

```js
const searchBST = function (root, target) {
  if (!root) return null;
  if (root.val > target) return searchBST(root.left, target);
  if (root.val < target) return searchBST(root.right, target);
  return root;
};
```

## 배운 점 or 주의할 점

이진 탐색 트리는 이름에서 알 수 있듯이 **탐색에 특화**된 트리이다  
삽입 시 **시간 복잡도 O(logN)**을 평균적으로 유지한다  
이는 부모 노드와 왼쪽 자노드, 오른쪽 자노드가 갖고 있는 **특징** 덕분이다  
이진 탐색 트리가 궁금하다면 위 링크를 클릭!  
재귀가 재밌기도 하면서 어렵다..
