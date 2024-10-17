# Leetcode/120/Triangle

#DP

### solve 1

### solve 1 Code

```js
const minimumTotal = (triangle) => {
  for (let i = 1; i < triangle.length; i++) {
    const row = triangle[i];
    const prev = triangle[i - 1];

    for (let j = 0; j < row.length; j++) {
      const prevA = prev[j] === undefined ? Infinity : prev[j];
      const prevB = prev[j - 1] === undefined ? Infinity : prev[j - 1];

      triangle[i][j] += Math.min(prevA, prevB);
    }
  }

  return Math.min(...triangle.at(-1));
};
```
