# LeetCode/70/Climbing Stairs

#DP

## Want

`n`개의 계단을 오르고 있다  
한번에 1개 or 2개의 계단만 오를 수 있다  
계단을 오르는 방법을 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @return {number}
 */
```

## Solving Strategies

`dp`를 활용하여 해결할 수 있는 문제  
`dp` + 재귀 활용

점화식 `f(n) === f(n - 1) + f(n - 2)`  
1칸을 오르는 경우의 수 `f(1) === 1`  
2칸을 오르는 경우의 수 `f(2) === 2`

### solve 1

`Solving Strategies`의 코드화  
**Top down** 방식으로 해결  
재귀를 통하여 순차적으로 해결하였다

### solve 1 Code

```js
const climbStairs = function (n) {
  const array = [0, 1, 2];

  const memo = (num) => {
    if (array[num]) {
      return array[num];
    }
    array[num] = memo(num - 1) + memo(num - 2);
    return array[num];
  };
  return memo(n);
};
```

## Refactoring

1. **Bottom Up**
2. 공간 복잡도 향상

2가지 방식으로 리팩토링

### solve 2

**Bottom Up** 방식으로 해결
반복문을 활용하여 다음 수를 구해주었다

### solve 2 Code

```js
const climbStairs = function (n) {
  const array = [0, 1, 2];

  for (let i = 3; i <= n; i++) {
    array.push(array[i - 1] + array[i - 2]);
  }
  return array[n];
};
```

### solve 2

배열 대신 변수를 활용하여  
공간 복잡도의 최적화하는 방법

### solve 2 Code

```js
const climbStairs = (n) => {
  if (n <= 2) return n;

  let prev2 = 1;
  let prev1 = 2;
  let curr = 0;

  for (let i = 3; i <= n; i++) {
    curr = prev2 + prev1;
    prev2 = prev1;
    prev1 = curr;
  }
  return curr;
};
```

## 배운 점 or 주의할 점

각 주차별로 다른 알고리즘을 해결할 생각이다  
밸런스가 처참히 무너진 육각형 모습인 것 같아서  
다른 부분도 열심히 공부할 생각이다

이번 주는 `DP`-다
