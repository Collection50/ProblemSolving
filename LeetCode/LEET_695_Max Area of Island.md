# LeetCode/695/Max Area of Island

#BFS#DFS

### solve 1

### solve 1 Code

```js
const traverse = (grid, row, col) => {
  const q = [[row, col]];
  const directions = [
    [0, 1],
    [-1, 0],
    [0, -1],
    [1, 0]
  ];
  let area = 1;
  grid[row][col] = 0;

  while (q.length) {
    const [r, c] = q.shift();

    directions.forEach(([_r, _c]) => {
      const nextR = r + _r;
      const nextC = c + _c;

      if (
        nextC >= 0 &&
        nextR >= 0 &&
        nextC < grid[0].length &&
        nextR < grid.length &&
        grid[nextR][nextC] === 1
      ) {
        grid[nextR][nextC] = 0;
        area += 1;
        q.push([nextR, nextC]);
      }
    });
  }
  return area;
};

const maxAreaOfIsland = function (grid) {
  let max = 0;

  grid.forEach((row, rowIndex) => {
    row.forEach((value, index) => {
      if (value) {
        const island = traverse(grid, rowIndex, index);
        max = Math.max(island, max);
      }
    });
  });
  return max;
};
```
