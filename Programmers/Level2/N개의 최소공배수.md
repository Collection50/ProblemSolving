# Programmers/N개의 최소공배수

#정수론#NumberTheory

## Want

`n`개의 숫자를 담은 배열 `arr`이 입력되었을 때 이 수들의 최소공배수를 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[]} arr
 * @return {number}
 */
```

## Solving Strategies

`n`개 값의 모든 최소공배수를 구한다  
이는 각 1개씩의 최소공배수를 구하여 반환하는 것과 동일하다

### solve 1

`Solving Strategies`의 코드화

1. `GCD`, `LCM` 함수 정의
2. `GCD`, `LCM`이 올바르게 동작하려면, `a > b`가 충족되어야 한다
3. 따라서 `arr`을 내림차순으로 정렬
4. 각 값에 대해 최소공배수를 구한다 (`reduce`)

### solve 1 Code

```js
function GCD(a, b) {
  return b === 0 ? a : GCD(b, a % b);
}

function LCM(a, b) {
  return Math.floor((a * b) / GCD(a, b));
}

function solution(arr) {
  return arr
    .slice()
    .sort((a, b) => b - a)
    .reduce((tempLCM, num) => LCM(tempLCM, num), arr[0]);
}
```

## 배운 점 or 주의할 점

최소공배수와 최대공약수에 관한 내용을 알고 있다면 어렵지 않게 해결할 수 있다  
모든 수의 최대공약수는 각 2개 값마다의 최대공약수를 연속적으로 적용하면 된다
