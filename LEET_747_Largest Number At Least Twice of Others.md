# LeetCode/747/Largest Number At Least Twice of Others

#배열

## Want
정수 배열이 주어졌을 때, (배열의 최대값 > 다른 값 * 2)이 참이라면 최대값 인덱스를 반환하고 거짓이라면 -1 반환  
단, 최대값은 단 한개만 존재한다. (최대값 중복 없음)  

## Understanding & Seperating
1. 각 원소의 2배 값을 갖는 새로운 배열 생성
2. 최대값, 최대값의 인덱스 찾기
3. 최대값이 자신을 제외한 다른 값의 2배가 넘는지 확인하여 값 반환

## Refactoring

### solve 1
nums 요소의 2배 값을 가지고 있는 배열을 생성했고 (doubledNums)  
`for문`을 통해 nums의 최대값과 최대값의 인덱스를 구했다 (max, maxIndex)  
다시 `for문`을 통해 max 자신을 제외한 값들과 max값의 대소관계를 파악해 반환값을 지정했다

### solve 2
`JavaScript-Like solve`  
굳이 nums 요소들을 2배한 배열을 생성할 필요가 없었다  
왜냐면 이러한 조건식은 마지막에 지정할 수 있다고 생각했고 더 `자바스크립트스러운` 코드를 작성하고 싶었다  
  
`map` 함수를 사용해 nums와 동일한 배열을 생성했다 (result)  
`Math.max`, `indexOf` 함수를 사용해 각각 최대값과 최대값의 인덱스를 구했다 (max, maxIndex)  
최대값을 제외한 값들에만 조건을 검사해야 하므로 `splice` 함수를 활용해 최대값을 잘라냈다  
`every` 함수를 활용해 (요소 * 2)와 max 값의 대소관계를 파악해 반환값을 지정했다

### solve 1

```js
// Runtime: 82 ms, faster than 48.55 %
// Memory Usage: 41.6 MB, smaller than 97.11 %
const dominantIndex = function (nums) {
  const doubledNumber = nums.map((num) => num * 2);

  let max = -1;
  let maxIndex;

  for (let i = 0; i < nums.length; i++) {
    if (max < nums[i]) {
      max = nums[i];
      maxIndex = i;
    }
  }

  for (let i = 0; i < doubledNumber.length; i++) {
    if (doubledNumber[i] > max && i !== maxIndex) {
      return -1;
    }
  }
  return maxIndex;
};
```

### solve 2

```js
// Runtime: 75 ms, faster than 60.69 %
// Memory Usage: 41.9 MB, smaller than 84.97 %
const dominantIndex = function (nums) {
  const result = nums.map((num) => num);
  const max = Math.max(...result);
  const maxIndex = result.indexOf(max);
  result.splice(maxIndex, 1);

  return result.every((num) => max >= num * 2) ? maxIndex : -1;
};
```

## 배운 점 or 주의할 점
시간 복잡도를 계속 생각하면서 코드를 작성하니 머리에서 계속 효율적인 코드를 생각하게 되고 더 좋은 코드가 없을지 고민한다  
효율성과 가독성 사이에서 끊임없는 줄타기를 하고 있지만 그 안에서 계속 유연성을 유지해야 한다고 느껴진다  
자바스크립트스러운 코드를 사용하려면, 자바스크립트와 친해져야 한다  
자바스크립트의 함수들은 가독성이 굉장히 높기 때문에 자바스크립트를 속속들이 알아야 한다..!
  
`solve 1`에서 let을 통해 선언한 max, maxIndex를 보며 굉장히 불편했었는데  
`solve 2`의 const를 사용하면서 해소되었고 대다수의 사람들이 무슨 기능을 하는 함수인지 쉽게 알아차릴 수 있는 코드를 작성한 것 같다