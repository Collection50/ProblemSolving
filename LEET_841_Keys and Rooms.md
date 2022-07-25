# LeetCode/841/Keys and Rooms

## Want

숫자 배열(`rooms`)이 주어진다  
`rooms는` `n`개의 요소가 존재하며 각 방의 번호는 `0 ~ n - 1`이다  
0번 방을 제외한 다른 문은 열쇠가 있어야 들어갈 수 있으며  
각 방에는 다른 방으로 향하는 열쇠가 존재하거나 존재하지 않는다  
**모든 방을 들어갈 수 있는지 여부**를 불리언 값으로 반환하라

## INPUT && OUTPUT

INPUT: ARRAY  
OUTPUT: BOOLEAN

## First Understanding & Seperating

쉽게 설명하자면 **그래프** 문제인데  
각 정점의 간선이 주어지며 **모든 그래프가 연결되어 있는지 여부**를 확인하면 되는 문제다  
근데 **꼭 그렇게 풀지 않아도** 될 수 있다고 생각했다  
*'모든 방을 들어갈 수 있다'*는 *'모든 방의 열쇠를 갖고 있다'*로 이어지기에  
순회를 통해 얻은 열쇠 개수와 방의 개수의 비교 결과를 반환해주었다

### solve 1

1. `keySet`을 활용하여 0번 방에서 얻은 열쇠 저장
2. 1번에서 얻은 열쇠를 통해 방에 입장하여, 발견한 열쇠를 `keySet`에 저장
3. 새로운 열쇠가 있다면 그 열쇠를 통해 다른 방 들어가기 반복
4. 모든 방 순회 후 0번 방의 열쇠도 존재하는 것이므로 `keySet`에 저장
5. 이후 열쇠 개수와 방의 개수를 비교한 불리언 값 `return`

### solve 1 Code

```js
// Runtime: 73 ms, faster than 85.75%
// Memory Usage: 43.8 MB, smaller than 92.76%
const canVisitAllRooms = function (rooms) {
  const ZERO = 0;
  const keySet = new Set(rooms[ZERO]);

  for (const keys of keySet) {
    rooms[keys].forEach((key) => {
      keySet.add(key);
    });
  }
  keySet.add(ZERO);
  return keySet.size === rooms.length;
};
```

## Refactoring

### solve 2

(_엇? `Set`, `Array.map()` 활용하면 재밌게 풀 수 있을 듯한 걸?_)  
이게 문제를 이해한 후 처음 들었던 생각이다

`solve 1`과 풀이 방법만 상이할 뿐 **로직은 동일**하다

어쩌면 `saveKey` 함수는 불필요할 지 모른다  
하지만 `Array.map` 함수의 새로운 사용법을 알고 난 이후  
꼭 한 번 이렇게 사용해보고 싶었다 ~~(헤헤)~~  
어쩌면.. 함수형 프로그래밍.. 얼마 안 남았을 지도..?

### solve 2 Code

```js
// Runtime: 109 ms, faster than 30.61%
// Memory Usage: 44.1 MB, smaller than 75.70%
const canVisitAllRooms = function (rooms) {
  const ZERO = 0;
  const keySet = new Set(rooms[ZERO]);

  const saveKey = function (key) {
    keySet.add(key);
  };

  for (const keys of keySet) {
    rooms[keys].map(saveKey);
  }

  keySet.add(ZERO);
  return keySet.size === rooms.length;
};
```

## 배운 점 or 주의할 점

내 풀이 방법이 너무 마음에 들어서 Leetcode Discuss에 내 풀이를 공유했다  
[여기](https://leetcode.com/problems/keys-and-rooms/discuss/2327127/JavaScript-Easiest-Solve)서 확인 가능

프로그래밍의 좋은 점은 정답이 **Only One**이 아니라는 점이다  
정답을 향한 다양한 접근 방법이 있으며 사용 **환경과 특수성에 따라 좋은 코드의 방향이 바뀔 수 있다**  
그래서 내가 작성한 코드를 하나하나 **설명할 줄 알아야** 하며 **타당한 이유가 존재해야** 한다
