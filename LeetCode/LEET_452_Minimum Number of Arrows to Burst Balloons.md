# Leetcode/452/Minimum Number of Arrows to Burst Balloons

#구현

### solve 1

### solve 1 Code

```js
var findMinArrowShots = function (points) {
  let count = 0;
  let prev = -Infinity;

  points.sort((a, b) => a[1] - b[1]);

  for (let i = 0; i < points.length; i++) {
    const [s, e] = points[i];

    if (s <= prev) {
      continue;
    }
    count += 1;
    prev = e;
  }
  return count;
};
```
