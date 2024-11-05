# Leetcode/453/Minimum Moves to Equal Array Elements

#수학

### solve 1

### solve 1 Code

```js
var minMoves = function (nums) {
  let min = Math.min(...nums);
  let result = 0;

  for (const num of nums) {
    result += num - min;
  }
  return result;
};
```
