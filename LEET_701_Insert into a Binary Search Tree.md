# LeetCode/701/Insert into a Binary Search Tree

#재귀#트리

## Want

이진 탐색 트리의 루트와 노드의 값이 주어진다  
이진 탐색 트리에 값을 삽입하여 올바른 이진 탐색 트리를 만들어라  
[이진 탐색 트리란?](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tree.md)

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @param {number} val
 * @return {TreeNode}
 */
```

## Solving Strategies

1. 이진 탐색 트리의 규칙대로 위치 찾기
2. `node`와 비교하여 값이 작거나 클 때 노드의 자식 자리가 비었다면 자리에 채우기
3. 자리가 비지 않고 노드의 자식 값과 비교하여 올바른 자리가 아니라면 재귀 함수 호출

### solve 1

재귀를 이용하여 풀었다  
위 전략을 토대로

1. 노드 값보다 `val`이 클 때와 작을 때로 분리
2. 해당 위치가 비어있는지 아닌지 확인
3. 비어 있다면 그 위치에 삽입하고, 비어있지 않다면 재귀 함수 호출하여 위치 찾기

### solve 1 Code

```js
// Runtime: 175 ms, faster than 46.16%
// Memory Usage: 52.9 MB, smaller than 49.49%
const insertIntoBST = function (root, val) {
  const treeNode = new TreeNode(val);
  if (!root) return treeNode;

  function insert(node) {
    if (node.val > val) {
      if (!node.left) {
        node.left = treeNode;
      } else {
        insert(node.left);
      }
    } else if (!node.right) {
      node.right = treeNode;
    } else {
      insert(node.right);
    }
  }

  insert(root);
  return root;
};
```

## Refactoring

### solve 2

위 코드는 로직은 명확하나 가독성도 좋은 편이 아니고, 들여쓰기가 과도하게 포함되어 있다  
따라서 가독성이 훨씬 좋은 코드로 리팩터링 해보았다  
이렇게 풀이하는 것을 꽤 해봤음에도 불구하고  
처음부터 이러한 풀이가 생각나지 않은 점이 아쉬웠다

### solve 2 Code

```js
// Runtime: 204 ms, faster than 17.37%
// Memory Usage: 53.6 MB, smaller than 7.53%
const insertIntoBST = function (root, val) {
  if (!root) return new TreeNode(val);

  if (root.val > val) {
    root.left = insertIntoBST(root.left, val);
  } else {
    root.right = insertIntoBST(root.right, val);
  }

  return root;
};
```

## 배운 점 or 주의할 점

좋은 코드란 무엇인가..?  
어떻게 하면 좋은 코드를 작성할 수 있을 것인가..?  
계속 고민된다
