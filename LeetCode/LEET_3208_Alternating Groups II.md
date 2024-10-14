# Leetcode/3208/Alternating Groups II

#구현

### solve 1

### solve 1 Code

```js
const numberOfAlternatingGroups = (colors, k) => {
  const dp = Array.from({length: colors.length + k}, () => 1);

  dp[0] = 1;

  for (let i = 1; i < colors.length + k; i++) {
    if (colors[i % colors.length] === colors[(i - 1) % colors.length]) {
      dp[i % colors.length] = 1;
    } else {
      dp[i % colors.length] = dp[(i - 1) % colors.length] + 1;
    }
  }

  return dp.filter((value) => value >= k).length;
};
```

## Refactoring

### solve 2

### solve 2 Code

```js
const numberOfAlternatingGroups = (colors, k) => {
  let result = 0;
  let count = 1;

  for (let i = 1; i < colors.length + k - 1; i++) {
    if (colors[i % colors.length] === colors[(i - 1) % colors.length]) {
      count = 1;
    } else {
      count += 1;
    }
    if (count >= k) {
      result += 1;
    }
  }
  return result;
};
```
