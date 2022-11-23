# LeetCode/129/Sum Root to Leaf Numbers

#트리

## Want

각 노드가 `0 ~ 9`를 값으로 갖고 있는 트리가 존재한다  
노드를 순회하는 순서대로 값을 이어붙여 저장한다  
리프노드에 도착했다면, 이어붙인 값을 숫자로 변경한다  
모든 리프 노드를 방문했을 때 얻은 숫자의 총합을 구하여라  
[문제 링크](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @return {number}
 */
```

## Solving Strategies

`dfs`를 활용하여 전위 순회 수행한다  
노드, 노드의 값을 `dfs` 매개변수로 전달  
리프 노드라면 해당 값을 `values`에 저장  
`values`의 모든 값을 합하여 반환

### solve 1

문자 + 숫자가 더해졌을 때, 문자열로 변환되는 자바스크립트 성질 활용

1. 첫 `dfs`가 호출될 때, `root.val`을 문자열로 변환하여 전달하였다
2. 자노드가 존재하는 경우, (자노드, `value` + 자노드 값)을 매개변수로 `dfs`를 호출한다
3. 리프 노드인 경우 해당 값을 `values`에 추가
4. `reduce` 활용하여 총합을 구하여 반환하였다

### solve 1 Code

```js
const sumNumbers = function (root) {
  const values = [];

  const dfs = (node, value) => {
    if (!node.left && !node.right) {
      values.push(value);
      return;
    }

    if (node.left) {
      dfs(node.left, value + node.left.val);
    }
    if (node.right) {
      dfs(node.right, value + node.right.val);
    }
  };
  dfs(root, `${root.val}`);

  return values.reduce((acc, cur) => acc + Number(cur), 0);
};
```

## 배운 점 or 주의할 점

자바스크립트의 특징을 이용한다면 굉장히 쉽게 풀 수 있었다  
아마 각 자리수를 활용해야 했다면, 10의 지수가 증가하는 형식으로 해결해야 했을 듯하다
