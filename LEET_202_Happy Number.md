# LeetCode/202/Happy Number

#해시

## Want

주어진 정수가 HappyNumber인지 판별하라  
HappyNumber 조건

1. 1 이상의 정수로 시작하는 정수 `n`은 각 자리수의 제곱의 합으로 바뀐다
2. 바뀐 값이 1이 될 때까지 1번을 반복한다
3. 1로 바뀐다면 HappyNumber이며 `true`를 반환하고
4. 바뀌지 않는다면 `false` 반환

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @return {boolean}
 */
```

## First Understanding & Seperating

1. 각 자리수를 분해하여 제곱을 만든 후 더해주는 함수 작성
2. 1번에서 얻은 값이 `set`에 존재한다면 cycle이므로 `false` 반환
3. 존재하지 않는다면 `set`에 `add`
4. HappyNumber 여부 `return`

### solve 1

주어진 정수 `n`은 1이 될 때까지 각 자리수를 제곱한 값을 더하여 **새로운 수**로 바뀐다  
이 과정에서 **cycle**이 만들어지는 경우가 발생하는데 이 경우는 **Happy Number가 될 수 없는 경우**다  
따라서 `getDigitSquareSum`을 통해 각 자리수 제곱을 더하여 새로운 정수를 생성해주었고  
모든 경우에서 발생할 수 있는 `num`을 `set`에 저장하여 중복 여부를 확인할 수 있도록 하였다

### solve 1 Code

```js
// Runtime: 108 ms, faster than 41.40%
// Memory Usage: 43.8 MB, smaller than 59.58%
const getDigitSquareSum = function (num) {
  return num
    .toString()
    .split('')
    .reduce((acc, cur) => acc + (+cur) ** 2, 0);
};

const isHappy = function (n) {
  const set = new Set();
  let num = n;

  while (num !== 1) {
    if (set.has(num)) return false;
    set.add(num);
    num = getDigitSquareSum(num);
  }
  return true;
};
```

## 배운 점 or 주의할 점

요즘 문제를 풀면서 드는 생각인데, 꽤 수학 문제를 풀 듯이 접근해야 하는 것들이 존재한다  
규칙성을 찾는다던지, 시도해보면서 예외를 찾는다던지 말이다  
2022.08월 현재를 기준으로 코딩 테스트의 추세도 알고리즘 지식에 기반한 문제풀이보다  
창의성, 사고력을 요하는 문제로 바뀌어가고 있다고 한다  
내가 작성한 코드 한 줄, 한 줄에 의도를 부여하자
