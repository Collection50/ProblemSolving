# Leetcode/53/Maximum Subarray

#DP

## Want

정수 배열이 `nums`가 주어진다  
부분 배열의 최대합을 반환하라  
[문제 링크](https://leetcode.com/problems/maximum-subarray/)

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
```

## Solving Strategies

메모이제이션을 활용하는 곳은 어디일까?  
어떤 값을 저장해서 사용해야 할까?  
배열을 반환하는게 아니라 값을 반환하는거네  
음수가 있기에 어려움이 있음

누적합  
`이전까지의 최대 누적합과 현재 값을 비교`

### solve 1

`Solving Strategies`의 코드화

### solve 1 Code

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
const maxSubArray = function (nums) {
  const dp = [0];

  for (let i = 0; i < nums.length; i++) {
    dp.push(Math.max(dp[i] + nums[i], nums[i]));
  }
  return Math.max(...dp.slice(1));
};
```

## 배운 점 or 주의할 점

DP도 열심히
