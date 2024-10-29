# Leetcode/309/Best Time to Buy and Sell Stock with Cooldown

#DP

### solve 1

### solve 1 Code

```js
const maxProfit = (prices) => {
  let hold = -prices[0];
  let sold = 0;
  let rest = 0;

  for (let i = 1; i < prices.length; i++) {
    let prevHold = hold;
    let prevSold = sold;
    let prevRest = rest;

    hold = Math.max(prevRest - prices[i], prevHold);
    sold = prevHold + prices[i];
    rest = Math.max(prevRest, prevSold);
  }

  return Math.max(sold, rest);
};
```
