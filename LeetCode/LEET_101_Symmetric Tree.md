# LeetCode/101/Symmetric Tree

#트리#BFS

## Want

이진 트리의 루트가 주어진다  
루트를 기준으로 트리가 데칼코마니인지 확인하라

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
```

## First Understanding & Seperating

BFS를 활용하면 되지 않을까?  
같은 레벨에서의 값의 리스트(배열)가 **_회문_** 인지 확인하는거야!

1. `queue`를 활용하여 BFS 방식으로 레벨마다의 값 확인
2. `while`문 내의 `for`문 활용
3. 회문이 아니라면 `false` 반환
4. 회문이라면 `true` 반환

### solve 1

이 문제를 풀면서 내가 확인하고 싶은 것은 아래다

1. 나는 이 문제를 BFS를 활용하여 풀고 싶었다
2. 각 레벨 간의 값과, 구조가 데칼코마니인지 확인하는 것이 문제의 의도였기에  
   회문이라는 아이디어가 떠올라 적용해보고도 싶었다
3. 더하여 비슷한 문제를 최근에 풀었기에, 그 개념을 내가 제대로 이해했는지 확인까지 하고 싶었다

결과적으로 3가지 모두 만족시켰으며 풀어냈다!

### solve 1 Code

```js
const isSymmetric = function (root) {
  const queue = [root];

  while (queue.length) {
    const queueSize = queue.length;
    const values = [];

    for (let i = 0; i < queueSize; i++) {
      const current = queue.shift();
      if (!current) values.push(null);
      else {
        values.push(current.val);
        queue.push(current.left);
        queue.push(current.right);
      }
    }

    const valueSize = values.length;
    for (let i = 0; i < Math.floor(valueSize / 2); i++) {
      if (values[i] !== values[valueSize - 1 - i]) return false;
    }
  }
  return true;
};
```

## Refactoring

### solve 2

문제 맨 아래 권장 사항 중 재귀, 반복을 각각 활용해 풀어보라는 권장이 존재했다  
이걸 너무 늦게 보아 다른 사람들의 답변을 확인하던 중, BFS를 활용한 풀이는 찾지 못하고  
DFS를 활용한 풀이만 존재했었다  
따라서 재귀를 적용시켜보았고 로직은 간단하다  
**대칭을 가리키는 노드의 구조와 값이 동일한가?** 이다  
`left.left`, `right.right` 비교, `left.right`, `right.left`를 비교하게 되면  
대칭적인 위치에 놓인 노드들을 비교하여 불리언 값을 반환한다  
따라서 대칭적인 위치에 있는 노드들의 반환값이 같다면 `true`를 반환하고, 아니면 `false`를 반환하게 하였다  
dfs를 사용했기에 트리 순회를 위한 `traverse` 함수를 선언해 사용하였다

### solve 2 Code

```js
var isSymmetric = function (root) {
  const traverse = (left, right) => {
    if (!left && !right) return true;
    if (!left || !right) return false;
    if (left.val !== right.val) return false;

    return traverse(left.left, right.right) && traverse(left.right, right.left);
  };
  return traverse(root.left, root.right);
};
```

## 배운 점 or 주의할 점

재귀와 반복, 각 방법의 장단점은 확연히 존재한다  
어느 분야에서나 존재하는 **은탄환은 없듯**이  
어느 것이 더 좋고 나쁘다는 없으며 **어떤 상황에서 어떻게 사용하느냐가 더 중요**하다  
아마 **개발자의 중요 역량**은 이러한 **상황을 구분하고 분류하여 적절한 것을 사용**하는 것에 있지 않을까?  
그렇기 때문에 경력직에 대한 선호도가 높을 수밖에 없는 것이고 말이다  
많은 경험을 쌓아보자!
