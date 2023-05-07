## Use Maps More and Objects Less
https://www.builder.io/blog/maps

- Object에 비해 Map은 Read/Write 성능이 뛰어남
- 목적이 분명한 자료구조
- Copy도 간편하게 할 수 있음 `const copiedMap = new Map(map)`
  - constructor parameter로 iterable tuple 받을 수 있음
- key의 데이터 타입 제한이 없음. Object, Function 등 가리지 않음
  - 문제: key로 사용한 object가 사라지면 Memory leak 이슈 발생. Why? key가 reference type이기 때문에 GC가 이를 처리할 수 없음 -> How to solve? `WeakMap` 사용하면 GC가 처리함 (!`WeakMap` 다시보기)
- Map Use case: metadata - 가져온 DB data object를 오염시키지 않고 temp로 data binding 가능
- Map의 사촌인 Set도 Array에 비해 성능 우수. Unique 값을 가짐 -> Memory leak? -> `WeakSet` (!Use case 찾아보기)
- iteration도 훨씬 깔끔. literal obj는 built-in method를 수반하기 때문에 key validation에 hasOwnProperty를 동반해야함. Map은 그럴 필요 없이 `for...of` 하나로 간단히 처리 가능
- Map은 key ordering 보장 (!내부적으로 LRU cache / Linked list와 연관되어있는듯? -> [Least Recently Used Cache 가장 사용되지 않은 캐시를 삭제하고 새로운 캐시를 추가하는 알고리즘 기법](https://www.interviewcake.com/concept/java/lru-cache), Double Linked list로 구현, 높은 cache hit rates -> hit rates? 얼마나 캐시를 성공적으로 회수했는가? <-> cache misses)
- `[[firstKey, firstValue], [secondKey, secondValue]] = map` 순서보장 및 destructuring 가능
- Map to Obj / Obj to Map -> `Object.fromEntries(map)` / `new Map(Object.entries(obj))`
- Obj, Array는 build JSON method로 (de)serialization이 용이 -> 하지만, Map, Set도 JSON method의 두번째 인자(replacer, reviver)를 통해 변환 가능하다 (@추가 구현이 필요하나 성능 우위가 되어야하는 상황에서는 괜찮은 trade-off로 생각)
- Map Use case: read/write가 빈번하면 Map, 구조화된/고정된 key-value set이라면 Obj -> Why? Obj는 write/delete cost가 높기 때문
- Set Use case: 복제 없고 순서가 중요하지 않을 경우, 중복 값을 허용하지 않는 경우
- ---
- @Obj의 편의성과 Map의 성능 및 기타 이점을 저울질하여 상황에 맞게 적용하면 된다. 
- @퍼포먼스 밴치마크는 실제 환경과 같을 수 없다. 성능과 관련한 이유로 도입하고자할 때 퍼포먼스 검증을 해야한다.
