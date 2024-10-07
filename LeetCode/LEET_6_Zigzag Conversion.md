# Leetcode/6/Zigzag Conversion

#구현

### solve 1

### solve 1 Code

```js
const convert = (s, numRows) => {
  if (numRows <= 1) {
    return s;
  }
  const array = Array.from({length: numRows}, () => []);

  for (let i = 0; i < s.length; i++) {
    for (let j = i; j < i + numRows; j++) {
      array[j - i].push(s[j]);
    }
    i += numRows;

    for (let j = numRows - 2; j >= 1; j--) {
      array[j].push(s[i + numRows - 2 - j]);
    }
    i += numRows - 3;
  }

  return array.map((row) => row.join("")).join("");
};
```
