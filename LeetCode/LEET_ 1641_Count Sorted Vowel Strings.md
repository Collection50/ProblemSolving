# LeetCode/1641/Count Sorted Vowel Strings

#DP#Knapsack

## Want

정수 `n`이 주어진다  
자음 `a`, `e`, `i`, `o`, `u`으로 만들 수 있는  
`n`개 길이 문자열의 개수를 반환하라  
이때 문자열은 사전순으로 정렬되어있어야 한다

## INPUT && OUTPUT

```js
/**
 * @param {number} n
 * @return {number}
 */
```

## Solving Strategies

`DP`를 활용한 문제  
전형적인 `Knapsack` 문제  
점화식은 `dp[i][k] = dp[i][k - 1] + dp[i - 1][k] (i > 1)`이다  
그림을 보면 조금 더 쉽게 이해할 수 있다  
![스크린샷 2023-04-09 오후 9 22 22](https://user-images.githubusercontent.com/86355699/230772252-af4a198c-10cc-4c3d-bf32-ddbff10e9ffe.png)

`Knapsack`의 대표적인 문제로  
문자열에서 `a`가 존재할 때와 `a`가 존재하지 않을 때의 개수를 더해주는 로직이다

`Bottom-Up` 방식

### solve 1

`Solving Strategies`의 코드화

### solve 1 Code

```js
function countVowelStrings(n) {
  const dp = Array.from({ length: n + 1 }).map(() => Array.from({ length: 6 }, () => 0));

  for (let i = 1; i <= n; ++i) {
    for (let k = 1; k <= 5; ++k) {
      dp[i][k] = dp[i][k - 1] + (i > 1 ? dp[i - 1][k] : 1);
    }
  }
  return dp[n][5];
}
```

## 배운 점 or 주의할 점

비슷한 규칙을 찾아냈었는데  
점화식을 세워 구체화하지 못하였다  
내가 이때까지 접한 `DP`와는 다른 접근 방법이어서  
명확한 해결책도 잘 떠오르지 않았다

몰라서 틀린 것은 괜찮다  
하지만 연속되는 것은 괜찮지 않다
