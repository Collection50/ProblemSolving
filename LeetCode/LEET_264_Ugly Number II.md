# Leetcode/264/Ugly Number II

#DP

### solve 1

### solve 1 Code

```js
const nthUglyNumber = (n) => {
  const nums = [1];
  let x = 0;
  let y = 0;
  let z = 0;

  for (let i = 1; i < n; i++) {
    nums.push(Math.min(nums[x] * 2, nums[y] * 3, nums[z] * 5));

    const last = nums.at(-1);

    if (last === nums[x] * 2) {
      x++;
    }
    if (last === nums[y] * 3) {
      y++;
    }
    if (last === nums[z] * 5) {
      z++;
    }
  }

  return nums.at(-1);
};
```
