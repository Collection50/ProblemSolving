# Leetcode/567/Permutation in String

#Sliding Window

### solve 1

### solve 1 Code

```js
const isSame = (arr1, arr2) => {
  for (let i = 0; i < arr1.length; i++) {
    if (arr1[i] !== arr2[i]) {
      return false;
    }
  }
  return true;
};

const checkInclusion = (s1, s2) => {
  if (s1.length > s2.length) {
    return false;
  }
  const alphaS1 = Array.from({length: 26}, () => 0);
  const alphaS2 = Array.from({length: 26}, () => 0);

  for (let i = 0; i < s1.length; i++) {
    const charCode = s1[i].charCodeAt() - "a".charCodeAt();
    const charCode2 = s2[i].charCodeAt() - "a".charCodeAt();
    alphaS1[charCode] += 1;
    alphaS2[charCode2] += 1;
  }

  for (let i = 0; i <= s2.length - s1.length; i++) {
    if (isSame(alphaS1, alphaS2)) {
      return true;
    }

    if (s2?.[i + s1.length]) {
      alphaS2[s2[i + s1.length].charCodeAt() - "a".charCodeAt()] += 1;
      alphaS2[s2[i].charCodeAt() - "a".charCodeAt()] -= 1;
    }
  }
  return false;
};
```
