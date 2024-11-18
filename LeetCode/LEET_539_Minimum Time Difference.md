# Leetcode/539/Minimum Time Difference

#정렬

### solve 1

### solve 1 Code

```js
const tt = (string) => {
  const [h, m] = string.split(":").map(Number);
  return h * 60 + m;
};

var findMinDifference = function (timePoints) {
  const nums = timePoints.map(tt).sort((a, b) => a - b);

  let result = Infinity;

  for (let i = 0; i < nums.length - 1; i++) {
    result = Math.min(result, nums[i + 1] - nums[i]);
  }

  return Math.min(result, 1440 - nums[nums.length - 1] + nums[0]);
};
```
