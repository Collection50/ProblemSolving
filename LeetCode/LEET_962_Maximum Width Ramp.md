# Leetcode/962/Maximum Width Ramp

#스택

### solve 1

### solve 1 Code

```js
const maxWidthRamp = (nums) => {
  const stack = [];
  let result = 0;

  for (let i = 0; i < nums.length; i++) {
    if (!stack.length || nums[stack.at(-1)] > nums[i]) {
      stack.push(i);
    }
  }

  for (let i = nums.length - 1; i > result; i--) {
    while (stack.length && nums[stack.at(-1)] <= nums[i]) {
      result = Math.max(result, i - stack.pop());
    }
  }

  return result;
};
```
