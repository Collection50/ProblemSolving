# LeetCode/283/Move Zeroes

#배열

## Want
배열이 주어졌을 때 세 번째로 큰 수를 반환하라  
단, 세 번째로 큰 수가 존재하지 않을 경우 최대값을 반환하라  

## Understanding & Seperating
1. 새로운 배열 or 객체 생성
2. 정렬 수행 (내림차순)
3. 중복 제거
4. 만약 배열의 길이가 3이상이라면 arr[2] 반환
5. 배열의 길이가 3 미만이라면 arr[0] 반환

- 정렬 == `sort`로 수행
- 중복 제거는?  
1) 객체 통한 중복 여부 검사  
2) `Set` 객체 이용

## Refactoring

### solve 1
중복을 제거하기 위한 exists 객체를 생성했고  
`filter` 함수를 활용해 exists 객체에 없는 요소들만 array 배열에 저장했다  
sort 함수까지 `메서드 체이닝`하여 가독성을 높였다  
정렬된 배열의 개수가 3개 이상 => 세 번째로 큰 값을 출력  
3개 미만 => 최대값 출력하는 로직으로 작성했다  

### solve 2
하지만 나는 배열의 배열 고차 함수 없이 풀고 싶었고  
시간 복잡도를 위해 `중복을 제거하지 않고` 풀 수 있다고 생각했다(~~하지만 시간 복잡도가 더 높게 나옴~~)  
내림차순으로 정렬된 배열에서 최대값보다 세 번째로 작은 수를 됐다  
이 방법은 중복을 제거하지 않기 때문에 배열의 개수가 3개 이상이어도 최대값을 찾는 반복문이 실행된다  
`[1, 1, 1, 1, 2, 2]`인 경우에도 세 번째로 큰 수는 존재하지 않기 때문이다  
이 부분에서 시간 복잡도가 증가하지 않았을까  

### solve 3
근데 여기서 만족하고 싶지 않다
내가 자바스크립트로 알고리즘을 푸는 이유가 존재해야 했다  
`JavaScript-Like solve`
Set 객체를 활용하여 중복을 제거 해줌과 동시에 스프레드 문법을 활용하여  
배열 고차 함수인 sort를 사용할 수 있도록 했다  
마지막으로 배열의 개수에 따라 반환값에 차이를 두었다

### solve 1

```js
  // Runtime: 83 ms, faster than 53.03 %
  // Memory Usage: 44.9 MB, smaller than 23.01 %
const thirdMax = function(nums) {
  const exists = {};

  const array = nums
  .filter((num) => {
    if (exists[num]) return false;
    exists[num] = 1;
    return true;
  })
  .sort((a, b) => b - a);

  if (array.length >= 3) return array[2];
  return array[0];
};
```

### solve 2

```js
  // Runtime: 86 ms, faster than 51.51 %
  // Memory Usage: 42.6 MB, smaller than 76.14 %
const thirdMax = function(nums) {
  const array = nums.sort((a, b) => b - a);
  let nthMax = array[0];
  let count = 0;

  if (array.length > 2) {
    for (let i = 1; i < array.length; i++) {
      if (nthMax > array[i]) {
        nthMax = array[i];
        count++;
      }
      if (count === 2) {
        return array[i];
      }
    }
  }
  return array[0];
};
```

### solve 3

```js
// Runtime: 70 ms, faster than 80.02 %
// Memory Usage: 44.6 MB, smaller than 30.78 %
// JavaScript-like solve
const thirdMax = function(nums) {
  const array = [ ...new Set(nums) ].sort((a, b) => b - a);

  if (array.length >= 3) {
    return array[2];
  }
  return array[0];
};
```

## 배운 점 or 주의할 점
solve 3가 제일 오래 걸릴 줄 알았는데 제일 빨랐고  
배열 고차 함수를 제일 적게 사용한 순서대로 더 오래 걸렸다  
둘 다 중요한 것 같다
1. 하나하나 해부 해보면서 `for문`으로 구현하며 로직을 머리와 손끝으로 느끼는 방법
2. 언어에서 제공하는 강력한 함수들을 사용한 `Language-Like` 풀이 방법
