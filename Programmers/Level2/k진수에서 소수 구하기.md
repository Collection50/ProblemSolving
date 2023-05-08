# Programmers/k진수에서 소수 구하기

#문자열

## Want

정수 `n`과 `k`가 매개변수로 주어진다  
`n`을 `k`진수로 바꿨을 때, 변환된 수 안에서 찾을 수 있는 소수의 개수를 반환하라  
소수 조건은 아래와 같다

- `0P0`처럼 소수 양쪽에 `0`이 있는 경우
- `P0`처럼 소수 오른쪽에만 `0`이 있고 왼쪽에는 아무것도 없는 경우
- `0P`처럼 소수 왼쪽에만 `0`이 있고 오른쪽에는 아무것도 없는 경우
- `P`처럼 소수 양쪽에 아무것도 없는 경우

단, `P`는 각 자릿수에 `0`을 포함하지 않는 소수입니다.  
 예를 들어, `101`은 `P`가 될 수 없습니다.

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
```

## Solving Strategies

1. `n`을 `k`진수로 변환하기
2. 조건에 부합하는 숫자를 찾기
3. 해당 숫자가 소수인지 확인
4. 해당 숫자 안에 0이 들어가는지 확인

### solve 1

조건에 부합하기 위해, 정규표현식 활용

- 처음에서 숫자로 시작하면서 `0`이 나오는 경우
- `0` 사이에 숫자가 있는 경우
- `0`으로 시작하면서 숫자로 마무리 될 때
- `0`이 없는 숫자만 존재하는 경우

**Fail**
정규 표현식을 활용하여 해결하려고 노력했으나 해결되지 않았다  
특히 `203410235023`와 같은 경우 `03410`은 `match` 함수로 반환되나  
`02350`은 판별할 수 없었다  
차이점이 무엇인지 궁금하다

### solve 1 Code

```js
function isPrime(number) {
  if (number < 2) {
    return false;
  }

  for (let i = 2; i <= Math.sqrt(number); i++) {
    if (number % i === 0) {
      return false;
    }
  }
  return true;
}

function hasZero(number) {
  return /0/.test(number);
}

function solution(n, k) {
  const regs = [/^[1-9]+0{1,}/, /0{1,}[1-9]+$/, /^[1-9]+$/];
  const midReg = /0{1,}[1-9]+0{1,}/g;
  const knaryNumber = n.toString(k);

  const count = regs.reduce((acc, reg) => {
    const matchResults = knaryNumber.match(reg) ?? '0';
    const matchedNumber =
      matchResults[0].replace(/^0+|0+$/g, '') || matchResults;

    return !hasZero(matchedNumber) && isPrime(matchedNumber) ? acc + 1 : acc;
  }, 0);
  const matchResults = knaryNumber.match(midReg) ?? '0';

  return [...matchResults].reduce((count, result) => {
    const number = result.replace(/^0+|0+$/g, '') || result;

    if (!hasZero(number) && isPrime(number)) {
      return acc + 1;
    }
    return acc;
  }, count);
}
```

## Refactoring

`0`으로 `split`  
이후 남은 숫자의 소수 여부 판단 => `count++`

### solve 2

1. `n`을 `k`진수로 변환
2. `0`으로 `split`
3. `reduce` 활용 => 숫자들의 소수 개수를 파악하여 `count++`

### solve 2 Code

```js
function solution(n, k) {
  return n
    .toString(k)
    .split('0')
    .reduce((count, number) => (isPrime(number) ? count + 1 : count), 0);
}
```

### solve 3

`filter`를 활용하여 가독성 향상 리팩토링

### solve 3 Code

```js
function solution(n, k) {
  return n.toString(k).split('0').filter(isPrime).length;
}
```

## 배운 점 or 주의할 점

너무나도 아쉽고 너무나도 아쉽다  
`split`을 활용한 해결 방법을 생각 못한 것은 아니다  
하지만 이게 최고의 방법이었다

문제의 조건에 완전히 낚여버렸다  
특히 마지막 조건인  
**단, `P`는 각 자릿수에 `0`을 포함하지 않는 소수입니다.**  
**예를 들어, `101`은 `P`가 될 수 없습니다.**  
이것을 본 이후 무조건 정규 표현식으로만 해결해야 한다고 생각했다  
하지만 도리어 이 조건은 `split`을 사용할 수 있도록 하는 조건이라고 생각한다  
`0`이 들어가는 모든 경우는 조건의 부합하는 소수가 될 수 없기 때문이다  
잘 풀 수 있었고 잘 해결할 수 있었던 문제인데  
문제 해석에 대한 부분이 많이 아쉽게 여겨졌다  
문제 잘 읽고, 잘 해석하자

그런데 왜 `203410235023`의 `03410`, `02350`은 판별할 수 없었을까?
