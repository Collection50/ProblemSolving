# Leetcode/5/Longest Palindromic Substring

#구현#DP

## Want

문자열 `s`가 주어진다  
가장 긴 회문으로 이뤄진 서브 문자열을 반환하라  
[문제 링크](https://leetcode.com/problems/longest-palindromic-substring/description/?source=submission-ac)

## INPUT && OUTPUT

```js
/**
 * @param {string} s
 * @return {string}
 */
```

## Solving Strategies

완전 탐색  
시간 복잡도 `O(N^3)`

### solve 1

모든

### solve 1 Code

```js
const isPalindrome = (string) => {
  for (let i = 0; i < string.length; i++) {
    if (string[i] !== string[string.length - 1 - i]) {
      return false;
    }
  }
  return true;
};

const longestPalindrome = function (s) {
  let length = s.length;
  let result = '';

  while (length) {
    for (let i = 0; i + length <= s.length; i++) {
      const subs = s.slice(i, length + i);
      if (isPalindrome(subs) && result.length < subs.length) {
        return subs;
      }
    }
    length--;
  }
  return result;
};
```

## Refactoring

DP를 사용한 시간 단축  
시간 복잡도 `O(N^2)`

### solve 2

### solve 2 Code

```js
const longestPalindrome = function (s) {
  const { length } = s;
  const dp = Array.from({ length }, () => Array(length).fill(false));
  let start = 0;
  let end = 0;

  for (let i = 0; i < length; i++) {
    dp[i][i] = true;
    start = i;
    end = i;
  }

  for (let i = 0; i < length - 1; i++) {
    if (s[i] === s[i + 1]) {
      dp[i][i + 1] = true;
      start = i;
      end = i + 1;
    }
  }

  for (let k = 2; k < length; k++) {
    for (let i = 0; i < length - k; i++) {
      const j = i + k;
      if (s[i] === s[j] && dp[i + 1][j - 1]) {
        dp[i][j] = true;
        if (end - start + 1 < j - i + 1) {
          start = i;
          end = j;
        }
      }
    }
  }
  return s.slice(start, end + 1);
};
```

## 배운 점 or 주의할 점

재활운동 꾸준히
