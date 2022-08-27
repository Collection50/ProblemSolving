# LeetCode/220/Contains Duplicate III

#배열

## Want

정수 배열과 정수 `k`, `t`가 주어진다  
서로 다른 두 인덱스 `i`, `j`가 아래 두 조건을 모두 만족한다면 `true`를 반환하라

1. `abs(nums[i] - nums[j]) <= t`
2. `abs(i - j) <= k`

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @param {number} t
 * @return {boolean}
 */
```

## Solving Strategies

1. 2번째 조건에 만족하는 범위 내에서 1번 조건을 판별하면 된다
2. 모든 경우에서 1,2번 조건을 만족하는 것이 있는지 확인해주어야 한다
3. 모든 경우의 수를 판별해야 하므로 완전 탐색으로 해결한다

### solve 1

해결하는 데 어렵지 않았다  
서로 다른 인덱스 `i`, `j`를 두고 `Want`의 1,2번 조건을 만족시키면 된다  
따라서 `k`번 반복하는 `while`문 내에서 1번 조건만 확인하면 되었다  
1,2번 조건을 만족한다면 바로 `true`를 반환하도록 하였고  
모든 경우에서 조건을 만족하지 못한다면 `false`를 반환하도록 하였다  
**시간 복잡도 O(N^2)**

### solve 1 Code

```js
// Runtime: 999 ms, faster than 56.50%
// Memory Usage: 43 MB, smaller than 74.67%
const containsNearbyAlmostDuplicate = function (nums, k, t) {
  for (let i = 0; i < nums.length - 1; i++) {
    let j = i + 1;

    while (Math.abs(i - j) <= k && j < nums.length) {
      if (Math.abs(nums[i] - nums[j]) <= t) {
        return true;
      }
      j++;
    }
  }
  return false;
};
```

## 배운 점 or 주의할 점

시간 복잡도 O(N)으로 풀 수 있다고 하는데  
어떤 방법을 사용해야 할까?  
조금 더 고민이 필요해 보인다
