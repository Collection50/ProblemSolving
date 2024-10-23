# Leetcode/1593/Split a String Into the Max Number of Unique Substrings

#DFS

### solve 1

### solve 1 Code

```js
const maxUniqueSplit = (s) => {
  let max = 0;

  const dfs = (index, set) => {
    if (index >= s.length) {
      max = Math.max(max, set.size);
      return;
    }

    for (let i = index + 1; i <= s.length; i++) {
      const substring = s.slice(index, i);
      if (!set.has(substring)) {
        set.add(substring);
        dfs(i, set);
        set.delete(substring);
      }
    }
  };
  dfs(0, new Set());

  return max;
};
```
