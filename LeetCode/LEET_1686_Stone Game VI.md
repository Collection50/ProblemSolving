# Leetcode/1686/Stone Game VI

### solve 1

### solve 1 Code

```js
const stoneGameVI = (aliceValues, bobValues) => {
  const values = [];
  let alice = 0;
  let bob = 0;

  for (let i = 0; i < aliceValues.length; i++) {
    values.push({value: aliceValues[i] + bobValues[i], index: i});
  }

  values.sort((a, b) => b.value - a.value);

  for (let i = 0; i < values.length; i++) {
    const index = values[i].index;
    if (i % 2 === 0) {
      alice += aliceValues[index];
    } else {
      bob += bobValues[index];
    }
  }

  if (alice === bob) {
    return 0;
  }
  return alice > bob ? 1 : -1;
};
```
