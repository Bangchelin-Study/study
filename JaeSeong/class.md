# 1. 객체 지향 프로그래밍
자바스크립스는 기본적으로 OOP언어이다.
js는 클래스 구조과 구현법이 다르다.
prototype에 메소드를 연결하는 구조.


# 2. class 선언, 구조, 표현

ex) class 선언문 예시
```js
class Point {
    getPoint() {
        return 100;
    }
};
const obj = new Point();
```

인스턴스는 항상 new 키워드를 통해 생성.

ex) class 표현식 예시
```js
const Point = class {
    getPoint() {
        return 100;
    }
};
const obj = new Point();
```

클래스의 구성요소 : property, prototype(생성자 및 작성 함수), _proto_
인스턴스의 구성요소 : property(인스턴스 프로퍼티), _proto_(클래스의 prototype을 참조)

값 자체가 대체되지 않는 한 const 키워드를 사용.


# 3. class 작성 기준
1. 메소드 작성법 : function 키워드를 작성하지 않는다.
2. 메소드 사이에 콤마를 작성하지 않는다.
3. 세미콜론은 선택적.
클래스의 typeof는 function 이다.
ex)
```js
class Point {
    setPoint(point) {
        this.point = point;
    }
    getPoint() {
        return this.point;
    };
};
```
computed name : 대괄호[] 안에 조합할 이름을 작성. 오브젝트를 만드는 시점에 메소드 이름이 결정된다.
ex)
```js
const name = "Point";
class Point {
    static ["get" + name](flag) {
        return add ? 100 : 50;
    }
};
getPoint(true);
```

class 작성기준
1. 메소드를 prototype에 연결하여 호출하면 안된다.
2. 클래스 밖에서 메소드를 prototype에 연결할 수 있다. 만든 인스턴스에도 바로 연결된다. (prototype sharing)
```js
const Point = class {};
const obj = new Point();
Point.prototype.getPoint = function() {
    return 100;
};
```


# 4. constructor, constructor 반환
ex) 사용법
```js
class Point {
    constructor(point) {
        // point가 인스턴스 프로퍼티로 등록된다.
        this.point = point;
    }
};
const obj = new Point(100);
```
constructor가 없으면 인스턴스를 생성할 수는 있지만, 초기화를 할 수 없다.
constructor에 return을 작성하지 않으면, 생성한 인스턴스를 반환.
constructor에서 Number, String을 반환하면 이를 무시하고 생성한 인스턴스를 반환.
constructor에서 Object를 반환하면 인스턴스 대신 Object 반환.


# 5. getter, setter, static 메소드
getter는 호출시 소괄호 없이 name만 작성함.
getter는 메소드 이름 앞에 get을 작성.
```js
class Point {
    constructor(point) {
        this.point = point;
    }
    get getPoint() {
        return this.point;
    }
};
const obj = new Point(100);
console.log(obj.getPoint);
```

setter또한 호출시 소괄호 없이 name만 작성함.
setter는 메소드 이름 앞에 set을 작성.
```js
class Point {
    set setPoint(point) {
        this.point = point;
    }
};
const obj = new Point();
obj.setPoint = 100;
```

static은 정적 메소드를 생성할 때 사용하는 키워드.
클래스만 static을 사용할 수 있다.
클래스명.name으로 접근.
static 메소드는 클래스에 연결되며, 생성한 인스턴스에 연결되지 않는다.

호이스팅(hoisting) : 인터프리터가 변수와 함수의 메모리 공간을 미리 할당하는 것.
클래스는 호이스팅되지 않는다. 따라서 코드 순서에서 클래스 선언 이전에 클래스를 사용할 수 없다.

new.target : new 연산자로 호출되었는지 여부를 감지하는 기능.
new 연산자로 constructor를 호출했다면 그 constructor를 참조하여 반환.
일반 함수로 호출하면 undefined 반환.
```js
class Point {
    constructor() {
        console.log(new.target.name);
    }
};
new Point();
```


# 6. 상속, extends, 메소드 오버라이딩
```js
class Book {
    // 소스 코드
}
class Point extends Book {
    constructor() {
        console.log(new.target.name);
    }
};
new Point();
```

상속한 클래스의 객체는 _proto_를 계층적으로 구성함. 인스턴스의 자식 클래스 멤버를 먼저 본 다음 부모 클래스 멤버를 탐색하러 간다.


# 7. super 키워드, 부모의 constructor 호출
super 키워드로 부모 클래스를 호출할 수 있다.
(엄밀히 super 키워드는 한 단계 아래의 _proto_ 를 참조한다.)

부모의 constructor 호출
1. constructor를 모두 작성하지 않으면 디폴트로 실행.
2. super 클래스에만 작성하면 인자를 super 클래스로 넘김.
3. 자식 클래스에만 작성하면 error 발생!
4. constructor를 모두 작성하면 자식 클래스에서 super() 를 통해 부모 생성자를 호출해야함.


# 8. Built-in 오브젝트 상속, Object 상속
Built-in 오브젝트 상속 : 인스턴스가 빌트인 오브젝트의 특징을 모두 가짐.
this 키워드로 빌트인 오브젝트에 접근.

Object 상속 : 엄밀히 Object는 클래스가 아니어서 상속이 불가.
하지만 _proto_ 구조를 만듬으로 상속처럼 구현할 수 있다.
Object.setPrototypeOf()로 구조를 만듬.
```js
const Book = {
    getTitle() {
        console.log("Book");
    }
};
const Point = {
    getTitle() {
        super.getTitle();
    }
};
Object.setPrototypeOf(Point, Book); // Point의 _proto_의 아래에 Book의 _proto_를 연결함으로 Point가 Book을 상속하는 것 처럼 작동
Point.getTitle();
```

Built-in 상속의 예시로 Image, Audio 상속이 있다.


# 9. this 참조, Generator
this
인스턴스.메소드() 의 형태에서 this는 인스턴스를 참조.
static 메소드에서 this는 메소드가 속한 클래스를 참조.
constructor에서 this.constructor는 클래스를 참조.

Generator : 여러 값을 필요에 따라 하나씩 반환. 제너레이터 함수는 호출하면 제너레이터 객체를 반환한다. 이 객체는 이터러블하다. {value: value, done: bool} 이 객체는 이터러블 하기 때문에 for-of 문으로 값을 얻을 수 있다.
참고 : https://ko.javascript.info/generators
클래스의 Generator 함수는 prototype에 연결된다.
Generator 함수는 인스턴스로 호출해야 한다.
```js
class Point {
    *getPoint() {
        yield 10;
        yield 20;
    }
};
```