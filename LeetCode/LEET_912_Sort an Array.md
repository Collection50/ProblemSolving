# LeetCode/912/Sort an Araay

#Sort#MergeSort

## Want

정수 배열이 주어진다  
오름차순으로 정렬하여 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
```

## Solving Strategies

주어진 정수배열을 오름차순으로 정렬하여 반환하면 되는 문제  
여러가지 정렬 중에서, 대표적으로 3가지 정렬을 사용하여 문제를 해결했다

1. Quick Sort
2. Merge Sort
3. Tim Sort (자바스크립트 내장 `Array.sort()`)

### solve 1

흔히 **굉장히 빠른** 정렬 방법이라고 알려져있는 **퀵 정렬**을 사용하여 문제를 해결하였다  
피봇값을 기준으로 범위를 나눠 정렬을 하기에 **분할 정복**을 활용한 방법이며  
평균적으로 **시간 복잡도 O(NlogN)**을 유지하지만, 거의 정렬되어있는 경우 **시간 복잡도 O(N^2)**를 만족한다

### solve 1 Code

```js
// Runtime: 2358 ms, faster than 26.34%
// Memory Usage: 59.5 MB, smaller than 47.21%
const quickSort = (array, start, end) => {
  if (start >= end) return;

  let i = start + 1;
  let j = end;
  const pivot = start;

  while (i <= j) {
    while (array[i] <= array[pivot]) {
      i++;
    }
    while (array[j] >= array[pivot] && j > start) {
      j--;
    }

    if (i > j) {
      const temp = array[pivot];
      array[pivot] = array[j];
      array[j] = temp;
    } else {
      const temp = array[i];
      array[i] = array[j];
      array[j] = temp;
    }
  }
  quickSort(array, start, j - 1);
  quickSort(array, j + 1, end);
};

const sortArray = function (nums) {
  quickSort(nums, 0, nums.length - 1);
  return nums;
};
```

## Refactoring

### solve 2

분할 정복을 활용한 다른 정렬 방법인 **병합 정렬**을 활용하여 문제를 해결하였다  
병합 정렬의 경우 **Top-down**, **Bottom-up** 두 가지로 나눌 수 있는데  
아래는 **Top-down 방식을 활용**하여 문제를 해결하였다  
간략하게 설명하자면,

1. 분할: `sortArray` 재귀 함수를 활용해 각 배열의 요소를 쪼개지지 않을 때까지 쪼갠 뒤에
2. 병합: `merge` 함수를 활용하여 정렬을 수행하며 배열을 합친다

### solve 2 Code

```js
// Runtime: 236 ms, faster than 58.81%
// Memory Usage: 67.3 MB, smller than 24.41%
const merge = function (arr1, arr2) {
  const mergedArr = [];
  let i = 0;
  let j = 0;

  while (i < arr1.length && j < arr2.length) {
    if (arr1[i] < arr2[j]) mergedArr.push(arr1[i++]);
    else mergedArr.push(arr2[j++]);
  }
  while (i < arr1.length) {
    mergedArr.push(arr1[i++]);
  }
  while (j < arr2.length) {
    mergedArr.push(arr2[j++]);
  }

  return mergedArr;
};

const sortArray = function (nums) {
  if (nums.length <= 1) return nums;

  const mid = Math.floor(nums.length / 2);
  const left = sortArray(nums.slice(0, mid));
  const right = sortArray(nums.slice(mid));

  return merge(left, right);
};
```

### solve 3

자바스크립트의 내장 메서드인 `Array.sort()`를 활용하여 문제를 해결했다  
원래 정렬 내장 메서드는 Quick Sort를 사용했었는데, **Tim Sort**로 바뀌었다  
더하여 숫자를 정렬할 때는 콜백 함수를 전달해주어야 한다

### solve 2 Code

```js
const sortArray = function (nums) {
  return nums.sort((a, b) => a - b);
};
```

## 배운 점 or 주의할 점

주요한 정렬들을 모두 머리속에 넣어두고 쉽게 구현할 수 있었는데,  
오랜만에 정렬 메서드를 코드로 작성하려고 하다보니 헷갈리는 부분들이 존재했다  
예를 들어 퀵 정렬의 경우 비교 연산자를 사용할 때 **등호**가 들어가는지, 들어가지 않는지 헷갈리는 부분이 존재했었다  
**개념들을 이해하고 숙지하는 것과 그것을 코드로 작성해내는 능력은 별개의 능력**으로 보여진다  
영어 Reading을 잘한다고 해서 Speaking 실력과 온전히 비례하지 않는 것처럼 말이다  
나는 **두 마리 토끼를 모두 잡을 것**이다  
더하여 정렬 알고리즘들도 깃헙에 정리해야겠다고 여겨진다
