# Leetcode/377/Combination Sum IV

#DP

### solve 1

### solve 1 Code

```js
const combinationSum4 = (nums, target) => {
  const counts = Array.from({length: target + 1}, () => 0);
  counts[0] = 1;

  for (let i = 1; i <= target; i++) {
    for (const num of nums) {
      if (i >= num) {
        counts[i] += counts[i - num];
      }
    }
  }

  return counts[target];
};
```
