# LeetCode/162/Find Peak Element

#BinarySearch

## Want

정수 배열이 주어진다  
`peak element`의 인덱스를 반환하라  
`peak element`란 정확히 양 옆에 있는 수들보다 큰 수를 의미한다

만약 여러 개의 `peak element`가 존재한다면, 어떤 것을 반환해도 된다  
단, 올바르지 않은 인덱스의 경우 `-Infinity로` 간주한다  
**시간 복잡도 O(logN)** 을 유지하여 풀어야 한다

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
```

## Solving Strategies

관건: `pivot`값을 어떤 기준으로 만들 것인가?
결국 **탐색 범위를 반으로 좁혀가며** 해결해야 한다

1. 배열 중앙에 있는 값, 맨 끝의 양 옆을 비교한다 (`mid`, `start`, `end`)
2. 중앙에 존재하는 값과 양쪽 값의 비교를 통해 큰 값쪽으로 범위를 축소해나간다
3. `peak`값을 찾았다면 인덱스 `return`

### solve 1

이진 탐색으로 문제를 해결하기 위해선, 문제의 **범위를 반으로 줄여**가야 한다  
그래야 **시간 복잡도 O(logN)** 을 만족시킬 수 있기 때문이다  
따라서 중앙에 존재하는 값 `mid`를 찾아내어  
`mid`의 양쪽 값을 비교한 후 `mid`보다 **큰 값이 존재하는 쪽으로 범위를 좁혀** 나갔다

더하여 양쪽 값을 쉽게 비교하기 위해 `isPeak` 함수를 사용하였는데  
문제의 조건에서 올바르지 않은 인덱스의 경우 `-Infinity` 값을 가진다고 하였다  
이는 **배열의 맨 왼쪽 값은 오른쪽 값**만 비교, **맨 오른쪽 값은 왼쪽 값**만 비교하면 된다는 것을 의미한다  
따라서 올바르지 않은 인덱스를 참조했을 경우, 논리 연산자 `||`를 통해 `-Infinity`와 비교하여  
오류 가능성과 유지 보수성을 증가하였다  
(이러한 부분에 특별히 **더 시간을 소모한 것이 아니라**, 머릿속에서 자연스럽게 이러한 코드가 떠오름)

더하여 조건만 만족한다면 어떤 값을 반환해도 좋으므로,  
`start`, `mid`, `end` 모두 `isPeak` 함수의 인수로 넣어주었다

### solve 1 Code

```js
const findPeakElement = function (nums) {
  const isPeak = (center) =>
    (nums[center - 1] || -Infinity) < nums[center] &&
    (nums[center + 1] || -Infinity) < nums[center];

  let start = 0;
  let end = nums.length - 1;

  while (start <= end) {
    const mid = Math.floor((start + end) / 2);

    if (isPeak(start)) return start;
    if (isPeak(end)) return end;
    if (isPeak(mid)) return mid;

    if (nums[mid] < nums[mid + 1]) {
      start = mid + 1;
    } else {
      end = mid - 1;
    }
  }
  return -1;
};
```

## 배운 점 or 주의할 점

몇 번이고 설명했지만, **_이진 탐색에서 가장 중요한 점은 범위를 반으로 줄여나가는 것_** 에 있다  
따라서 범위를 반으로 줄이기 위한 `pivot` 선정의 기준이 중요하다고 생각한다  
내 코드에 하나하나 **이유를 대자**  
[Discuss](https://leetcode.com/explore/learn/card/binary-search/126/template-ii/948/discuss/2414855/Javascript-readable-Code!!!!!!!!)
