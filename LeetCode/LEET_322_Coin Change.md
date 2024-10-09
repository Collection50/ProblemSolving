# Leetcode/322/Coin Change

#DP

### solve 1

### solve 1 Code

```js
const coinChange = (coins, amount) => {
  const dp = Array.from({length: amount + 1}, () => Infinity);

  dp[0] = 0;

  for (let i = 1; i < dp.length; i++) {
    for (const coin of coins) {
      if (coin <= i) {
        dp[i] = Math.min(dp[i], dp[i - coin] + 1);
      }
    }
  }

  return dp[amount] === Infinity ? -1 : dp[amount];
};
```
