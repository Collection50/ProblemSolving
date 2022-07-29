# LeetCode/394/Decode String

#STRING

## Want

숫자와 괄호, 소문자 영어로 이뤄진 암호가 문자열로 주어진다  
이를 해독한 문자열을 반환하라

## INPUT && OUTPUT

INPUT: STRING  
OUTPUT: STRING

'3[a]2[bc]' => 'aaabcbc'  
'3[a2[c]]' => 'accaccacc'

## First Understanding & Seperating

1. 문자열의 패턴(특징)을 선언
2. 패턴에 만족하는 경우가 존재한다면 문자열 구분 및 분리
3. 몇 번 반복하는지 (`repeatNumber`), 무엇을 반복하는지 (`returnString`) 확인
4. 일치하는 문자열을 반복 횟수만큼 반복한 후, 1번의 패턴과 일치하는 문자열과 교환 (`replace`)
5. 결과값 `return`

### solve 1

문자열 함수를 사용하면 생각보다 재밌는 문제였다  
패턴은 단 한가지만 존재하므로 변수화하여 사용했다  
`replace` 함수의 콜백 함수를 활용하여 콜백 함수의 반환값이 패턴과 일치하는 문자열과 교환되도록 하였다  
더하여 원본 배열을 변경하지 않도록 새로운 변수를 선언하여 해결했다 (`decodedString`)

### solve 1 Code

```js
// Runtime: 82 ms, 56.72%
// Memory Usage: 42.1 MB, samller than 34.78%
const decodeString = function (string) {
  const ZERO = 0;
  let decodedString = string.slice();
  const decryptKey = /\d+\[[a-z]+\]/;

  while (decodedString.match(decryptKey)) {
    decodedString = decodedString.replace(decryptKey, (matchedString) => {
      const repeatNumber = matchedString.match(/[0-9]+/)[ZERO];
      return matchedString.match(/[a-z]+/)[ZERO].repeat(repeatNumber);
    });
  }
  return decodedString;
};
```

## 배운 점 or 주의할 점

문제를 통해 많이 접하지 못했던 `replace`, `match`, `repeat` 등의 함수를 사용하며  
개념에 대한 응용을 할 수 있어 재밌었다  
`replace`의 콜백함수 특징: 첫 번째로 전달한 인수(패턴)와 일치하는 문자열을 **콜백 함수의 반환 값과 교환**
`match`: 정규 표현식과 일치하는 문자를 **배열 형태**로 반환
