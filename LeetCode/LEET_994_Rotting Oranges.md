# Leetcode/994/Rotting Oranges

#BFS

### solve 1

### solve 1 Code

```js
const orangesRotting = function (grid) {
  const queue = [];
  const direction = [
    [0, 1],
    [1, 0],
    [0, -1],
    [-1, 0]
  ];
  let oranges = 0;
  let time = 0;

  for (let r = 0; r < grid.length; r++) {
    for (let c = 0; c < grid[r].length; c++) {
      if (grid[r][c] === 1) {
        oranges++;
      } else if (grid[r][c] === 2) {
        queue.push([r, c, 0]);
      }
    }
  }

  while (queue.length && oranges) {
    const [curR, curC, mins] = queue.shift();

    if (grid[curR][curC] === 1) {
      grid[curR][curC] = 2;
      oranges--;
      time = mins;
    }

    for (let [addR, addC] of direction) {
      const [newR, newC] = [curR + addR, curC + addC];
      if (!grid?.[newR]?.[newC]) {
        continue;
      }
      if (grid[newR][newC] === 1) {
        queue.push([newR, newC, mins + 1]);
      }
    }
  }

  return oranges ? -1 : time;
};
```
