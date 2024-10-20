# Leetcode/457/Circular Array Loop

#투포인터

### solve 1

### solve 1 Code

```js
const circularArrayLoop = function (nums) {
  const n = nums.length;

  const move = (i) => (((i + nums[i]) % n) + n) % n;

  for (let i = 0; i < n; i++) {
    if (nums[i] === 0) {
      continue;
    }

    let slow = i;
    let fast = move(i);

    while (nums[i] * nums[fast] > 0 && nums[i] * nums[move(fast)] > 0) {
      if (slow === fast) {
        if (slow === move(slow)) {
          break;
        }
        return true;
      }

      slow = move(slow);
      fast = move(move(fast));
    }

    slow = i;
    const sign = nums[i];

    while (nums[slow] * sign > 0) {
      const next = move(slow);
      nums[slow] = 0;
      slow = next;
    }
  }

  return false;
};
```
