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
    point: {value: 100, writable: true} // ES5 문법(Data descripter), point: 100과 같은 의미
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
### has()  
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

### deleteProperty()  
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


# 5. defineProperty(), preventExtensions(), isExtensible()
### defineProperty()  
파라미터 : target, key, descriptor(추가/변경할 디스크립터)
반환값 : boolean형 (성공: true, 실패: false)  
프로퍼티를 추가/변경할 때 호출되는 트랩이다.  
ex)  
```js
const target = {};
const handler = {
    deleteProperty(target, key, desc) {
        desc.value = desc.value * -1;
        Object.defineProperty(target, key, desc);
        return true;
    }
};
const obj = new Proxy(target, handler);
Object.defineProperty(proxy, "point", {
    value: 100, writable: true  // Data descriptor
})
log(obj.point)   // -100 출력
```

### preventExtensions()
파라미터 : target
반환값 : boolean형 (성공: true, 실패: false)  
target 오브젝트를 수정/확장 불가능하게 해주는 함수이자 트랩이다.
특징 : trap이 true를 반환하면 함수는 Proxy 인스턴스를 반환한다.  
준수사항 : target이 확장 불가일때 false를 반환하면 TypeError가 뜬다.

### isExtensible()
파라미터 : target
반환값 : boolena형 (확장 가능 : true, 확장 불가 : false)
target 오브젝트가 확장 불가인지 여부를 반환.  
다음과 같은 상황에서 false가 반환된다.  
Object.seal() Object.freeze() Object.preventExtensions() Reflect.preventExtensions()  
준수사항 : 원래 함수의 결과를 바꾸어서는 안된다.  

ex)  
```js
const target = {point: 100};
const handler = {
    preventExtensions(target) {
        if (target.point) {
            Object.preventExtensions(target);
            return true;
        }
        return false;
    }
};
const obj = new Proxy(target, handler);
const prevented = Object.preventExtensions(obj);
log(obj.point);   // 100 출력
log(Object.isExtensible(target));    // false 출력
log(Object.isExtensible(obj));    // false 출력
log(Object.isExtensible(prevented));    // false 출력
``` 


# 6. getPrototypeOf(), setPrototypeOf()
### getPrototypeOf()
파라미터 : target  
반환값 : 오브젝트 or null  
target의 prototype을 반환한다. (즉, target은 클래스여야 한다.)  
prototype을 가져오는 경우 이 트랩이 실행된다.  
ex)
```js
class Point {
    getPoint() {return 100;}
};
const handler = {
    getPrototypeOf(target) {
        return target.prototype;
    }
};
const obj = new Proxy(Point, handler);
log(Object.getPrototypeOf(obj).getPoint);   // getPoint() {return 100;} 출력
```

### setPrototypeOf()
파라미터 : target, prototype  
반환값 : boolean형 (성공: true, 실패: false)  
target의 __proto__ 에 prototype을 설정한다. (prototype에 설정하지 않는다!)  
준수사항 : target이 확장 불가일 때, prototype과 Object.getPrototypeOf(target)의 값이 같아야한다.  
ex)  
```js
class Book {setTitle() {return "A";}};
class Point {getPoint() {return 100;}};
Object.setPrototypeOf(Book, Point.prototype);

log(Book.prototype.getPoint);   // undefined 출력
log(Book.__proto__.getPoint);   // getPoint() {return 100;} 출력
const obj = new Book();
log(obj.getPoint);  // getPoint() {return 100;} 출력
```


# 7. construct(), apply() ownKeys(), getOwnPropertyDescriptor()
### construct()
파라미터 : target, argList(Array 자료형), newTarget(선택사항)  
반환값 : 생성한 인스턴스  
new 연산자의 트랩이다.  

### apply()
파라미터 : target(호출할 함수), this(선택사항, this 연산으로 참조할 오브젝트), params(선택 사항)  
반환값 : 호출된 함수에서 반환한 값  
함수 호출에 대한 트랩이다.

### ownKeys()
파라미터 : target(대상 오브젝트)  
반환값 : Array(프로퍼티 key)  
target의 모든 key를 배열로 만들어 반환하는 트랩이다.  

### getOwnPropertyDescriptor()
파라미터 : target, key  
반환값: Descripotr or undefined  
프로퍼티의 디스크립터를 반환하는 트랩.  
{value: , writable: , enumerable: , configurable: } 형태의 Data descriptor가 반환된다.