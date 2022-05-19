# LeetCode/724/Find Pivot Index

#배열#투포인터

## Want
정수 배열이 주어졌을 때, `(왼쪽 합 === 오른쪽 합)`을 `true`로 만드는 pivot 인덱스를 반환하라  

단, 인덱스가 맨 왼쪽에 위치했을 때는 (인덱스 === edge)  
0번째 값 기준 `왼쪽 값들의 합 === 0`으로 간주한다  
만약 피봇 인덱스가 존재하지 않을 경우 -1 반환  

## Understanding & Seperating
1. 새로운 배열 생성
2. 인덱스가 0일 때 오른쪽 값들의 합이 0이라면 0 반환
3. 피봇값을 기준으로 양쪽으로 나눠 양쪽의 합을 구해 더한다
4. 왼쪽값 + 오른쪽값 == 0 이라면 피봇값 반환 (leftSum, rightSum)
5. 2, 4번 조건을 만족하지 않는다면 -1 반환 (FALSE_NUM)

## Refactoring

### solve 1
`nums`와 같은 값을 갖는 배열을 생성했다 (array)  
인덱스가 edge일 때의 상황을 고려하기 위해 배열의 총합을 `sumRight` 변수에 할당했고  
이중 반복문을 통해 `pivot`값을 기준으로 왼쪽 합과 오른쪽 합을 더하여 비교했다  
<u>역시나 무지막지한 런타임</u>이 소요되었다..  
`시간복잡도 O(N ** 2)`  

### solve 2
문제를 보자마자 떠오른 것이  `어? 투포인터다.`라는 생각이었고  
`시간복잡도 O(N)`으로 충분히 줄일 수 있다고 생각했다  
증가하는 `pivot`에 해당하는 값만 `sumLeft`, `sumRight에서` 빼준 후 값을 비교하면 됐다  
인덱스 edge일 경우, `sumRight의` 값은 0번째 인덱스의 값을 포함하지 않아도 되므로 반복문을 거꾸로 돌려줬다  
`pivot`에 해당하는 값을 `sumLeft에` 더해주었고  
`sumRight`에서 빼주면 증가하는 `pivot`값에 따라 왼쪽 합과 오른쪽 합을 빠르게 비교할 수 있었다  
`시간복잡도는 O(N)`이지만, 생각보다 코드가 깔끔하지 않다  
내가 생각했을 때 로직은 간단한데, **코드가 복잡**했다  

### solve 3
`JavaScript-Like solve`  
문제의 반환형은 `값`이다!  
`reduce`함수를 활용해 총합을 구해줬다 (totalSum)  
`totalSum`에서 `pivot`에 해당하는 값과 leftSum을 빼주면 자연스럽게 오른쪽 합이 구해진다
왼쪽 합과 오른쪽 합을 비교하여 결과를 반환하였다
`시간복잡도 O(N)`  

### solve 1

```js
const pivotIndex = function (nums) {
  // solve 1
  // Runtime: 1597 ms, out of range
  // Memory Usage: 43.9 MB, smaller than 47.74 %
  const array = nums.map((num) => num);
  const FALSE_NUM = -1;

  let sumRight = 0;
  let sumLeft = 0;
  let pivot = 0;

  for (let i = 0; i < array.length - 1; i++) {
    sumRight += array[i + 1];
  }
  if (sumRight === 0) return 0;

  while (pivot++ < array.length - 1) {
    sumRight = 0;
    sumLeft = 0;

    for (let i = 0; i < array.length; i++) {
      if (i < pivot) sumLeft += array[i];
      else if (i > pivot) sumRight += array[i];
    }
    if (sumRight === sumLeft) return pivot;
  }
  return FALSE_NUM;
};
```

### solve 2

```js
const pivotIndex = function (nums) {
  // solve 2
  // Runtime: 74 ms, faster than 92.03 %
  // Memory Usage: 43.9 MB, smaller than 96.55 %
  const array = nums.map((num) => num);
  const FALSE_NUM = -1;
  let sumLeft = 0;
  let sumRight = 0;

  for (let i = array.length - 1; i > 0; i--) {
    sumRight += array[i];
  }
  if (sumRight === 0) return 0;

  for (let pivot = 1; pivot < array.length; pivot++) {
    sumLeft += array[pivot - 1];
    sumRight -= array[pivot];
    if (sumLeft === sumRight) return pivot;
  }
  return FALSE_NUM;
};
```

### solve 3
```js
const pivotIndex = function (nums) {
  // solve 3
  // Runtime: 103 ms, faster than 51.97 %
  // Memory Usage: 44.2 MB, smaller than 81.30 %
  const totalSum = nums.reduce((acc, cur) => (acc + cur), 0);
  const FALSE_NUM = -1;
  let leftSum = 0;

  for (let pivot = 0; pivot < nums.length; pivot++) {
    if (totalSum - nums[pivot] - leftSum === leftSum) {
      return pivot;
    }
    leftSum += nums[pivot];
  }
  return FALSE_NUM;
};
```

## 배운 점 or 주의할 점
**Want**, **Understanding & Seperating**을 통해 문제를 풀기 전 생각하고 그림을 그린다  
1. 문제에서 원하는 것이 무엇인지
2. 내가 문제를 제대로 이해했는지
3. 세분화 생각  

1, 2, 3번을 마친 후 키보드에 손을 올린다  
하지만 이에 더해 **입력 형태 + 반환 형태**도 함께 생각해야 한다고 느꼈다  

-1을 반환하는 것처럼 반환값이 **상수**일 경우, `FALSE_NUM과` 같은 변수로 반환하는 것이 훨씬 `좋은 코드`인 것 같다  

`solve 1`을 보면 숨이 턱 막히는 것 같다  
문제를 해석하는 시간에 더해 **코드를 해석하는 시간**도 상당히 소요될 것 같다  
사실 가장 아쉬운 점은 문제를 푼 사람의 <u>의도가 굉장히 모호하게</u> 느껴진다는 점이다  

결국 `좋은 코드`란?  
가독성과 효율성 두마리 토끼를 모두 잡는 코드이지 않을까 싶다  
`solve 3`을 보면 코드를 이해하는 데 긴 시간이 걸리지 않으며
무엇보다 문제를 푼 사람의 <u>의도가 명확하게</u> 느껴진다  
각 코드가 긴밀하게 연결되어 **유기적**으로 움직이는 코드를 작성하고 싶다  
**살아있는 것**처럼 말이다  

어쩌면 좋은 코드 === <u>숨 쉬는 코드</u>이지 않을까

이러한 이유로 **리팩토링이 즐겁다**  
코드를 살아숨쉬게 만들면 나도 살아있는 것처럼 느껴진다
