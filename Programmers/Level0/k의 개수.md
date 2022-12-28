# Programmers/k의 개수

#문자열

## Want

정수 `i`, `j`, `k`가 매개변수로 주어진다  
`i`부터 `j`까지 `k`가 몇 번 등장하는지 횟수를 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number} i
 * @param {number} j
 * @param {number} k
 * @return {number}
 */
```

## Solving Strategies

`i ~ j` 값을 갖고 있는 배열 생성  
배열을 순회하며 각 값이 갖고 있는 `k` 개수 세기

### solve 1

`Solving Strategies`의 코드화  
`reduce` 활용

### solve 1 Code

```js
function solution(i, j, k) {
  return Array.from({ length: j - i + 1 }, (_, index) => index + i).reduce(
    (count, number) => {
      const matched = number.toString().match(new RegExp(k, 'g'));
      return matched ? count + matched.length : count;
    },
    0
  );
}
```

## Refactoring

가독성을 향상하기 위해 함수 분리

### solve 2

동일하게 `reduce` 사용하였으나  
각 `k`의 개수를 세는 함수를 분리하여 가독성 향상

### solve 2 Code

```js
function countMatch(target, number) {
  const matched = number.toString().match(new RegExp(target, 'g'));

  if (matched) {
    return matched.length;
  }
  return 0;
}

function solution(i, j, k) {
  return Array.from({ length: j - i + 1 }, (_, index) => index + i).reduce(
    (count, number) => countMatch(k, number) + count,
    0
  );
}
```

### solve 3

`split`을 활용한 풀이

1. 문자열로 `join`
2. `k`로 `split`하여 개수 세기

`solve 3`의 풀이가 가장 마음에 든다

### solve 3 Code

```js
function solution(i, j, k) {
  return (
    Array.from({ length: j - i + 1 }, (_, index) => index + i)
      .join('')
      .split(k).length - 1
  );
}
```

## 배운 점 or 주의할 점

이런 상황은 오히려 날
