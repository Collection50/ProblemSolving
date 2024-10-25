# Leetcode/1035/Uncrossed Lines

#DP

### solve 1

### solve 1 Code

```js
const maxUncrossedLines = (nums1, nums2) => {
  const dp = Array.from({length: nums1.length + 1}, () =>
    Array.from({length: nums2.length + 1}, () => 0)
  );

  for (let i = 1; i <= nums1.length; i++) {
    for (let j = 1; j <= nums2.length; j++) {
      if (nums1[i - 1] === nums2[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1] + 1;
      } else {
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
      }
    }
  }

  return dp[nums1.length][nums2.length];
};
```
