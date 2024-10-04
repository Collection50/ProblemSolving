# Leetcode/134/Gas Station

#투포인터

### solve 1

### solve 1 Code

```js
const canCompleteCircuit = (gas, cost) => {
  let count = 0;
  let sum = 0;

  const gas2 = [...gas, ...gas];
  const cost2 = [...cost, ...cost];

  for (let i = 0; i < gas2.length; i++) {
    if (sum + gas2[i] < cost2[i]) {
      sum = 0;
      count = 0;
      continue;
    }
    sum += gas2[i] - cost2[i];
    count += 1;

    if (count === gas.length + 1) {
      return i - gas.length;
    }
  }
  return -1;
};
```
