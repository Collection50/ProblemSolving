# Leetcode/532/K-diff Pairs in an Array

#구현

### solve 1

### solve 1 Code

```js
var findPairs = function (nums, k) {
  const map = {};
  let count = 0;

  for (const num of nums) {
    map[num] = (map[num] || 0) + 1;
  }

  if (k === 0) {
    return Object.values(map).filter((value) => value >= 2).length;
  }

  for (const key of Object.keys(map)) {
    const nextKey = String(Number(key) + k);

    if (map[nextKey]) {
      count += 1;
    }
  }
  return count;
};
```
