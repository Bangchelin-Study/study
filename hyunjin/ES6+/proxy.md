# Proxy

- proxy는 기본 오퍼레이션을 중간에서 가로채어 대리, 대신하여 실행합니다.
- 가로채어 실행하더라도 전체 궤도를 벗어날 수 없으므로 오퍼레이션을 완전하게 바꿀수는 없습니다.
- ES6에 [[GET]]처럼 기본 오퍼레이션을 제공하는 13개의 메서드가 있습니다.

```js
const target = { order: "커피" }
const value = target.order

console.log(value)
```

1. getter 스펙에서 [[GET]]으로 표기
2. const value = target.order; 에서 target.order를 호출하면 target오브젝트의 **proto**에 있는 [[GET]]을 호출합니다.
3. [[GET]]을 호출하면서 파라미터 값으로 "order"를 넘겨줍니다.
4. [[GET]] 메서드가 order를 프로퍼티 키로 하여 프로퍼티 값을 구해 반환합니다.

## 논리

```js
const target = { food: "밥" }
const middle = new Proxy(target, {})
const left = middle.food
```

가운데에 Proxy를 두는 이유가 뭔가요??

- target 은 프록시 대상 object 입니다.
- Array, Object 등을 사용할 수 있습니다.
- `const obj = new Proxy(target, {}) 형태에서 첫번째 파라미터에 target을 작성합니다. 이렇게 Proxy인스턴스를 생성하므로 Proxy인스턴스와 target이 연결됩니다.

trap은 os 에서 사용하는 용어로 실행중인 프로그램에 이상이 발생했을 때 실행을 중단하고 사전에 정의된 제어로 전환하는 것을 말합니다.

```js
const target = { food: "밥" }
const handler = {
  get(target, key) {
    return target[key] + ".수저"
  },
  set(target, key) {},
}
const middle = new Proxy(target, handler)
const left = middle.food
console.log(left)
```
