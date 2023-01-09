# LeetCode/807/Max Increase to Keep City Skyline

#배열

## Want

2차원 정수 배열 `grid`가 주어진다  
정수 배열은 동서남북 4방향에서 바라보는 `skyline`이 존재한다  
각 방향이 바라보는 `skyline`을 유지하며 건물의 높이가 최대가 되도록 할 때  
원본 `grid`의 건물 높이와의 차이를 구하여라  
[문제 링크](https://leetcode.com/problems/max-increase-to-keep-city-skyline/)

## INPUT && OUTPUT

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
```

## Solving Strategies

모든 방향의 `skyline`을 유지하며 각 건물의 높이가 최대가 되도록 해야 한다  
즉 각 `row`, `col`의 값의 최댓값보다는 작으면서 최대 높이를 유지해야 한다

1. `grid`순회
2. `grid[row][col]` 좌표의 각 열, 행의 최대값을 구한다
3. 2번의 각 행, 열의 최대값 중 작은 것을 선택한다
4. 3번의 값이 `grid[row][col]`의 값이된다

### solve 1

1. 원본 배열과의 차이 확인하는 `newGrid` 생성
2. `grid`를 `map`으로 순회
3. 각 좌표에 대해 행의 최대값, 열의 최대값을 계산, 이 중 작은 값을 반환하는 `maxHeight` 함수 활용
4. `grid`, `newGrid` 2차원 배열의 총 합 확인하는 `getSum` 활용하여 값 반환

**시간 복잡도 O(N^4)**

### solve 1 Code

```js
// Runtime: 86 ms, faster than 68.66%
// Memory Usage: 48 MB, smaller than 17.16%
const getSum = (numArrays) =>
  numArrays.reduce(
    (rowSum, row) => row.reduce((sum, num) => num + sum, 0) + rowSum,
    0
  );

const maxHeight = (grid, rowIndex, colIndex) => {
  const rowMax = Math.max(...grid[rowIndex]);
  const colMax = Math.max(...grid.map((row) => row[colIndex]));
  return Math.min(rowMax, colMax);
};

const maxIncreaseKeepingSkyline = (grid) => {
  const newGrid = grid.map((row, rowIndex) =>
    row.map((_, colIndex) => maxHeight(grid, rowIndex, colIndex))
  );
  return getSum(newGrid) - getSum(grid);
};
```

## Refactoring

시간 복잡도, 공간 복잡도를 줄이는 방향으로 리팩토링

### solve 2

1. `newGrid`를 생성하지 않아 공간복잡도 단축
2. 2중 반복문을 사용하여 **시간 복잡도 O(N^2)**로 단축
3. 각 열, 행의 최대값을 찾고 2개 최대값 중에서 작은 값을 찾는다
4. 차이를 알아야 하므로 해당 `grid`값에서 뺀 값을 반환한다

### solve 2 Code

```js
// Runtime: 67 ms, faster than 94.78%
// Memory Usage: 43.7 MB, smaller than 52.24%
const maxIncreaseKeepingSkyline = function (grid) {
  const rows = Array.from({ length: grid.length }, () => 0);
  const cols = Array.from({ length: grid.length }, () => 0);

  grid.forEach((row, rowIndex) => {
    row.forEach((_, colIndex) => {
      rows[rowIndex] = Math.max(rows[rowIndex], grid[rowIndex][colIndex]);
      cols[colIndex] = Math.max(cols[colIndex], grid[rowIndex][colIndex]);
    });
  });

  return grid.reduce(
    (sum, row, rowIndex) =>
      sum +
      row.reduce(
        (rowSum, _, colIndex) =>
          rowSum +
          Math.min(rows[rowIndex], cols[colIndex]) -
          grid[rowIndex][colIndex],
        0
      ),
    0
  );
};
```

### solve 3

- 위의 `grid[rowIndex][colIndex]`는 `value`와 동일하다
- 가독성을 위해 `value` 사용

### solve 3 Code

```js
// Runtime: 67 ms, faster than 94.78%
// Memory Usage: 43.7 MB, smaller than 52.24%
const maxIncreaseKeepingSkyline = function (grid) {
  const rows = Array.from({ length: grid.length }, () => 0);
  const cols = Array.from({ length: grid.length }, () => 0);

  grid.forEach((row, rowIndex) => {
    row.forEach((value, colIndex) => {
      rows[rowIndex] = Math.max(rows[rowIndex], value);
      cols[colIndex] = Math.max(cols[colIndex], value);
    });
  });

  return grid.reduce(
    (sum, row, rowIndex) =>
      sum +
      row.reduce(
        (rowSum, value, colIndex) =>
          rowSum + Math.min(rows[rowIndex], cols[colIndex]) - value,
        0
      ),
    0
  );
};
```

## 배운 점 or 주의할 점

경험을 토대로 분류하자면, 이건 **규칙성 문제**로 접근하는 것이 좋다  
문제, 정답을 토대로 답을 유추 해볼 때  
결국 각 건물의 **최대 높이 === 각 열, 행의 최대값 중 작은 값**으로 보는 것이 현명하다  
각 방향에서 바라본 건물 높이를 저장하거나 최소 최대값의 범위를 찾으려고 했다면  
굉장히 어려운 문제로 여겨졌을 것 같다

역시 문제 풀기 전 생각하고 고민하자
