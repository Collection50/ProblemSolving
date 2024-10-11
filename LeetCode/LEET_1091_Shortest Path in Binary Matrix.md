# Leetcode/1091/Shortest Path in Binary Matrix

#BFS

### solve 1

### solve 1 Code

```js
const shortestPathBinaryMatrix = (grid) => {
  if (grid.length <= 1 && grid[0][0] === 0) {
    return 1;
  }
  const q = [[0, 0, 0]];
  const visited = Array.from({length: grid.length}, () =>
    Array.from({length: grid.length}, () => false)
  );
  const direction = [
    [0, 1],
    [-1, 0],
    [0, -1],
    [1, 0],
    [1, 1],
    [-1, 1],
    [-1, -1],
    [1, -1]
  ];
  let result = 0;

  while (q.length) {
    const [r, c, count] = q.shift();

    if (
      r < 0 ||
      c < 0 ||
      r >= grid.length ||
      c >= grid.length ||
      grid[r][c] ||
      visited[r][c]
    ) {
      continue;
    }
    visited[r][c] = true;

    if (r === grid.length - 1 && c === grid.length - 1) {
      result = count;
      break;
    }

    for (const [row, col] of direction) {
      q.push([row + r, col + c, count + 1]);
    }
  }

  return result ? result + 1 : -1;
};
```
