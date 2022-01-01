# 1. set()
파라미터 : target, key, value, receiver  
반환형 : boolean형 (성공: true, 실패: false)  
ex)
```js
const handler = {
    set(target, key, value, receiver) {
        target[key] = value;
    }
};
```

### Object.create(proxy, {property})  
proxy를 상속받은 인스턴스를 생성.  
proxy에 연결된 handler와 target을 사용할 수 있다.
ex)
```js
const target = {};
const handler = {
    set(target, key, value, receiver) {
        target[key] = value = 200;
    }
};
const proxy = new Proxy(target, handler);
const obj = Object.create(proxy, {
    point: {value: 100, writable: true} // ES5 문법(Data discripter), point: 100과 같은 의미
});
obj.point = 700
```
위 예제의 경우 obj.point가 700이 되며 handler는 실행되지 않는다.  
만약 obj에 point가 없었다면 target에서 point 프로퍼티를 생성했을 것.  
이 경우에는 handler또한 실행되어 target.point가 900이 된다.

set() 트랩에서는 반드시 target에 값을 설정해야함.
그러지 않으면 setter는 없는셈이 된다!  
이는 트랩 처리 준수사항임.


# 2. set()의 4번째 파라미터 (receiver), set()에서의 this
receiver 자리에는 trap을 발생한 객체 자신이 들어간다.
```js
const target = {};
const handler = {
    set(target, key, value, receiver) {
        log(Object.is(targer, receiver)); // false가 출력된다.
    }
};
const proxy = new Proxy(target, handler);
obj.point = 100;
```

set()에서 this는 handler 오브젝트를 참조한다.
```js
const target = {point: 100};
const handler = {
    point: 200,
    set(target, key, value, receiver) {
        log(this.point); // 200이 출력된다.
        this.book = "Book";
    }
};
const proxy = new Proxy(target, handler);
obj.point = 500;
log(handler.book); // "Book"이 출력된다.
log(target.book); // undefined가 출력된다.
```


# 3. get()
파라미터 : target, key, receiver  
반환값 : 프로퍼티 값

target의 프로퍼티가 Data discripter이면 [[Writable]]: false 혹은 [[Configurable]]: false일때 반환 값을 변경하여 return 불가


# 4. has(), deleteProperty()
has()  
파라미터 : target, key
반환값 : boolean (값이 있으면 true, 아니면 false)  
in 연산자의 트랩이다.
ex)
```js
const target = {point: 100};
const handler = {
    has(target, key) {
        return target[key];
    }
};
const obj = new Proxy(target, handler);
log("point" in obj); // true 출력
log("book" in obj); // false 출력
```

deleteProperty()  
파라미터 : target, key
반환값 : boolean형 (성공: true, 실패: false)  
delete 연산자의 트랩이다.  
오브젝트의 프로퍼티를 삭제하는 작업을 한다.  
ex)
```js
const target = {point: 100};
const handler = {
    deleteProperty(target, key) {
        if (key in target) {
            delete target[key];
            return true;
        }
        return false;
    }
};
const obj = new Proxy(target, handler);
log(delete obj.point); // true 출력
log(target.point); // undefined 출력
```