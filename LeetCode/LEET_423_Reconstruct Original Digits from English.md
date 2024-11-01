# Leetcode/423/Reconstruct Original Digits from English

#해시

### solve 1

### solve 1 Code

```js
var originalDigits = function (s) {
  const order = {
    z: "zero",
    w: "two",
    u: "four",
    x: "six",
    g: "eight",
    f: "five",
    s: "seven",
    o: "one",
    t: "three",
    i: "nine"
  };
  const number = {
    zero: "0",
    two: "2",
    four: "4",
    six: "6",
    eight: "8",
    five: "5",
    seven: "7",
    one: "1",
    three: "3",
    nine: "9"
  };
  const result = [];
  const map = {};

  for (const char of [...s]) {
    map[char] = (map[char] || 0) + 1;
  }

  for (const [key, word] of Object.entries(order)) {
    if (map[key]) {
      const count = Number(map[key]);

      for (const char of [...word]) {
        map[char] -= count;
      }
      result.push(number[word].repeat(count));
    }
  }

  return result.sort().join("");
};
```
