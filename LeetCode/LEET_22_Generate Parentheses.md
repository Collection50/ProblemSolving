# Leetcode/22/Generate Parentheses

#DFS

### solve 1

### solve 1 Code

```js
const generateParenthesis = (n) => {
  const result = [];

  const generate = (current, open, close) => {
    if (current.length === n * 2) {
      result.push(current);
      return;
    }

    if (open < n) {
      generate(current + "(", open + 1, close);
    }

    if (close < open) {
      generate(current + ")", open, close + 1);
    }
  };

  generate("", 0, 0);
  return result;
};
```
