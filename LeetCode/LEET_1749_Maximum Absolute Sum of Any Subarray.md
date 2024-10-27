# Leetcode/1749/Maximum Absolute Sum of Any Subarray

#DP

### solve 1

### solve 1 Code

```js
const maxAbsoluteSum = (nums) => {
  let max = -Infinity;
  let min = Infinity;
  let result = 0;

  for (const num of nums) {
    max = Math.max(num, max + num);
    min = Math.min(num, min + num);

    result = Math.max(result, Math.abs(max), Math.abs(min));
  }

  return result;
};
```
