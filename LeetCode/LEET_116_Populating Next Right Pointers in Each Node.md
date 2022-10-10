# LeetCode/116/Populating Next Right Pointers in Each Node

#Tree

## Want

완전 이진 트리가 주어진다  
완전 이진 트리의 특징

1. 모든 리프 노드들이 같은 레벨에 존재
2. 모든 부모 노드들은 2개의 자식 노드 소유

각 노드는 `left`, `right`, `next` 노드를 가지고 있다  
`next`는 같은 레벨에 존재하는 노드의 오른쪽 노드로 연결되며 오른쪽 노드가 없다면 `null`을 할당한다  
각 노드의 `next` 노드를 연결한 트리를 반환하라  
단, **상수 공간**을 사용하여 문제를 해결하여야 한다 (재귀는 허용)

## INPUT && OUTPUT

```js
/**
 * @param {Node} root
 * @return {Node}
 */
```

## First Understanding & Seperating

1. BFS 활용
2. 재귀 활용

두 가지 모두 풀어볼건데  
일단 BFS 방식을 활용하여 풀어보자

1. `queue` 생성
2. `while (queue.length)` 반복
3. `queue`에 인접 노드 넣기
4. `for`문 통해 next 노드 연결
5. `return`

### solve 1

BFS를 사용하면 같은 레벨의 `next` 노드를 연결할 때 훨씬 **직관적**이라고 볼 수 있다  
같은 레벨끼리의 노드만 모은 후, 연결하면 되기 때문이다  
따라서 같은 레벨의 노드만을 분류하기 위해 `level` 배열을 사용하였고  
`for`문을 활용해 `next` 노드를 **다음 인덱스에 존재하는 노드**로 연결해주었다  
재귀를 활용하는 것에 비해 굉장히 **빠른 시간** 안에 문제가 해결되는 것을 볼 수 있다  
공간 복잡도는 범위를 벗어났지만 말이다

### solve 1 Code

```js
// Runtime: 82 ms, faster than 94.67%
// Memory Usage: 49.7 MB, OUT OF RANGE
const connect = function (root) {
  if (!root) return root;

  const queue = [root];

  while (queue.length) {
    const size = queue.length;
    const level = [];

    for (let i = 0; i < size; i++) {
      const node = queue.shift();
      level.push(node);

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    for (let i = 0; i < level.length; i++) {
      level[i].next = level[i + 1];
    }
    level[level.length - 1].next = null;
  }
  return root;
};
```

### solve 2

하지만 문제의 조건에서 **상수 공간만을 사용**하여 문제를 해결하라는 권장이 존재했다  
따라서 **재귀**를 활용하는 방식으로도 문제를 해결했다  
재귀로 해결하기 위해선, 문제를 잘 읽어보아야 했다  
문제는 **완전 이진 트리**가 주어진다고 말하였는데,  
나는 이렇게 주어진 데에는 이유가 있을 것이라고 생각했고 그 **특성을 이용**해야겠다고 생각했다

생각, 고민, 시뮬레이션을 돌려보는 와중  
`node.next = traverse(node.left)` 처럼 재귀 함수가 노드를 반환하는 것이 아니라  
**순회만 수행**하며, `root`를 기준으로 `left`, `right`를 연결 후  
`right`의 `next`는 `next.left`로 연결된다는 것을 이해했다  
바로 이 부분에서 **완전 이진 트리의 속성이 사용**되며  
맨 오른쪽 노드, `node.right.next === null`이 되어야 할 노드에서는  
**옵셔널 체이닝 연산자** `?.`를 사용하여 해결했다

### solve 2 Code

```js
// Runtime: 122 ms, faster than 47.28%
// Memory Usage: 48.1 MB, smaller than 93.53%
const connect = function (root) {
  if (!root) return root;
  root.next = null;

  const traverse = function (node) {
    if (!node.left) return;

    node.left.next = node.right;
    node.right.next = node?.next?.left;
    if (!node.right.next) node.right.next = null;

    traverse(node.left);
    traverse(node.right);
  };
  traverse(root);
  return root;
};
```

## 배운 점 or 주의할 점

`solve 2`의 코드를 모두 작성하고 난 후  
계속 `traverse` 함수의 첫 조건문에서 오류가 발생했다  
`undefined` 참조 오류였는데 이해가 되지 않았다  
로직도 잘 작성하였고, 완전 이진 트리의 특성상 **한쪽 노드가 없으면 양쪽 노드가 모두 존재하지 않는** 건데  
왜 오류가 나지? 생각하며 지저분하게 예외처리를 해주었음에도 불구하고 작동하지 않았었다  
알고 보니 `right`를 `rigth`로 작성하여 `undefined` 참조 오류가 발생하였던 것이었다.... ~~(OTL)~~  
약 3-40분간 오류만 찾아 헤맸던 시간이 너무 아깝기도 하였고  
다시는 이러한 참조 오류를 발생시키지 않도록 노력해야겠다고 생각했다  
실제 코딩테스트에서는 보통 IDE를 사용할 수 없을텐데, 자동 완성이 없는 상황에서  
이러한 **사소한 실수를 줄여야겠다**고 생각했다
