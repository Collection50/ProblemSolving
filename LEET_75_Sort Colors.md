# LeetCode/75/Sort Colors

#배열#정렬

## Want

정수 배열이 주어진다  
정수 배열은 **빨강, 하양, 파랑**을 각 의미하는 0, 1, 2로 이뤄져있다  
같은 색끼리 인접할 수 있도록 배열 그 자체를 정렬하여 반환하라  
언어의 내장 메서드는 사용할 수 없다

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
```

## Solving Strategies

퀵소트를 사용하여 배열을 정렬하여 반환한다

### solve 1

퀵소트 구현을 통한 문제 해결  
평균적인 시간 복잡도는 `O(NlogN)`이지만, 최악의 경우 `O(N^2)`

### solve 1 Code

```js
// Runtime: 101 ms, faster than 30.45%
// Memory Usage: 42 MB, smaller than 73.53%
const quickSort = function (array, start, end) {
  if (start >= end) return;

  let i = start + 1;
  let j = end;
  const pivot = start;
  let temp;

  while (i <= j) {
    while (array[pivot] >= array[i]) {
      i++;
    }

    while (array[pivot] <= array[j] && j > start) {
      j--;
    }

    if (i > j) {
      temp = array[j];
      array[j] = array[pivot];
      array[pivot] = temp;
    } else {
      temp = array[i];
      array[i] = array[j];
      array[j] = temp;
    }
  }

  quickSort(array, start, j - 1);
  quickSort(array, j + 1, end);
};

const sortColors = function (nums) {
  quickSort(nums, 0, nums.length - 1);
  return nums;
};
```

## Refactoring

### solve 2

시간 복잡도 O(N), 공간 복잡도 O(1)로 해결하라는 권장 사항이 존재했다  
`solve 2`는 0, 1의 개수를 세어준 후 `nums`에 순서대로 할당하는 방식으로 해결하였다  
시간 복잡도 `O(N)`

### solve 2 Code

```js
// Runtime: 100 ms, faster than 31.90%
// Memory Usage: 42.6 MB, smaller than 9.95%
const sortColors = function (nums) {
  let zeroCount = 0;
  let oneCount = 0;

  for (const num of nums) {
    if (num === 0) zeroCount++;
    else if (num === 1) oneCount++;
  }

  for (let i = 0; i < nums.length; i++) {
    if (i < zeroCount) nums[i] = 0;
    else if (i < zeroCount + oneCount) nums[i] = 1;
    else nums[i] = 2;
  }
};
```

### solve 3

`solve 2`의 코드를 들여다보면, 상수들이 비일비재하게 사용되었다  
사실 이러한 **하드코딩은 굉장히 나쁜 코드**다  
**실수의 가능성**도 굉장히 높고, **유지 보수하기에도 까다롭기** 때문이다  
따라서 상수를 덜 사용하는 방식으로 해결하려고 노력하였고  
`colors`라는 **배열**을 사용함으로써 배열 메서드를 사용할 수 있었다  
이 부분에서 많은 가독성, 효율성 증대가 발생했다고 생각한다  
시간 복잡도 `O(N)`

### solve 3 Code

```js
// Runtime: 102 ms, faster than 28.49%
// Memory Usage: 42.1 MB, smaller than 63.90%
const sortColors = function (nums) {
  const colors = [0, 0, 0];

  for (const num of nums) {
    colors[num]++;
  }

  let index = 0;

  for (let i = 0; i < colors.length; i++) {
    while (colors[i]--) {
      nums[index++] = i;
    }
  }
};
```

## 배운 점 or 주의할 점

시간 복잡도를 줄이려는 사고에서 많은 것이 트이는 것 같다  
더하여 다른 사람들의 좋은 코드를 보며 생각 방법도 트이는 것 같다  
**상수 공간 복잡도**를 사용해야 할 때는 **배열, 해시와 같은 자료구조를 이용**하는 것이 굉장히 효율적이다
