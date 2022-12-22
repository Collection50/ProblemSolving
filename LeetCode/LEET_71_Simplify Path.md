# LeetCode/71/Simplify Path

#스택

## Want

디렉토리의 절대 주소가 주어진다  
`..`, `.`, `//` 등 절대 주소에 사용될 수 없는 것들을 제외하여 올바른 주소를 반환하라  
[문제 링크](https://leetcode.com/problems/simplify-path/description/)

## INPUT && OUTPUT

```js
/**
 * @param {string} path
 * @return {string}
 */
```

## Solving Strategies

스택 활용 문제  
`/a/../c`와 같은 경우의 최종 출력은 `/c`였다  
따라서 상위 디렉토리를 가리키는 `/../`의 경우 이전 디렉토리를 제외해야 한다  
올바른 형식에 대한 값을 제외한 이후 디렉토리 경로를 찾아간다ㄴ

### solve 1

실패  
스택을 활용한 것이 아니라 문자열 문제로 간주하였다  
따라서 올바른 해결 방법이 아니었을 뿐더러 효율적으로 보이진 않는다

### solve 1 Code

```js
const simplifyPath = function (path) {
  return path
    .replace(/\/{2}/, '/')
    .replace(/\.{1,2}/, '')
    .replace(/\/$/, '')
    .replace(/^$/, '/');
};
```

## Refactoring

### solve 2

스택을 활용하여 문제 해결

1. `/../`일 경우 상위 디렉토리 삭제
2. 현재 디렉토리를 가리키는 `.`은 무시된다
3. 따라서 `.`을 제외한 값들에 한하여 `pathStack`에 `push`

### solve 2 Code

```js
const simplifyPath = function (path) {
  const pathStack = [];
  const splitedPath = path.split('/').filter((directory) => directory);

  splitedPath.forEach((directory) => {
    if (directory === '..') {
      pathStack.pop();
    } else if (directory !== '.') {
      pathStack.push(directory);
    }
  });
  return `/${pathStack.join('/')}`;
};
```

### solve 3

`reduce`를 활용하여 가독성과 성능 향상  
더하여 `filter`의 조건을 하나 더 부여하여 가독성을 향상시켰다

### solve 3 Code

```js
const simplifyPath = function (path) {
  const splitedPath = path
    .split('/')
    .filter((directory) => directory && directory !== '.')
    .reduce((result, directory) => {
      if (directory === '..') {
        return result.slice(0, result.length - 1);
      }
      return [...result, directory];
    }, []);
  return `/${splitedPath.join('/')}`;
};
```

## 배운 점 or 주의할 점

끝날 때까진 정말 끝난 게 아니다
