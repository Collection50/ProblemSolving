# Leetcode/1992/Find All Groups of Farmland

#BFS

### solve 1

### solve 1 Code

```js
const findFarmland = (land) => {
  const visited = Array.from({length: land.length}, () =>
    Array.from({length: land[0].length}, () => false)
  );
  const result = [];
  const direction = [
    [0, 1],
    [-1, 0],
    [0, -1],
    [1, 0]
  ];

  const bfs = (r, c) => {
    const q = [[r, c]];
    let maxR = 0;
    let maxC = 0;

    while (q.length) {
      const [row, col] = q.shift();

      if (
        row >= land.length ||
        col >= land[0].length ||
        row < 0 ||
        col < 0 ||
        visited[row][col] ||
        land[row][col] !== 1
      ) {
        continue;
      }
      visited[row][col] = true;
      maxR = Math.max(row, maxR);
      maxC = Math.max(col, maxC);

      for (const [nr, nc] of direction) {
        q.push([row + nr, col + nc]);
      }
    }
    return [maxR, maxC];
  };

  for (let i = 0; i < land.length; i++) {
    for (let j = 0; j < land[0].length; j++) {
      if (!visited[i][j] && land[i][j] === 1) {
        result.push([i, j, ...bfs(i, j)]);
      }
    }
  }

  return result;
};
```
