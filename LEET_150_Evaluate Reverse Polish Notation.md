# LeetCode/150/Evaluate Reverse Polish Notation

#Stack

## Want

후위 표기법의 각 요소가 문자로 된 배열이 주어진다  
후위 표기법을 활용한 연산 결과를 반환하라  
[_후위 표기법이란?_](https://en.wikipedia.org/wiki/Reverse_Polish_notation)

## INPUT && OUTPUT

INPUT: ARRAY
OUTPUT: NUMBER

## First Understanding & Seperating

1. 숫자, 연산자 `stack` 선언 (`numStack`, `operStack`)
2. 배열의 각 값을 분류하여 각 `stack`에 저장
3. 숫자 `stack`에 2개 이상, 연산자 `stack`에 1개 이상 존재한다면
   숫자 `stack`의 2개 `pop`, 연산자 `stack`의 1개 `pop`하여 연산
4. 3번의 결과를 다시 숫자 `stack`에 `push`
5. `tokens`의 요소를 모두 읽을 때까지 3, 4번 반복
6. 결과 `return`

### solve 1

문제가 원하는 핵심은 **두 가지**다

1. **후위 표기법을 제대로 이해**하여 올바른 결과값을 도출할 수 있는가?

- 연산자와 숫자가 모두 **문자**로 주어진다
- 따라서 각 문자열을 숫자와 연산자로 구분해야 했는데 이 부분에서 깔끔한 코드를 작성하고 싶어 고민을 했다
- 결론적으로 `for...of`문에서 `+` 연산자를 사용하여 `number`로 타입 변환을 수행해주었다
- 이후 `isNan`함수를 사용하여 숫자와 연산자를 구분하여 각 배열에 `push`
- 만약 숫자 배열에 2개 이상, 연산자 배열에 1개 이상 존재하는 경우, `calculator` 함수를 통해 연산자, 숫자들을 조합한 결과를 생성하였다

2. 음수를 나눌 때 **문제의 조건을 만족**할 수 있는가?

- _Note that division between two integers should truncate toward zero._
- 쉽게 말하자면 **나눗셈을 할 때 0을 향하여 소숫점이 정리**되어야 한다는 의미다
- 이 부분에서 조금 시간을 할애했었는데, 문제가 말하는 정확한 의미가 헷갈려 이것저것 시도해보았었다
  ```js
  if (operator === '/') {
    return Math.abs(num1 / num2) <= 0.9 ? 0 : Math.floor(num1 / num2);
  }
  ```
  ```js
  if (operator === '/') {
    Math.floor(num1 / num2) >= 0 ? Math.floor(num1 / num2) : 0;
  }
  ```
- 정확하게는 **나눗셈 결과가 양수라면 내림, 음수라면 올림을 사용**하여 소숫점 결과가 존재하는 경우 0과 가깝게 만들라는 의미였다
- 이는 양수일 때 `Math.floor`, 음수일 때 `Math.cel`을 활용하여 해결할 수 있었다

### solve 1 Code

```js
// Runtime: 123 ms, faster than 32.64%
// Memory Usage: 44.2 MB, smaller than 91.55%
const calculator = (num1, num2, operator) => {
  if (operator === '+') return num1 + num2;
  if (operator === '-') return num1 - num2;
  if (operator === '*') return num1 * num2;
  if (operator === '/') {
    return num1 / num2 >= 0 ? Math.floor(num1 / num2) : Math.ceil(num1 / num2);
  }
};

const evalRPN = function (tokens) {
  const numStack = [];
  const operStack = [];

  for (const elem of tokens) {
    if (Number.isNaN(+elem)) operStack.push(elem);
    else numStack.push(+elem);

    while (numStack.length >= 2 && operStack.length >= 1) {
      const secondNum = numStack.pop();
      const firstNum = numStack.pop();
      const operator = operStack.pop();

      numStack.push(calculator(firstNum, secondNum, operator));
    }
  }
  return numStack.pop();
};
```

## Refactoring

### solve 2

`JavaScript-Like`  
객체를 활용하여 조금 더 자바스크립트스러운 코드로 리팩토링 하였다  
기본적인 로직은 `solve 1`과 동일하고 `operators` 객체를 활용하여  
결과값 계산에 더해 연산자와 숫자 구분도 가능케 하였다

### solve 2 Code

```js
// Runtime: 112 ms, faster than 44.16%
// Memory Usage: 45.7 MB, smaller than 32.49%
const evalRPN = function (tokens) {
  const operators = {
    '+': (a, b) => a + b,
    '-': (a, b) => a - b,
    '*': (a, b) => a * b,
    '/': (a, b) => (a / b >= 0 ? Math.floor(a / b) : Math.ceil(a / b)),
  };
  const numStack = [];
  const operStack = [];

  for (const elem of tokens) {
    if (operators[elem]) operStack.push(elem);
    else numStack.push(+elem);

    while (numStack.length >= 2 && operStack.length >= 1) {
      const secondNum = numStack.pop();
      const firstNum = numStack.pop();
      const operator = operStack.pop();

      numStack.push(operators[operator](firstNum, secondNum));
    }
  }
  return numStack.pop();
};
```

## 배운 점 or 주의할 점

아직 내 머리속에서 `JavaScript-Like`한 코드를 무의식적으로 밀어내려고 하는 듯한 느낌이다  
C언어로 알고리즘을 꽤 풀었던 경험이 존재해서인지, 이렇게 결과값을 저장해야된다는 생각이 뿌리깊게 박혀있는 듯한 느낌.  
계속계속 이러한 생각의 고착화를 한꺼풀 한꺼풀 벗겨내면서 `JS`적으로 생각해야겠다고 여겨진다  
`solve 2`의 `operators`를 보면 굉장히 예뻐보임ㅎ.ㅎ
