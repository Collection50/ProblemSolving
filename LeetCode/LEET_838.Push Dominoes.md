# Leetcode/838/Push Dominoes

#DP

### solve 1

### solve 1 Code

```js
const pushDominoes = (dominoes) => {
  const array = Array.from({length: dominoes.length}, () => 0);
  const forces = Array.from({length: dominoes.length}, () => 0);

  let force = 0;

  for (let i = 0; i < dominoes.length; i++) {
    const block = dominoes[i];

    if (block === "R") {
      force = dominoes.length;
    } else if (block === "L") {
      force = 0;
    } else {
      force = Math.max(force - 1, 0);
    }
    forces[i] = force;
  }

  for (let i = dominoes.length - 1; i >= 0; i--) {
    const block = dominoes[i];

    if (block === "L") {
      force = dominoes.length;
    } else if (block === "R") {
      force = 0;
    } else {
      force = Math.max(force - 1, 0);
    }
    forces[i] -= force;
  }

  return forces
    .map((value, index) => {
      if (value === 0) {
        return ".";
      }
      return value > 0 ? "R" : "L";
    })
    .join("");
};
```
