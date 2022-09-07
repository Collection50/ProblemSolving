# LeetCode/997/Find the Town Judge

#그래프#배열

## Want

마을에 `1 ~ n` 숫자로 구별되는 `n`명의 사람이 존재한다  
그 중 한 명은 비밀스러운 `judge`이며 `judge` 조건은 아래와 같다

1. `judge`는 아무도 믿지 않는다
2. 모든 사람은 `judge`를 믿는다
3. 1,2를 만족하는 것은 단 한 사람이다

주어지는 배열 `trusts[i]`는
`trust[i] = [a, b]`로 표현되며, 이는 `a`가 `b`를 믿는 것이다
`judge`로 구별되는 숫자(사람)을 반환하라
만약 `judge`가 존재하지 않는다면 `-1` 반환

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @param {number[][]} trust
 * @return {number}
 */
```

## Solving Strategies

1. 누군가를 신뢰하는 사람의 수가 `n - 1`이면 그 사람은 `judge`일 가능성이 존재
2. 그 사람이 누군가를 신뢰하고 있다면 `judge` 없음
3. 아무도 신뢰하지 않고 있다면 `judge`임

### solve 1

1. 각 배열을 순회하며 `trustee`를 키로, `truster`를 값으로 분류하여 `trustObj`에 저장 (set 객체 사용)
2. 누군가를 신뢰하는 사람이 `n - 1`명인 사람 찾기 (`judge` 찾기)
3. `trustObj`의 키 중에 `judge`가 존재한다면, 2번 조건을 만족하지 않으므로 `FAIL` 할당
4. `judge` 값이 `FAIL`이라면 `judge`가 없는 것이므로 -1 반환
   아니라면 `judge`를 찾은 것이므로 구별된 번호 반환

그래프 활용하여 해결  
**시간 복잡도 O(N)**

### solve 1 Code

```js
// Runtime: 187 ms, faster than 35.96%
// Memory Usage: 51.6 MB, smaller than 27.93%
const findJudge = function (n, trust) {
  if (n === 1) return n;

  const trustObj = {};
  const FAIL = -1;
  let judge = -1;

  trust.forEach(([truster, trustee]) => {
    trustObj[trustee] = trustObj[trustee] || new Set();
    trustObj[trustee].add(truster);
  });

  Object.keys(trustObj).forEach((key) => {
    if (trustObj[key].size === n - 1) {
      judge = +key;
    }
  });

  Object.values(trustObj).forEach((value) => {
    if (value.has(judge)) {
      judge = FAIL;
    }
  });

  return judge === FAIL ? FAIL : judge;
};
```

### solve 2

**그래프**를 활용해 해결하긴 했지만, 나보다 더 좋은 코드를 작성한 사람이 있을 거라고 생각했다  
확인해보니 배열을 사용한다면 더 직관적으로 문제를 해결함을 확인할 수 있다

아래의 로직을 설명하자면 다음과 같다

1. `n + 1`개 요소를 가진 배열 0으로 초기화
2. `trust` 배열의 각 요소에 대해, `trustee`가 존재한다면 해당 값 + 1
3. `truster`가 존재한다면 해당 값 - 1
4. 마지막으로 각 `candidates`의 값이 `n - 1`과 같다면 (누구도 믿지 않고 신뢰만 당한다면) `judge`
5. `return`

### solve 2 Code

```js
// Runtime: 148 ms, faster than 66.51%
// Memory Usage: 50.6 MB, smaller than 64.51%
const findJudge = function (n, trust) {
  const candidates = new Array(n + 1).fill(0);

  for (const [truster, trustee] of trust) {
    candidates[truster]--;
    candidates[trustee]++;
  }

  for (let i = 1; i < candidates.length; i++) {
    if (n - 1 === candidates[i]) {
      return i;
    }
  }
  return -1;
};
```

## 배운 점 or 주의할 점

각 풀이만의 장점이 존재한다  
단순히 빠른 속도를 위한다면 `solve 2`가 더 직관적이고 빠를 것이고  
누가 누구를 신뢰하는지, 누가 `judge`인지 각 요소를 확인해야 한다면 `solve 1`이 더 나은 코드일 것이다  
따라서 **다양한 방법으로 로직을 작성**하는 것이 중요할 것 같다  
**상황**에 따라 좋은 코드도 바뀐다
