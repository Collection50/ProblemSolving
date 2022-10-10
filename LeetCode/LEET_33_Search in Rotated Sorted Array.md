# LeetCode/33/Search in Rotated Sorted Array

#BinarySearch

## Want

오름차순으로 정렬되어있고, 각 값이 다른 정수로 이뤄진 배열이 주어진다  
`search` 함수로 전달되기 전, 정수 배열은 오른쪽으로 `k`번 순환된다(이동)  
`([0, 1, 2, 4, 5, 6, 7], k = 3) => [4, 5, 6, 7, 0, 1, 2]`  
이러한 정수 배열과 정수 `target`이 주어졌을 때  
정수 배열 안에 `target`이 존재한다면 인덱스를 반환하고, 없다면 `-1`을 반환하라
시간 복잡도 O(logN)을 유지해야 한다

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
```

## Solving Strategies

이분 탐색을 하기 위해선, 탐색 대상이 **오름차순으로 정렬**되어 있어야 한다  
하지만 주어진 배열은 오름차순으로 정렬된 배열이 `k`번 순환된 형태로 주어진다  
따라서 일반적인 이진 탐색은 사용할 수 없고

1. **오름차순 여부**
2. 범위 내 `target`의 **존재 여부를 확인**하며 값의 범위를 줄여나가야 한다

### solve 1

쉽게 설명하자면 다음과 같다

1. `[start, mid - 1]` 범위의 오름 차순 여부를 확인한다
2. 1번이 오름차순이 아니라면 자연스럽게 `[mid + 1, end]` 범위가 오름차순으로 여겨진다
3. 오름차순인 범위 내에 `target`이 존재하는지 확인하여 범위를 수정한다

오름차순 판별이 중요한 이유는, **오름차순이어야 이분 탐색을 사용**하여  
`target`과의 비교를 통해 `start`, `end`를 변경할 수 있기 때문이다

따라서 아래 2개를 만족하며 조건문을 사용하는 것이 문제 해결의 핵심이라고 볼 수있다

1. 오름차순 여부를 확인하여 `target` 유무 확인
2. `start`, `end` 변경으로 탐색 범위 줄여가기

### solve 1 Code

```js
// Runtime: 69 ms, faster than 85.95%
// Memory Usage: 41.9 MB, smaller than 86.93%
const search = function (nums, target) {
  if (!nums.length) return -1;

  let start = 0;
  let end = nums.length - 1;

  while (start <= end) {
    const mid = Math.floor((end - start) / 2);

    if (nums[mid] === target) return mid;

    if (nums[start] <= nums[mid]) {
      if (target >= nums[start] && target < nums[mid]) {
        end = mid - 1;
      } else {
        start = mid + 1;
      }
    } else if (target > nums[mid] && target <= nums[end]) {
      start = mid + 1;
    } else {
      end = mid - 1;
    }
  }
  return -1;
};
```

## 배운 점 or 주의할 점

나는 이 문제가 **꽤나 어려웠다**
보통의 이분 탐색 개념이 아니라는 것을 떠나,
이분 탐색에서 **이중 조건문을 사용**하는 발상이 잘 떠오르지 않았기 때문이다  
오름차순 여부를 확인하여 범위를 설정하는 것까지는 구상이 그려졌으나,  
`target`의 존재 여부를 확인하여 범위를 설정하는 것이 잘 떠오르지 않았다
문제의 의도를 잘 파악하여 **해결 전략을 겹치지 않게, 빠짐없이** 세우는 게 중요하다  
이것도 실무에서 사용되는 경우가 존재하겠지
