# LeetCode/450/Delete Node in a BST

#Tree#BST

## Want

이진 탐색 트리(BST)의 루트 노드와 타겟 값(`key`)이 주어진다  
BST의 삭제를 구현하라  
[자세히 확인하고 싶어요!!](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tree.md)

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @param {number} key
 * @return {TreeNode}
 */
```

## Solving Strategies

**재귀를 활용**하여 BST의 값을 찾고 삭제한다

삭제 과정은 3가지로 나뉠 수 있다

1. 삭제 노드의 **자식이 없는 경우**
2. 삭제 노드의 **자식이 한 개**만 있는 경우
3. 삭제 노드의 **자식이 둘 다** 존재하는 경우

1번의 경우 `null`을 반환하여 해당 노드만 삭제한다  
2번의 경우 **삭제 노드의 부모 노드**와 **삭제 노드의 자식 노드**를 연결한다  
3번의 경우 삭제 노드의 **오른쪽 노드 중 가장 작은 값**을 삭제 노드에 할당 후 가장 작은 값을 갖고 있는 노드를 삭제한다

만약 `target`이 BST에 존재하지 않는 경우 트리를 그대로 반환한다

### solve 1

경우의 수를 잘 분리하여 풀어야 하는 문제이다(로직도 쉽지는 않음)

`deleteNode` 함수의 반환값이 노드이기에,  
각 노드의 `left` or `right`에 반환값을 할당하여 가독성을 증가시켰다  
또한 효율적인 로직 작성을 위해 **한쪽이 존재하지 않을 때 반대쪽을 반환**하는 형식으로 작성하였다  
한쪽이 존재할 때 그 한 쪽을 반환한다면, **양쪽 다 존재할 때**를 확인하는 것이 비효율적이기 때문이다

삭제 노드가 양쪽 자식을 모두 소유하고 있을 때, 오른쪽 서브 트리 중 가장 작은 노드를 확인하기 위해
`getMinNode`라는 함수를 정의해 활용하였다

### solve 1 Code

```js
// Runtime: 117 ms, faster than 79.81%
// Memory Usage: 50.6 MB, smaller than 85.56%
const getMinNode = (node) => {
  let current = node;

  while (current.left) {
    current = current.left;
  }
  return current;
};

const deleteNode = function (root, key) {
  if (!root) return null;

  if (root.val === key) {
    if (!root.left && !root.right) return null;

    if (!root.left) return root.right;
    if (!root.right) return root.left;

    const minNode = getMinNode(root.right);
    root.val = minNode.val;
    root.right = deleteNode(root.right, minNode.val);
  }

  if (root.val > key) {
    root.left = deleteNode(root.left, key);
  } else {
    root.right = deleteNode(root.right, key);
  }

  return root;
};
```

## 배운 점 or 주의할 점

자료구조를 학습하고 배울 때 눈에 많이 익었다고 생각했는데,  
막상 활용하고 사용하려다보니 잘 생각나지 않고 한 눈에 들어오지 않는 부분이 아쉬웠다  
자료구조를 조금 더 들여다 보아야겠다는 생각이 들었다
