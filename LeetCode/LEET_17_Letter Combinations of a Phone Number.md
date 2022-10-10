# LeetCode/17/Letter Combinations of a Phone Number

#재귀

## Want

2-9 사이의 숫자로 이뤄진 문자열이 주어진다  
전화번호 버튼을 토대로, 숫자를 만들 수 있는 모든 문자 조합을 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {string} digits
 * @return {string[]}
 */
```

## Solving Strategies

순열을 만들라는 건데, 일단 2-9 각 숫자와 문자를 **키-값 형태**로 저장

1. 키-값 저장할 객체 생성
2. 재귀 함수 생성 (인수: 배열, 순서)
3. 알파벳 개수만큼 반복하는 for문 생성
4. 순서에 해당하는 키, for문에 해당하는 값을 배열에 넣고 다시 재귀함수 호출
5. 만약 `순서 === digits.length` 라면 `result`에 입력
6. `result` 반환

### solve 1

쉽게 설명하자면, 주어진 번호의 알파벳들을 하나씩 모두 사용해 순열을 만들면 된다  
숫자-알파벳 조합을 활용하기 위해 `telephone` 객체를 생성해 활용하였고  
순열을 생성하는 재귀 함수를 사용했다  
만들어진 값을 담기 위해 `current` 배열을 활용하였고  
`digits`의 각 숫자를 가리키기 위해 `order`를 사용하였다

### solve 1 Code

```js
// Runtime: 59 ms, faster than  96.49%
// Memory Usage: 42.5 MB, smaller than 7.49%
const letterCombinations = function (digits) {
  const result = [];
  if (digits.length === 0) return result;

  const telephone = {
    2: ['a', 'b', 'c'],
    3: ['d', 'e', 'f'],
    4: ['g', 'h', 'i'],
    5: ['j', 'k', 'l'],
    6: ['m', 'n', 'o'],
    7: ['p', 'q', 'r', 's'],
    8: ['t', 'u', 'v'],
    9: ['w', 'x', 'y', 'z'],
  };

  function combie(current, order) {
    if (order === digits.length) {
      result.push(current.join(''));
      return;
    }

    const digit = digits[order];
    const alphas = telephone[digit];

    for (let i = 0; i < alphas.length; i++) {
      combie([...current, alphas[i]], order + 1);
    }
  }

  combie([], 0);
  return result;
};
```

## Refactoring

### solve 2

조금 더 **선언적**인 코드로 리팩토링 할 수 있다고 생각했다  
따라서 **가독성 + 효율성** 모두 잡은 코드로 리팩토링 해보았다  
**배열 => 문자열화**  
**`for...of` 사용**  
**문자열 연결 연산자 사용**

### solve 2 Code

```js
// Runtime: 71 ms, faster than 79.25%
// Memory Usage: 41.8 MB, smaller than 85.71%
const letterCombinations = function (digits) {
  const result = [];
  if (!digits.length) return result;

  const telephone = {
    2: 'abc',
    3: 'def',
    4: 'ghi',
    5: 'jkl',
    6: 'mno',
    7: 'pqrs',
    8: 'tuv',
    9: 'wxyz',
  };

  function combie(current, order) {
    if (order === digits.length) {
      result.push(current);
      return;
    }

    const alphas = telephone[digits[order]];

    for (const alpha of alphas) {
      combie(current + alpha, order + 1);
    }
  }

  combie('', 0);
  return result;
};
```

## 배운 점 or 주의할 점

내 생각을 코드로.  
진짜는 행동으로 증명한다  
가수는 노래로 증명한다  
연기자는 연기로 증명한다  
개발자는 코드로 증명한다
