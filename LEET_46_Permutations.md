# LeetCode/46/Permutations

#재귀#순열

## Want

정수 배열이 주어진다  
가능한 모든 순열을 만들어 배열 형태로 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
```

## Solving Strategies

`nPr`인데, `n === r`인 경우라고 보면 되겠지  
재귀를 활용하여 해결하면 괜찮을 것 같네

1. 1개 값을 고정시킨 후 n - 1개에서 r - 1개를 고른다
2. 다시 1개를 고정시킨 후 n - 2개에서 r - 2개를 뽑는다
3. 고정된 값의 길이와 본 배열의 길이가 같을 경우 `result`에 `push`
4. 재귀 스택이 종료되었다면 고정 해제

### solve 1

순열에서 주의할 점은, **순서가 중요**하다는 점이다  
`[1, 3, 2]`, `[1, 2, 3]`을 **다른 것**으로 보기 떄문이다  
위 예시처럼, 맨 앞 값이 1이더라도 뒤에 나올 숫자들의 순서를 바꿔야 할 필요가 존재한다  
해시인 `Set`을 활용하여 원하는 숫자를 **고정**할 수 있도록 하였고 (`fixed`)  
만약 고정된 값이 아니라면 **값을 해시에 추가하며 재귀 함수를 호출**하도록 하였다  
마지막으로 재귀 스택이 종료되었다면 **해시에서 값을 삭제**하여, 순열의 중요점인 순서를 바꿔 조합될 수 있도록 하였다

### solve 1 Code

```js
// Runtime: 111 ms, faster than 51.20%
// Memory Usage: 45 MB, smaller than 61.39%
const permute = function (nums) {
  const result = [];

  function getPermute(fixed) {
    if (fixed.size === nums.length) {
      result.push([...fixed.values()]);
      return;
    }

    for (let i = 0; i < nums.length; i++) {
      const value = nums[i];

      if (fixed.has(value)) continue;

      getPermute(fixed.add(value));
      fixed.delete(value);
    }
  }

  getPermute(new Set());
  return result;
};
```

## 배운 점 or 주의할 점

머리속에 있는 생각을 코드로 잘 그려낸다면 쉽게 해결할 수 있는 문제라고 생각한다  
그러니까 중요한 점은, **자신의 생각과 로직을 코드로 구현하는 것이 중요**한 것이라는 점이다  
흔히들 개발자를 보며 컴퓨터 한 대만 있으면 세상을 창조할 수 있는 직업이라고들 하는데  
그게 가능해지려면, 자신의 생각을 코드로 나타낼 줄 알아야 한다  
어쩌면 개발자에게 있어 **가장 중요한 능력**이지 않을까 싶다  
재귀 함수를 머리속에서 그려보기...
