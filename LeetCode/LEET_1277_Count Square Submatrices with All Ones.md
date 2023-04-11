# LeetCode/1277/Count Square Submatrices with All Ones

#DP

## Want

`m * n` 크기의 `0`, `1`로만 이뤄진 2차원 배열이 주어진다  
만들 수 있는 모든 정사각형의 개수를 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[][]} matrix
 * @return {number}
 */
```

## Solving Strategies

DP를 활용한 풀이

가장 큰 정사각형을 찾는 로직을 그대로 적용할 수 있다  
차이점은 정사각형을 찾을 때 사용하였던  
`memo` 배열의 값을 모두 더한 것이 모든 정사각형의 개수라는 점이다

완전히 다른 문제로 생각했었는데  
의도치 않게 동일한 로직의 문제를 해결하였다  
[동일한 로직의 다른 문제](https://github.com/Collection50/ProblemSolving/blob/master/Programmers/Level2/%EA%B0%80%EC%9E%A5%20%ED%81%B0%20%EC%A0%95%EC%82%AC%EA%B0%81%ED%98%95%20%EC%B0%BE%EA%B8%B0.md)

### solve 1

`Solving Strategies`의 코드화

### solve 1 Code

```js
const countSquares = function (matrix) {
  const memo = Array.from({ length: matrix.length }, () =>
    Array.from({ length: matrix[0].length }, () => 0)
  );

  matrix.forEach((row, rowIndex) => {
    row.forEach((value, colIndex) => {
      if (!rowIndex || !colIndex) {
        memo[rowIndex][colIndex] = value;
      } else if (value) {
        memo[rowIndex][colIndex] =
          value +
          Math.min(
            memo[rowIndex][colIndex - 1],
            memo[rowIndex - 1][colIndex],
            memo[rowIndex - 1][colIndex - 1]
          );
      }
    });
  });
  return memo.flat().reduce((a, b) => a + b, 0);
};
```

## 배운 점 or 주의할 점

창의성을 활용한 규칙성 찾기  
아직까지 `DP` 문제들이 크게 와닿지 않는다
