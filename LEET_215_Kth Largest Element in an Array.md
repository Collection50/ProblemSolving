# LeetCode/215/Kth Largest Element in an Array

#정렬#배열

## Want

정수 배열과 정수 `k`가 주어진다  
`k`번째로 큰 값을 찾아서 반환하라  
시간 복잡도 **O(N)**을 만족하라

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
```

## Solving Strategies

Quick Select 알고리즘을 사용하여 문제 해결  
[Quick Selet란?](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Quick%20Select.md)

### solve 1

**Quick Select**을 사용하여 문제를 해결하였다  
문제의 조건에서 반드시 **시간 복잡도 O(N)**을 유지하며 해결하라는 문구가 존재했는데  
이는 Quick Select를 사용하라는 말과 동일하다고 판단하였고 아래와 같이 문제를 해결하려고 노력하였지만  
**실패**하였다..

### solve 1 Code

```js
// Fail..
const findKthLargest = function (nums, k) {
  const getRandom = (min, max) => min + Math.floor(Math.random() * (max - min));

  const quickSelect = (array, order) => {
    const pivotIndex = getRandom(0, array.length);
    const bigger = [];
    const smaller = [];

    array.forEach((num) => {
      if (num > array[pivotIndex]) {
        bigger.push(num);
      } else if (num < array[pivotIndex]) {
        smaller.push(num);
      }
    });

    if (bigger.length >= order) {
      return quickSelect(bigger, order);
    }
    if (bigger.length + 1 === order) {
      return array[pivotIndex];
    }
    return quickSelect(smaller, order - bigger.length - 1);
  };

  return quickSelect(nums, k);
};
```

## Refactoring

### solve 2

`quickSelect`만 따로 떼놓고 봐보자  
`solve 1`에서 내가 간과했던 점은 `array[pivotIndex]`값을 기준으로 나누다 보니  
`array[pivotIndex]`과 같은 모든 값들이 `bigger`, `smaller` 어디에도 속하지 못해 올바른 분할이 이뤄지지 않았다  
따라서 `pivotIndex`의 값에 따라 출력값이 매번 달라졌었다

`solve 2`에서는 이러한 것을 보완하여 다시 로직을 작성하였고  
**pivot**으로 설정된 값을 제외하고 분할해주었다

### solve 2 Code

```js
// Runtime: 157 ms, faster than 68.01%
// Memory Usage: 59.9 MB, smaller than 10.58%
const quickSelect = (array, order) => {
  const pivotIndex = getRandom(0, array.length);
  const bigger = [];
  const smaller = [];

  for (let i = 0; i < array.length; i++) {
    if (pivotIndex === i) continue;

    const value = array[i];

    if (value > array[pivotIndex]) {
      bigger.push(value);
    } else {
      smaller.push(value);
    }
  }

  if (bigger.length >= order) {
    return quickSelect(bigger, order);
  }
  if (bigger.length + 1 === order) {
    return array[pivotIndex];
  }
  return quickSelect(smaller, order - bigger.length - 1);
};
```

### solve 3

`for ... of`문을 사용하여 가독성을 증가시켰다  
객체에서만 존재하는 `entries()`를 배열에서도 사용할 수 있다는 것을 배워서 활용하였다  
따라서 로직은 `solve 2`와 동일하지만 전체적인 가독성을 향상시켜  
`JavaScript-Like` 코드로 작성하였다

### solve 3 Code

```js
// Runtime: 210 ms, faster than 43.48%
// Memory Usage: 72.8 MB, OUT OF RANGE
const quickSelect = (array, order) => {
  const pivotIndex = getRandom(0, array.length);
  const bigger = [];
  const smaller = [];

  for (const [index, num] of array.entries()) {
    if (index === pivotIndex) continue;

    if (num > array[pivotIndex]) bigger.push(num);
    else smaller.push(num);
  }

  if (bigger.length >= order) {
    return quickSelect(bigger, order);
  }
  if (bigger.length + 1 === order) {
    return array[pivotIndex];
  }
  return quickSelect(smaller, order - bigger.length - 1);
};
```

## 배운 점 or 주의할 점

내가 아직 배우지 못한 빌트인 객체의 내장 메서드들이 많이 존재한다는 것을 느끼고 있다  
배열의 `entries()`, 문자열의 `padStart()`등 많은 메서드들을 잘 활용하고 싶다  
자바스크립트에 대한 이해도를 높이자..!!
