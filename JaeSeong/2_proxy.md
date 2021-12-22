# 1. Proxy, 기본 오퍼레이션 논리
Proxy는 기본 오퍼레이션을 중간에 가로채어 대리실행.  
ex)  
```js
const counter = {order: "coffee"};
const o = counter.order;
```
여기서 counter.order는 getter의 역할을 한다.  
이때, 엔진이 order를 반환하기 위해 내부 메소드 __proto__에 있는 [[Get]]을 호출한다.  
이것은 기본 오퍼레이션이다.

ES6에는 이러한 기본 오퍼레이션 13개가 있음!


# 2. Proxy 논리, 형태
Proxy는 일종의 adapter 역할을 한다.  
ex)
```js
const tg = {food: "rice"};
const proxy = new Proxy(tg, {});
const get = test_proxy.food;
```
위 예제에서 Proxy의 getter가 호출되면 Proxy에서 tg의 getter를 호출.  
Proxy는 가운데에서 대리자 역할을 한다.


# 3. target, trap, handler
target : Proxy의 대상 오브젝트.  
Array, Object 등이 그 대상이 될 수 있다.  
```js
const obj = new Proxy(target, {});
```
위와 같은 형태에서 Proxy의 첫 인자에 target을 작성.

trap : OS에서 실행하는 비동기 Interupt. 정해진 작업을 수행한다.  
Proxy는 요청되는 작업에 해당하는 trap을 처리한다.  
handler : trap 발생 시 수행하는 작업을 정의한다.
ex)
```js
const target = {food: "rice"};
const handler = {
    get(target, key) {
        return target[key] + ", spoon";
    },
    set(target, key) {}
};
const proxy = new Proxy(target, handler);
const get = proxy.food;
```
위와 같은 형태에서 Proxy의 둘째 인자에 handler를 작성.  
proxy.food에서 .food는 handler에서 'key'에 해당한다.  
필수 내부 메서드와 핸들러 메서드 : https://ko.javascript.info/proxy


# 4. Proxy 인스턴스 생성 (new Proxy(), Proxy.revocable())
Proxy는 exotic object이다. (특별한 동작을 하는 object.)  
Proxy에는 생성자가 정의되어있지 않다!

Proxy.revocable()  
비활성화 가능한 Proxy 객체를 생성.  
기능은 같지만, new Proxy()와 접근방식이 다르다.  
ex)
```js
const target = {point: 100};
const handler = {
    get(target, key) {
        return target[key];
    }
};
const obj = new Proxy(target, handler);
const pt = obj.proxy.point; // Available!

obj.revoke();

obj.proxy.point; // Runtime Error!
```
