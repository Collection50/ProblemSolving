# Leetcode/376/Wiggle Subsequence

#DP

### solve 1

### solve 1 Code

```js
const wiggleMaxLength = (nums) => {
  let peak = 1;
  let low = 1;

  for (let i = 1; i < nums.length; i++) {
    if (nums[i - 1] < nums[i]) {
      peak = low + 1;
    }
    if (nums[i - 1] > nums[i]) {
      low = peak + 1;
    }
  }
  return Math.max(peak, low);
};
```
