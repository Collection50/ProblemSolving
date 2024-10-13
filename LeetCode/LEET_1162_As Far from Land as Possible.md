# Leetcode/1162/As Far from Land as Possible

#BFS

### solve 1

### solve 1 Code

```js
const maxDistance = (grid) => {
  const q = [];
  const directions = [
    [0, 1],
    [-1, 0],
    [0, -1],
    [1, 0]
  ];
  const visited = Array.from({length: grid.length}, () =>
    Array.from({length: grid.length}, () => false)
  );

  for (let i = 0; i < grid.length; i++) {
    for (let j = 0; j < grid.length; j++) {
      if (grid[i][j]) {
        q.push([i, j, 0]);
      }
    }
  }
  let result = 0;

  while (q.length) {
    const [r, c, count] = q.shift();

    if (grid[r][c] === 0) {
      result = count;
    }

    for (const [row, col] of directions) {
      const nr = row + r;
      const nc = col + c;
      if (
        nr >= 0 &&
        nc >= 0 &&
        nr < grid.length &&
        nc < grid.length &&
        grid[nr][nc] !== 1 &&
        !visited[nr][nc]
      ) {
        visited[nr][nc] = true;
        q.push([nr, nc, count + 1]);
      }
    }
  }
  return result || -1;
};
```
