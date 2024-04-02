# Leetcode/45/Jump Game II

#DP

## Want

각 요소 `nums[i]`는 인덱스 i에서 앞으로 점프할 수 있는 최대 길이를 나타냅니다  
즉 `nums[i]`에 있는 경우 `nums[i + j]`의 모든 위치로 점프할 수 있습니다  
`nums[n - 1]`에 도달하기 위한 최소 점프 횟수를 반환합니다  
[문제 링크](https://leetcode.com/problems/jump-game-ii/description/)

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
```

## Solving Strategies

dp로 해결할 수 있는문제  
return 값은 `dp[dp.length - 1]`  
그러니까 최단 경로(거리)를 찾는 문제인건데  
bottom-up 방식으로 하는게 제일 좋네  
시간 복잡도 `O(N)`  
최악의 경우 `O(N^2)`

1. `dp` 배열 생성 `index`로 초기화
2. `nums`순회
3. `nums[i]`에 존재하는 수만큼 앞의 dp 배열 업데이트
4. `dp[i] = Math.max(dp[i], nums[j] + 1);`
5. `return dp[dp.length - 1]`;

### solve 1

### solve 1 Code

```js
const jump = function (nums) {
  const dp = Array.from({ length: nums.length }, (_, i) => i);

  nums.forEach((num, i) => {
    for (let j = 0; j <= num; j++) {
      if (i + j !== 0) {
        dp[i + j] = Math.min(dp[i] + 1, dp[i + j]);
      }
    }
  });
  return dp[nums.length - 1];
};
```

## 배운 점 or 주의할 점

재활운동
