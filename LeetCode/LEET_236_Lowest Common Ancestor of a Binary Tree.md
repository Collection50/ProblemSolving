# LeetCode/236/Lowest Common Ancestor of a Binary Tree

#Tree

## Want

이진 트리가 주어진다  
가장 낮은 단계에 존재하는 공통 조상을 찾아라 (Lowest Common Ancestor, LCA)  
가장 낮은 공통 조상은 두 노드 p와 q 사이에 p와 q가 모두 자손으로 있는  
노듸 T의 가장 낮은 노드로 정의됩니다 (여기서 노드 자신은 자손이 될 수 있음)

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
```

## First Understanding & Seperating

일단 두 노드가 어디 있는지 찾아야 겠지?  
두 노드를 찾으면서 공통 조상을 찾는 것은 어려우려나?  
두 노드를 찾은 이후엔?

1. 노드 p가 노드 q의 조상일 때: 노드 p 반환
2. 노드 p, q의 조상이 각각 다를 때: 가장 낮은 공통 조상 반환

다양한 방법들을 적용시키려고 노력했다  
1번은 **BFS를 활용**하여 노드를 배열에 저장한 후  
부모 노드 == `(n - 1) / 2` (n == 인덱스)  
왼쪽 자노드 == `2n + 1`, 오른쪽 자노드 == `2n + 2` **특성을 활용**하여 같은 부모를 가리키는지 확인해볼까 했고  
2번은 **DFS를 활용**해 노드를 순회하여 `p` or `q`를 찾으면 `true`를 `return`하여  
재귀 함수의 반환값이 둘 다 `true`인 노드를 반환하려고 생각했었다  
하지만 둘 다 Fail.

### solve 1

**DFS를 활용**해 노드를 탐색한다  
`p`, `q` 노드를 찾으면 그 노드를 반환하고, 리프 노드를 발견했을 경우 `null`을 반환한다  
경우의 수는 두 가지로 나뉜다

1. `left`, `right`가 모두 존재할 경우(`p`, `q` 모두 찾은 경우)
2. `left`, `right` 하나만 찾은 경우(`p`, `q` 하나만 찾은 경우)

1번의 경우 바로 `root`를 반환하면 된다  
왜냐하면 `p`, `q`는 **왼쪽 가지, 오른쪽 가지에서 각각 존재**하는 것이기 때문이다  
반면 2번의 경우, 쉽게 설명하자면 `p`, `q` **둘 중 하나가 가장 낮은 공통 조**상을 의미하며  
나머지 **하나는 그 서브 트리 내에 속해** 있다는 것을 의미한다  
따라서 존재하는 노드를 반환하면 된다

### solve 1 Code

```js
// Runtime: 134 ms, faster than 32.73%
// Memory Usage: 52 MB, smaller than 33.73%
const lowestCommonAncestor = (root, p, q) => {
  if (!root || root === p || root === q) return root;
  const left = lowestCommonAncestor(root.left, p, q);
  const right = lowestCommonAncestor(root.right, p, q);
  if (!left) return right;
  if (!right) return left;
  return root;
};
```

## 배운 점 or 주의할 점

생각의 **확장이 트일 듯 말 듯** 하는 것 같다  
**효율적인 방법**을 따라 머리속이 움직이는 것 같은데 아직 정확한 방향을 잡지 못하고 있는 듯한 느낌이다  
조금 더 **Computing Thinking**을 해보자
