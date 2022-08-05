# LeetCode/102/Binary Tree Level Order Traversal

#트리

## Want

이진 트리의 루트가 주어진다  
레벨을 우선으로 순회한 노드의 값을 배열 형태로 반환하라

순회 순서

1. 왼쪽 => 오른쪽
2. 레벨 순으로

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
```

## First Understanding & Seperating

BFS를 응용하면 되면 쉽게 풀릴 것 같은데?  
[트리에서 BFS를 활용하려면?](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tree.md)

1. 큐를 활용한 BFS 구현하자
2. 결과 반환을 위한 `result` 배열을 활용하자
3. `level`을 활용해 각 레벨의 값을 담아 `result`에 추가하고
4. 마지막에 반환하면 되겠다

**_실패_**

### solve 1

BFS류 문제들을 많이 풀어보지는 않았지만 문제 이해 및 분류를 잘 했다고 생각했었는데  
생각보다 이곳 저곳에서 많은 삽질을 시도했던 문제가 되어버렸다  
`level`에 모든 레벨에 맞춰 값이 담기는 것이 아니라, 한 노드의 자식들만 `level`로 분류되어 `result`에 추가되는 것을 확인할 수 있다

### solve 1 Code

```js
// Fail
const levelOrder = function (root) {
  if (!root) return [];

  const queue = [root];
  const result = [[root.val]];

  while (queue.length) {
    const current = queue.shift();
    if (!current.left && !current.right) continue;

    const level = [];

    if (current.left) {
      queue.push(current.left);
      level.push(current.left.val);
    }
    if (current.right) {
      queue.push(current.right);
      level.push(current.right.val);
    }
    result.push(level);
  }
};
```

## Refactoring

### solve 2

BFS를 사용하는 로직은 변화는 없지만  
`for`문을 사용하는 것이 문제의 핵심 로직이었다  
**현재 큐에 존재하는 노드 수 === 같은 레벨의 노드들**을 의미하기 때문에(BFS의 특징)  
현재 큐에 존재하는 노드들의 값만을 `level` 배열에 저장하면 되었다  
현재 큐에 존재하는 노드 개수인 `size`만큼 반복하면  
딱 같은 레벨의 노드들의 값만을 추출하여 `level`에 저장할 수 있었고  
그 노드들의 자식들을 다시 큐에 넣을 수 있으므로 각 반복마다 레벨이 같은 노드들을 확실히 분류할 수 있었다

### solve 2 Code

```js
// Runtime: 89 ms, faster than 64.72%
// Memory Usage: 44.8 MB, smaller than 7.72%
const levelOrder = function (root) {
  if (!root) return [];

  const queue = [root];
  const result = [];

  while (queue.length) {
    const size = queue.length;
    const level = [];

    for (let i = 0; i < size; i++) {
      const node = queue.shift();

      level.push(node.val);
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    result.push(level);
  }
  return result;
};
```

## 배운 점 or 주의할 점

어렵지 않다고 생각했던 문제에서 꽤 많은 시간을 고민하니 다양한 생각이 날 뒤덮었다  
머리속에서 **'효율적인 알고리즘이라면 무조건 시간 복잡도 O(N^2)보다 성능이 좋아야 해'**라고 맹목적으로 말하고 있었다  
그래서인지 이중 반복문을 사용하는 것이 정답이 아닐 것이라고 **_당연히_** 생각하게 되었는데  
이는 모르는 것이 많은 상태이며 더 나아가 **자만**하는 상태라는 것을 깨달았다

더하여 문제가 잘 풀리지 않고 생각했던 곳에서 막히다 보니 테스트 케이스를 성급하게 **일반화**하여  
모든 경우에 사용되는 **범용적 코드**가 아니라, **하드 코딩이 남발**하는 코드가 되어버린 경향도 존재했다  
이러한 경우와 생각, 상황들을 **발판 삼아 높이 뛰자**
