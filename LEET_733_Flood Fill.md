# LeetCode/841/Keys and Rooms

## Want

2차원 배열(grid), row, col, target이 주어진다  
시작 좌표인 grid[row][col]과 4방향 (동서남북)으로 인접한 좌표에 있는 값이 시작 좌표의 값과 같다면  
각 좌표의 모든 인접한 좌표의 값을 target으로 변경하라

## INPUT && OUTPUT

INPUT: ARRAY, NUMBER, NUMBER, NUMBER  
OUTPUT: ARRAY

## First Understanding & Seperating

1. 시작 좌표의 값을 저장 (`value`)
2. 현재 좌표가 `value`면 `target`으로 값을 바꾸고 상하좌우 각 값을 `dfs`
3. 현재 좌표의 값이 `target`이거나 좌표가 유효하지 않을 때 `return`
4. 상하좌우 값들을 확인하여 `value`와 같다면 2, 3번 반복
5. `return`

### solve 1

[이 문제](https://github.com/Collection50/ProblemSolving/blob/master/LEET_200_Number%20of%20Islands.md)와 접근 방법이 굉장히 비슷했다
각 4방향을 모두 탐색하여 배열의 값을 변경하는 문제였다  
`flipColor` 함수에서 `row`, `col`을 판단할 때 `if`문을 2개로 나눠 작성했다  
이는 코드가 너무 길어 논리식을 이해하기에 시간이 걸릴 것 같다고 생각하여 가독성이 좋게 바꾸었는데  
이러한 방식이 올바른지, 아니면

```js
if (row >= ROW_SIZE || col >= COL_SIZE || row < 0 || col < 0) return;
```

이렇게 작성하는 것이 나은지 궁금하다  
실무에선 어떤 코드가 사용되는지에 대한 궁금증이 항상 남아있다

### solve 1 Code

```js
// Runtime: 120 ms, faster than 34.07%
// Memory Usage: 43.8 MB, smaller than 94.36%
const floodFill = function (image, sr, sc, color) {
  const value = image[sr][sc];
  const ROW_SIZE = image.length;
  const COL_SIZE = image[0].length;

  if (value === color) return image;

  const filpColor = function (row, col) {
    if (row >= ROW_SIZE || col >= COL_SIZE) return;
    if (row < 0 || col < 0) return;
    if (image[row][col] !== value) return;

    image[row][col] = color;
    filpColor(row, col + 1);
    filpColor(row - 1, col);
    filpColor(row, col - 1);
    filpColor(row + 1, col);
  };
  filpColor(sr, sc);
  return image;
};
```

## 배운 점 or 주의할 점

어쩌면 `solve 1`에서의 고민은 너무 미시적인 수준이지 않을까?  
전체적인 구조와 로직이 더 중요한 부분이고 내가 비교적 디테일한 부분에서 고민을 하는 것이지 않을까 싶다  
무엇이 정답인가?  
정답이 존재하는가?
