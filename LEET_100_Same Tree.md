# LeetCode/100/Same Tree

#트리#재귀

## Want

이진 트리의 루트 p, q가 주어진다  
두 이진 트리가 동일한지 여부를 반환하라  
동일 조건

1. 구조적 일치
2. 노드 값의 일치

## INPUT && OUTPUT

```js
/**
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
```

## Solving Strategies

재귀를 사용하니, 큰 문제를 작은 문제로 쪼개자  
각 트리의 왼쪽 노드, 오른쪽 노드를 인수로 전달하여 불리언 값을 확인하자

1. 종료 조건 확인
2. 각 트리의 `left`를 떼네어 재귀 함수에 넣자
3. `left.val`이 동일하다면 1번 반복
4. `right`도 동일한 방식으로 검사

### solve 1

크게 아래와 같은 단락으로 나누어 해결했다

1. 구조적 일치 여부 검사
2. 값 일치 여부 검사

구조 일치 여부는 2가지로 나눠볼 수 있다

1. 둘 다 `null`을 만난 경우 리프 노드까지 모든 구조, 값이 같은 것이므로 `return true`
2. 한 쪽만 `null`을 만난 경우 구조가 다르므로 `return false`

마지막으로 값 일치 여부는 구조가 동일하다는 전제 하에 마지막으로 검사해주었다

### solve 1 Code

```js
// Runtime: 103 ms, faster than 22.84%
// Memory Usage: 42.5 MB, smaller than 36.93%
const isSameTree = function (p, q) {
  if (!p && !q) return true;
  if (!p || !q) return false;
  if (p.val !== q.val) return false;

  return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
};
ㄴ;
```

## 배운 점 or 주의할 점

우리는 왜 재귀 함수를 사용하는가?  
**큰 문제를 작은 문제로 쪼개**어 해결하기 위해서다  
각 문제 풀이 방법의 **사용과 목적 등을 이해하며 해결**하면 더 좋은 코드를 작성할 수 있을 것 같다
