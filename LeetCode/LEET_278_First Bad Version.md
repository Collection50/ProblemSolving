# LeetCode/278/First Bad Version

#BinarySearch

## Want

버전의 개수를 뜻하는 정수 `n`이 주어진다 (1 ~ `n`까지의 버전 존재)  
각 버전은 이전 숫자의 버전을 참조해 만들어지기 때문에 어떤 버전이 bad라면 이후의 버전도 모두 bad가 된다  
처음 bad 버전이 시작되는 숫자를 반환하라  
`isBadVersion` 함수를 활용하여 해결하라  
예를 들어 3부터 bad라면 3 ~ `n`까지 모두 bad이다

## INPUT && OUTPUT

```js
/**
 * Definition for isBadVersion()
 * @param {integer} version number
 * @return {boolean} whether the version is bad

 * Definition for solution()
 * @param {function} isBadVersion()
 * @return {function}

 * Definition for inner function
 * @param {integer} n Total versions 
 * @return {integer} The first bad version
 */
```

## Solving Strategies

범위는 **2가지**로 좁혀지게 된다 (이진 탐색 활용)  
따라서 `mid`를 기준으로  
왼쪽에서 `badVersion` 시작, `mid`도 bad  
오른쪽에서 `badVersion` 시작, `mid`는 bad 아님

1. `mid`가 bad라면 `end` = `mid` 할당
2. `mid`가 bad가 아니라면 `start = mid + 1` 할당
3. `return`

### solve 1

핵심 로직: 보통의 이진 탐색처럼 `end = mid - 1`을 해주면 안 되고 `mid`까지 포함하여여야 한다  
`mid`번째부터 시작하는 경우엔 찾을 수 없는 경우가 존재하기 때문이다

하지만 이러한 변화에 따라 조심해야 할 부분이 존재했다  
`while`문의 조건을 `start <= end`로 설정한다면 bad의 시작 지점을 찾더라도 무한 루프에 빠지게 된다  
이를 예방하기 위해 조건을 `start < end`로 설정한 후 `end`를 반환하도록 하였다

`mid`가 아닌 `end`를 반환하는 이유:
`mid`부터 bad가 시작될 경우, `while`문을 탈출할 때 `mid`는 bad의 시작 지점이 아닌 (시작지점 -1)을 가리키게 된다  
따라서 `start`, `end` 둘 중 하나를 선택해야 했고, 나는 `end`를 선택하였다

### solve 1 Code

```js
const solution = function (isBadVersion) {
  return function (n) {
    let start = 1;
    let end = n;
    let mid;

    while (start < end) {
      mid = Math.floor((start + end) / 2);

      if (isBadVersion(mid)) end = mid;
      else start = mid + 1;
    }
    return end;
  };
};
```

## 배운 점 or 주의할 점

이진 탐색 문제를 풀고 있는 요즘  
생각보다 이진 탐색을 활용한 다양한 문제가 존재한다는 것을 체감하고 있다  
코딩 테스트에 빈번하게 출제되는 카테고리는 아니지만,  
**알고리즘 === 자신의 생각을 코드로 표현할 수 있는 연습**이라고 생각하기 때문에  
알고리즘은 **언제나 중요하고 훌륭한 연습**이다
