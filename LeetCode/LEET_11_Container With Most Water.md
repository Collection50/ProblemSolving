# LeetCode/11/Container With Most Water

#Array#TwoPointer

## Want

`0 ~ 10^4`의 높이를 가진 `n`개의 막대가 주어진다  
두 개의 막대로 상자를 만들어 물을 담을 수 있다고 가정하였을 때  
물을 가장 많이 물을 담을 수 있도록 하는 상자의 넓이를 구하라

## INPUT && OUTPUT

```js
/**
 * @param {number[]} height
 * @return {number}
 */
```

## Solving Strategies

투포인터 활용  
양 옆에서 증가하거나 감소하는 인덱스  
막대의 최대 개수가 `10^5`개이기에  
**O(N^2)**으로 해결할 수 없는 문제라고 판단하였다

### solve 1

1. 맨 왼쪽, 맨 오른쪽 인덱스로 초기화 (`start`, `end`)
2. 각 인덱스의 막대 값 중 최소값 선택
3. 상자 넓이 === `2번 결과 * 인덱스 차이`
4. `max` 업데이트
5. 각 인덱스의 막대 값을 비교해 작은 쪽을 더하거나 빼기
6. `max` 반환

`heights[start]`, `heights[end]` 중  
작은 쪽에 해당하는 인덱스를 감소하거나 더한다  
물을 담을 수 있는 상자의 최대값을 구하기 위함이다  
양쪽 끝에서부터 높은 막대를 찾아가며 범위를 좁힌다  
해당 범위의 상자 크기를 갱신하며 최대 넓이를 구한다

### solve 1 Code

```js
const maxArea = function (heights) {
  let max = -Infinity;
  let start = 0;
  let end = heights.length - 1;

  while (start < end) {
    const tempHeight = Math.min(heights[start], heights[end]);
    const tempArea = tempHeight * (end - start);
    max = Math.max(tempArea, max);

    if (heights[start] <= heights[end]) {
      start++;
    } else {
      end--;
    }
  }
  return max;
};
```

## 배운 점 or 주의할 점

간단한 문제라고 생각했는데 생각보다 어려웠다  
더하여 투포인터 문제라는 것을 분류하는 데 시간도 소요되었다  
투포인터 카테고리를 오랜만에 마주쳐서 그런 것 같다  
다양한 문제를 꾸준히 해결하는 연습이 필요하다  
더하여 각 문제를 분류하는 기준이 필요하다고 느낀다
