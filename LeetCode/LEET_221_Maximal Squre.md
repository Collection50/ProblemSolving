# Leetcode/221/Maximal Square

#DP

### solve 1

### solve 1 Code

```js
const maximalSquare = (matrix) => {
  const dp = Array.from({length: matrix.length + 1}, () =>
    Array.from({length: matrix[0].length + 1}, () => 0)
  );
  let max = 0;

  for (let i = 1; i <= matrix.length; i++) {
    for (let j = 1; j <= matrix[0].length; j++) {
      if (matrix[i - 1][j - 1] === "1") {
        dp[i][j] = Math.min(dp[i - 1][j], dp[i - 1][j - 1], dp[i][j - 1]) + 1;
        max = Math.max(max, dp[i][j]);
      }
    }
  }

  return max ** 2;
};
```
