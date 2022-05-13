# LeetCode/283/Move Zeroes

#배열

## Solving

want: 정수 배열이 주어졌을 때, 숫자 0을 배열의 끝부분에 위치하도록 하자 => `0이 아닌` 정수들을 앞쪽으로 정렬

단, 배열 자체가 수정되어야 한다


## Understanding & Seperating`
1. 0을 만나면 splice
2. splice 실행될 때마다 count++
3. count 수만큼 0 push



## Refactoring

### solve 1
배열에서 특정 숫자를 지우고 뒤에 있는 요소들을 한 자리씩 앞당기는 문제들은(직관적으로 요소가 사라지고 그 빈자리를 채우는 형태)
JS의 고차 배열 함수인 `splice` 함수를 사용하면 굉장히 간단하게 풀 수 있다

### solve 2
하지만 나는 `splice`함수를 사용하지 않고 풀고 싶었다
`splice` 함수도 내부적으로는 for문이 돌아가는 것을 알기에 순수 for문을 사용하여 문제를 리팩토링했다

### solve 3
아직 사라지지 않은 의문.
내 코드보다 더 좋은 코드가 있지 않을까?
`가독성 or 퍼포먼스` 측면에서 말이다
snowBall 개념을 적용해 0의 개수가 많아질수록 snowBall이 굴러가면서
0과 0이 아닌 수를 교체하는 인덱스의 크기가 늘어나는 로직이다


### solve 1

```js
// Runtime: 128 ms, faster than 45.48%
// Memory Usage: 46.7 MB, faster than 55.03%
const moveZeroes = function (nums) {
  let zeroCount = 0;

  for (let i = 0; i < nums.length; i++) {
    if (nums[i] === 0) {
      nums.splice(i--, 1);
      zeroCount++;
    }
  }
  for (let i = 0; i < zeroCount; i++) {
    nums.push(0);
  }
};
```

### solve 2

```js
// Runtime: 91 ms, faster than 89.59%
// Memory Usage: 46 MB, faster than 63.88 %
const moveZeroes = function (nums) {
  let valueIdx = 0;

  for (let i = 0; i < nums.length; i++) {
    if (nums[i] !== 0) {
      let temp = nums[valueIdx];
      nums[valueIdx++] = nums[i];
      nums[i] = temp;
    }
  }
};
```

### solve 3

```js
// Runtime: 131 ms, faster than 42.56%
// Memory Usage: 42.56 MB, faster than 73.09%
const moveZeroes = function (nums) {
  let snowBall = 0; 
    for (let i = 0; i < nums.length; i++) {
      if (nums[i] == 0){
        snowBall++;
      } else if (snowBall > 0) {
        let temp = nums[i];
        nums[i] = 0;
        nums[i - snowBall] = temp ;
    }
  }
};
```


## 배운 점 or 주의할 점
`splice` 함수 사용하여 현재 index의 값을 제거할 때,
값을 제거한 이후의 배열의 index와 값이 변경되므로 순회 시의 i값의 변화에 주의하자
시간복잡도와 공간복잡도의 개념을 머리속에 계속 생각하면서 문제를 풀어야 할 듯
