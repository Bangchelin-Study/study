| <a id="a1"></a>목차                                          |
| ------------------------------------------------------------ |
| [1. 객체지향 언어](#1)<br/>                                  |
| [2. 클래스 선언](#2)<br/>                                    |
| [3. 클래스의 작성기준](#3)<br/>                              |
| [4. Constructor](#4)<br/>                                    |
| [5. getter / setter, static메소드, 호이스팅](#5)<br/>        |
| [6. 상속](#6)<br/>                                           |
| [7. Super 키워드, constructor 호출](#7)<br/>                 |
| [8. Built-in 오브젝트 상속, Object 상속, Image 오브젝트 상속, Audio 오브젝트 상속](#8)<br/> |
| [9. this 참조, Generator](#9)<br/>                           |
| [10. Proxy](#10)                                             |
| [11. Proxy 논리](#11)                                        |
| [12. handler, trap](#12)                                     |
| [13. Proxy 인스턴스 생성](#13)                               |
| [14. Proxy trap](#14)                                        |

<br/>

# <a id="1"></a>[1](#a1). 객체지향 언어

> ---

> ### 자바스크립트의 객체지향적 특성
>
> - 자바 스크립트는 기본적으로 <U>**객체지향 언어**</U>이다.
> - 기본적으로 object를 이용하여 프로그램을 작성할 수 있다.
> - 자바스크립트의 object는 property의 **집합**이다.
> - **형체, 실체**가 있다. $\rightarrow$ property의 집합니이때문이다. class로 사용한다.
> - **prototype**<sup >[1](#prototype)</sup> <U>**인스턴스**</U>를 생성한다.
>   <br/><br/>

> ---

```javascript
//class를 이용하여 object 생성
class Point(){
    //point라는 property
    constructor(point) {this.point = point;}

    //getPoint라는 함수 method
    getPoint(){return point;}
}

//obj라는 인스턴스 생성
const obj = new Point(100);
console.log(obj.getPoint());
console.log(obj.point);
/*
*   [결과값]
*   100
*   100
*/
```

<br/>
<br/>
<br/>

# <a id="2"></a>[2](#a1). 클래스 선언

> ---

> ## 클래스의 특징
>
> - 클래스 키워드를 만나면 object를 생성한다.
> - new 연산자를 생성하면 인스턴스를 생성한다.
> - 인스턴스를 생성하지 않고 method 나 property 를 불러오면 오류가난다
>   <br/><br/>

> ---

### 클래스 표현식

```javascript
//Point라는 클래스 선언
const Point = class {
  getPoint() {
    return 100;
  }
};
```
<br/>
<br/>

---

---

## prototype이란?<a id="prototype"></a>[](#a1)

- 자바스크립트의 object는 모든 객체의 property와 method를 prototype에서 상속받고 있다.
- 이런 구조를 갖고 객체의 형태를 띈다.

```javascript
function object(name, first, second) {
  this.name = name;
  this.first = first;
  this.second = second;
}
object.prototype.sum = function () {};

var kim = new object("kim", 10, 20);
```

| object    |
| --------- |
| prototype |

$\uparrow\downarrow$

| object's prototype     |
| ---------------------- |
| constructor<br/> sum() |

$\uparrow\downarrow$

| kim                                    |
| -------------------------------------- |
| proto<br/> name<br/> first <br/>second |

### proto 와의 차이점은?

- proto의 경우는 인스턴스를 생성했을 때, 인스턴스의 proto는 *object의 prototype 에 연결 링크*를 제공한다.

---

---

<br/>
<br/>
<br/>

# <a id="3"></a>[3](#a1). 클래스의 작성기준

> ---

>
> - 세미콜론은 선택이다.
> - function 키워드를 사용하지 않는다.
> - 메소드와 메소드 사이에는 콤마 작성을 하지않는다.
> - 클래스의 type은 function이다.
>   <br/><br/>

> ---

> ## Computed name
> 
> - 메소드 이름을 조합하여 사용 가능하다.
>   <br/><br/>

> ---

```javascript
const name = "point";
class Point {
  static ["get" + name](add) {
    return add ? 100 : 50;
  }
}

const obj = new Point();
Point.prototype.getPointProto = function () {
  return 50;
};

console.log(Point["getpoint"](true));
console.log(obj.getPointProto());
/*[결과]
 *   100
 *   50
 */
```

<br/>
<br/>
<br/>

# <a id="4"></a>[4](#a1). Constructor

> ---

> ## constructor란?
>
>    <br/>
>
> - constructor는 자바스크립트에서 생성자이다.
>   <br/> <br/>
>
> ---

```javascript
class Point {
  //constructor(생성자) 가 point라는 property를 생성해준다.
  constructor(point) {
    this.point = point;
  }
}

const obj = new Point(100);
```

> ---

> ## constructor의 실행순서
>
>   <br/>
>
> 1.  new 연산자를 만나면 constructor를 호출한다.
> 2.  **빈 오브젝트**(인스턴스)를 생성한다.
> 3.  인스턴스에 property 이름과 값을 설정하여 인스턴스 구조를 만든다.
> 4.  proto, proto.constructor등의 구조가 생성된다.
> 5.  constructor를 실행한다.
> 6.  this를 실행을 한다. 이때 this는 인스턴스가 있으므로 스스로 참조가 가능하다.
> 7.  **_this.point (undefined)은 인스턴스의 property가 된다._**
> 8.  point의 값은 100이된다.
> 9.  생성한 인스턴스를 반환한다.
>     <br/> <br/>
>
> ---

<br/>
<br/>

> ---

> ## constructor의 초기화
>
> >
> > ### constructor를 작성하지 않았을 경우
> >
> > - 클래스 전체를 참조하도록 환경을 만든다.
> > - prototype.constructor를 사용하므로 인스턴스 생성은 가능하지만 초기화가 불가능
> >
> > ### constructor를 작성한 경우
> >
> > - prototype.constructor를 오버라이드한다.
> >   <br/><br/>

> ---

> ## constructor의 반환
>
> >
> > ### constructor의 return을 작성하지 않았을 경우
> >
> > - 생성한 인스턴스를 반환한다.
> >
> > ### constructor의 return을 작성한 경우
> >
> > - Number, String을 반환할 경우 이를 무시하고 인스턴스를 반환한다.
> >
> > - Object를 반환하면 Object를 반환한다.
> >   <br/>
> >
> >   <br/>

---

> ---

```javascript
class Point {
  //constructor(생성자) Object를 반환한다.
  constructor(point) {
    return { point: point };
  }
}

const obj = new Point(100);
log(obj);
log(obj instanceof Point);
/*
 * [결과]
 * {point : 100}
 * false      object이므로 인스턴스가 아니다
 */
```

# <a id="5"></a>[5](#a1). getter / setter, static메소드, 호이스팅

> ---

> ## getter / setter
>
> >
> > - getter / setter 는 메소드를 호출하여 값을 구한다.
> >
> > - 메소드와 같이 ()를 작성하지만, 사용할때는 변수와 같이 사용한다.
> >   <br/><br/>

> ---

```javascript
class Point {
  constructor(point) {
    this.point = point;
  }
  //getter의 키워드는 get이다.
  get getPoint() {
    return this.point;
  }
  set setPoint(point) {
    this.point = point;
  }
}

const obj = new Point(100);
obj.getPoint = 40;
log(obj.getPoint);
/*
 *	[결과]
 *	40
 */
```

> ---

> ## static
>
> >
> > - 구조적 특성은 prototype이 아닌 **클래스**에 직접 연결되어 있어 인스턴스에서는 호출할 수 없다.
> >   <br/> <br/>

> ---

```java
class Point{
    //static 키워드는 static이다.
    static getPoint(){
        return 100;
    }
};

log(Point.getPoint);
/*
*	[결과]
*	100
*
*	const obj = new Point(100);
*	obj.getPoint() 오류난다. or undefined
*/
```

> ---

> ## 호이스팅
>
> >   <br/>
> >
> > - 클래스는 호이스팅되지 않는다.
> >
> > - const, let 변수처럼 class 키워드가 있는 시점에서 오브젝트를 생성하기 때문이다.
> >   <br/><br/>

> ---

```javascript
try {
  const obj = Point;
} catch {
  log("호이스팅 불가");
}
// 미리 값 불러오려하는 것, 호이스팅

class Point {
  static getPoint() {
    return 100;
  }
}

console.log(Point.getPoint());
// 선언 후에 불러오는 것
/*
 *	[결과]
 *	호이스팅 불가
 *	100
 */
```

> ---

> ## new.target
>
> >   <br/>
> >
> > - new.target 프로퍼티는 함수 or 생성자가 new 연산자로 호출된 여부를 반환한다.
> >
> > - new 연산자로 constructor를 호출하면 new.target은 constructor를 참조한다.
> > - 함수로 호출하면 undefined 반환
> >   <br/> <br/>

> ---

```javascript
class Point {
  construvtor() {
    log(new.target.name);
  }
}
new Point();
/*
 *	[결과]
 *	Point
 */

function book() {
  log(new.target);
}
book();

/*
 *	[결과]
 *	undefined
 */
```

<br/>
<br/>

# <a id="6"></a>[6](#a1). 상속

> ---

> ## 상속의 특징
>
> >
> > - 상속은 OOP기능 중 하나이다.
> > - 클래스에 다른 클래스를 포함시키는 형태이다.
> > - 포함시킨 클래스의 property를 사용할 수 있다.
> > - 키워드는 extends 이다.
> >   <br/> <br/>

> ---

```javascript
class Book {
  constructor(title) {
    this.title = title;
  }
  getTitle() {
    return this.title;
  }
}

//extends 키워드로 book을 상속 받았다.
class Point extends Book {
  setPoint(point) {
    this.point = point;
  }
}

//이 순간 Point의 prototype에는 setPoint가 존재하며 그안의 __proto__에 getTitle()이 존재한다.

const obj = new Point("책");

// obj는 Book에서 실행된 constructor에 의해 title="책"이 존재하며, __proto__ 에는 Book 인스턴스가 존재한다.

console.log(obj.getTitle());

// 처음에 point에서 setTitle()함수를 찾고, 없다면 book으로 가서 찾는다.

/*
 * [결과]
 *  책
 */
```

<br/>
<br/>

# <a id="7"></a>[7](#a1). Super 키워드, constructor 호출

> ---

> ## Super
>
> >
> > - 슈퍼 클래스와 서브 클래스에 같은 이름의 메소드가 있으면 서브 클래스의 메소드가 호출된다.
> > - super키워드를 이용해 슈퍼 클래스의 메소드를 호출할 수 있다.
> > - ex) super.getTitle
> >   <br/> <br/>

> ---

```javascript
class Book {
  getTitle() {
    console.log("슈퍼");
  }
}

//extends 키워드로 book을 상속 받았다.
class Point extends Book {
  getTitle() {
    super.getTitle();
    console.log("서브");
  }
}

new Point().getTitle();

/*
 * [결과]
 *  슈퍼
 *  서브
 */
```

> ---

> ## Constructor 호출
>
> >
> > - 서브와 슈퍼에 constructor를 모두 작성하지 않으면 defalt constructor가 호출된다.
> > - 슈퍼에만 작성시 파라미터 값을 슈퍼로 넘겨준다.
> > - 서브에서만 작성시 에러가난다.
> > - 둘다 작성시 서브에서 super()로 호출해야 한다.
> >   <br/> <br/>

> ---

```javascript

//          슈퍼, 서브 둘다 작성 x

class Book {
  setTitle(title) {
    this.title = title;
  }
}

//extends 키워드로 book을 상속 받았다.
class Point extends Book {}

const obj = new Point();
obj.setTitle("책");
console.log(obj.title);
/*
 * [결과]
 *  책
 */


<------------------------------------------>


//              슈퍼에만 작성

class Book {
  constructor(title){
    this.title = title;
  }
}

//extends 키워드로 book을 상속 받았다.
class Point extends Book {}

const obj = new Point("책");
console.log(obj.title);
/*
 * [결과]
 *  책
 */

<----------------------------------------->


//                둘다 작성

class Book {
  constructor(title){
    this.title = title;
  }
}

//extends 키워드로 book을 상속 받았다.
class Point extends Book {
  constructor(title,point){
    super(title);
    this.point = point;
  }

}

const obj = new Point("책",100);
console.log(`${obj.title}, ${obj.point}`);
/*
 * [결과]
 *  책, 100
 */
```

<br/>
<br/>

# <a id="8"></a>[8](#a1). Built-in 오브젝트 상속, Object 상속, Image 오브젝트 상속, Audio 오브젝트 상속

> ---

> ## Built-in 상속, Audio, Image
>
> >
> > - 빌트인 오브젝트 상속이 가능하며 인스턴스가 빌트인 오브젝트의 특성일 갖게 된다.
> > - this로 빌트인 오브젝트에 접근할 수 있다.
> > - extends 키워드로 구현한다.
> > - const value of this (array에서) 와 같은 키워드를 상속받아 for문을 반복할 수 있다.
> > - Audio, Image도 거의 비슷한 개념으로 상속 가능한다.
> >   <br/> <br/>

> ---

> ## Object 상속
>
> >
> > - 클래스가 아닌 오브젝트는 실제 상속은 안되지만, Object.setPrototypeOf( const a, const b )로 상속과 같은 구조로 만들 수 있다.
> >   <br/> <br/>

> ---

<br/>
<br/>

# <a id="9"></a>[9](#a1). this 참조, Generator

> ---

> ## this 참조
>
> >
> > - 인스턴스.메소드() 형태로 호출하면 메소드에서 this가 인스턴스를 참조한다.
> > - static 메소드에서 this는 메소드가 속한 **클래스**를 참조한다.
> > - <br/> <br/>

> ---

```javascript
class Point{
  static setPoint(point){
    this.value = point;
  }
}
Point.setPoint(100);
    //클래스 오브젝트 안에 설정이 된다.
console.log(Point.value)
    //클래스 오브젝트 안에 value값을 설정한다.
console.log(new Point().value);
    //인스턴스를 생성했기 때문에 static이 존재하지않아 없는 값을 참조한다.

/*
 * [결과]
 *  100
 *  undefined
 */

 <------------------------------>
 class Point{
   constructor(){
     log(this.constructor.get());
   }
  static get(point){
    return 100;
  }
}
new Point();

// 이렇게 사용하면 static도 인스턴스에서 사용이 가능하다.

/*
 * [결과]
 *  100
 */
```

> ---

> ## Generator
>
> >
> > - 클래스의 제너레이터 함수는 prototype에 연결된다.
> > - 인스턴스로 호출해야 한다.
> > - <br/> <br/>

> ---

```javascript
class Point {
  *getPoint() {
    yield 10;
    yield 20;
  }
}
const gen = new Point();

//제너레이터 함수를 호출한다.
const obj = gen.getPoint();
log(obj.next());
log(obj.next());
log(obj.next());

/*
 * [결과]
 *  {value: 10, done: false}
 *  {value: 20, done: false}
 *  {value: undefined, done: true}
 */
```

<br/>
<br/>

# <a id="10"></a>[10](#a1). proxy

> ---

> ## Proxy의 의미
>
> >
> > - 기본 오퍼레이션을 중간에서 가로채어 오퍼레이션을 대신하여 실행한다.
> > - 전체 괘도를 벗어날 수 없으므로 완전히 못바꿈
> >   <br/> <br/>

> ---

커피를 주문하는 오퍼레이션을 자바스크립트로 표현하면

```javascript
const counter = { order: "커피" };

//counter.order을 실행하여 함수를 실행하지 않고 property를 구했으며 이것은 getter이다.
const 주문자 = counter.order;

log(주문자);
// 즉 getter가 실행되며 값이 반환된다. 이것이 기본 오퍼레이션이다.

/*
 * [결과]
 * 커피
 */
```

<br/>
<br/>

> ---

> ## 기본 오퍼레이션 논리
>
> >   <br/>
> >
> > - counter.order실행시 "커피"를 구해야한다. 즉, 값을 구하는 메소드가 필요하다.
> > - 이때 엔진은 getter기능을 가진 내부 메소드 get을 호출한다. -->
> >   counter.order을 실행하면 proto에 있는 get을 호출한다.
> > - get을 호출하면 order를 넘겨주며, order를 property키로 하여 값을 구해 반환해준다.
> > - 13개의 기본 메소드가 있다.
> >   <br/> <br/>

> ---

<br/>
<br/>

# <a id="11"></a>[11](#a1). Proxy 논리

> ---

> ## Proxy 모습
>
> >
> > - 순서대로 1,2,3번의 사람이 있다고 가정하자
> > - 세사람은 식사를 해야한다.
> > - 왼쪽에서부터 중간, 중간에서부터 오른쪽 순으로 전달을 하면 왼쪽, 중간, 오른쪽 순으로 밥을 받는다.
> > - 이때, 중간사람이 proxy라고 할 수 있다. 대리자 역할인 것이다.
> > - 왼쪽사람이 오른쪽사람에게 직접 전달하면 proxy가 필요하지않다.
> >   <br/> <br/>

> ---

```javascript
//왼쪽 사람이 밥을 갖고있다.
const target = { food: "밥" };
//중간 사람이 target의 값을 받고
const middle = new Proxy(target, {});
//= 연산자가 전달해 준다.
const left = middle.food;

/*
1. middle.food가 실행이되면 target의 getter를 호출하면서 food를 파라미터로 전달.
2. new Proxy() 파라미터에 target을 작성하므로 middle에서 target을 알 수 있다.
*/

console.log(left);

/*
 * [결과]
 *   밥
 */
```

# <a id="12"></a>[12](#a1). handler, trap

> ---

> ## Proxy의 [사용이유](#proxy)
>
> >
> > - 가운데 사람이 밥을 받아 오른쪽에 전달해 준다. 근데 문제는 오른쪽 사람은 밥이 아닌 수저까지 필요하기때문에 중간사람이 수저까지 전달해 줘야한다.
> >   <br/><br/>
>
> ---
>
> ## target
>
> >
> > - target은 Proxy 대상 오브젝트이다.
> > - Array, Object 등을 사용할 수 있다.
> > - const obj = new Proxy(target, {})형태
> > - 첫 번째 파라미터에 target을 작성한다.
> > - Proxy 인스턴스와 target이 연결된다.
> > - <br/> <br/>
>
> ---
>
> ## trap
>
> >
> > - OS에서 사용하는 용어
> > - 실행 중인 프로그램에 이상이 생겼을 때 실행을 중단하고 사전에 정의된 제어로 전환
> >   <br/><br/>
>
> ---
>
> ## handler
>
> >
> > - 오브젝트에 get(), set()이 있다.
> > - handler를 핸들러 오브젝트라고 하며 핸들러라고 부른다.
> >   <br/><br/>
>
> <br/>

> ---

<a id="proxy"></a>

```javascript
//Proxy의 사용이유

//target은 밥 이라는 값만 갖고있다.
const target = { food: "밥" };
/*left는 수저와 밥을 동시에 받아야한다.
  그래서 중간에서는 수저를 같이 전달해 줘야한다.
*/
const handler = {
  get(target, key) {
    return target[key] + ", 수저";
  },
  set(target, key) {},
  //get과 set은 각각 getter, setter이고 동시에 trap이라고 한다.
};
// target에게 밥이라는 값을 받고 수저를 더해서 전달해 줘야한다.

const middle = new Proxy(target, handler);

const left = middle.food;
/*
1. 여기서 middle.food를 하면 [[Get]]대신에 Proxy에 등록된 get()트랩을 실행한다.
2. middle.food에서 key를 food로 받고 target[food]->밥 을 전달 받는다.
3. 밥 + , 수저 를 받아 left는 밥, 수저가 된다.
*/
console.log(left);
```

# <a id="13"></a>[13](#a1). Proxy 인스턴스 생성

|   구분   |                    개요                    |
| :------: | :----------------------------------------: |
| 파라미터 | target, 대상 오브젝트<br/>handler 오브젝트 |
|   반환   |           생성한 Proxy 인스턴스            |

> ---

> <br/>
>
> - Proxy 인스턴스를 생성하여 반환한다.
> - 첫 번째 파라미터
>   > <br/>
>   >
>   > - proxy 대상 target 오브젝트를 작성
>   > - Object, Array, Function 등
>   >   <br/><br/>
> - 두 번째 파라미터
>   > <br/>
>   >
>   > - 핸들러 작성
>   >   <br/><br/>
>
> <br/>

> ---

```javascript
const target = ["A", "B"];
const handler = {
  get(target, key) {
    return target[key] + ", 천 번째";
  },
};
const obj = new Proxy(target, handler);
/*
Proxy에서 target과 handler를 연결해 준다.

주의사항)
  new연산자가 없으면 typeError가 발생한다.
  handler를 사용하지 않더라도 작성하지 않으면 에러발생
  
*/
console.log(obj[0]);
/*
[결과]
A, 천 번째
*/
```

> ---

> ## trap의 원리
>
> - 위 코드에서 obj는 Handler를 갖고 있으며 이 안에 get과 proto가 형성이 된다.
> - proto 안에는 trap인 get, set함수가 있는데, 엔진은 바깥에서부터 함수를 찾아가게된다.
> - 따라서 obj[0]에서 get 트랩이 실행 될때는 이미 바깥에 생성해둔 get에 의해서 trap이 발생한다. 

> ---

> ---

> ## Proxy.revocable()
>
> | 구분     | 개요                                               |
> | -------- | -------------------------------------------------- |
> | 파라미터 | target, 대상 오브젝트<br/>handler, 핸들러 오브젝트 |
> | 반환     | 생성한 오브젝트                                    |
>
> - Proxy를 사용할 수 없는 상태로 바꿀 수 있는 오브젝트를 생성, 반환

> ---

```javascript
const target = {point : 100};
const handler = {
    get(target, key){
        return target[key];
    }
};
const obj = Proxy.revocable(target, handler);
// Proxy를 사용하지 못하게 만드는 기능이 추가된 Proxy를 반환했다고 생각하면 편하다.
// 단 obj.proxy 의 형태로 호출한다. 여기에 인스턴스가 할당되는 것이다.
console.log(obj.proxy.point);

obj.revoke();
//obj의 Proxy사용을 막아주는 코드이다.

try{
    obj.proxy.point;
}catch{
    console.log("Proxy 기능 사용 불가");
}

/*
*	[결과]
*	100
*	Proxy기능 사용 불가
*/
```

<br/><br/>

# <a id="14"></a>[14](#a1). Proxy trap ( set 편 )

> ---

> ## set() 트랩
>
> | 구분     | 개요                                                         |
> | -------- | ------------------------------------------------------------ |
> | 파라미터 | target, 대상 오브젝트<br/>key, property key<br/>value, property value<br/>receiver, 설명 참조 |
> | 반환     | boolean (성공여부)                                           |
>
> - 프로퍼티를 설정하는 트랩으로 target 또는 receiver에 property( key, value )를 설정한다.
> - set 트랩이 호출되면 엔진이 실행 환경을 분석하여 파라미터 값을 설정한다.

> ---

```javascript
// set트랩이 설정되지 않은 경우
const target={};
const obj = new Proxy(target, {});
//set trap호출
obj.point = 100;
//get trap호출
console.log(obj.point);
/*
이러한 경우에는 엔진이 원래 가지고 있는 set,get trap을 이용하기 때문에 Proxy가 없어도 상관이 없다.
*/

<------------------------------------->
const target={};


const handler={
    set(target,key,value,receiver){
        target[key] = value + 200;
    }
};
const obj = new Proxy(target, handler);
/*
1. 처음 파라미터는 target을 설정한다.
2. key에 point를 설정한다.
3. value에 100을 설정한다.
4. receiver파라미터에 Proxy 또는 Proxy를 상속받은 오브젝트를 설정한다.
5. 파라미터는 이름으로 매핑하지않고 순서로 매핑한다.
*/
obj.point = 100;

//get trap호출
console.log(obj.point);
/*
*	[결과]
*	300
*/
```

> ---

> ## set 트랩 호출
>
> - proxy[key]=100을 실행하면 set 트랩이 호출된다.
>
> - Object.create(proxy, {프로퍼티}) 이때 set트랩이 호출된다.
>
>   > - 인스턴스에 있는 property 설정, 인스턴스에 없는 property 설정이 나뉜다

> ---

```javascript
//인스턴스에 없는 property를 설정할 때
const target = {};
const handler = {
    point : 700,
    set(target, key, value, receiver){
        //아래 코드를 주석처리하면 obj.point가 undefined가 나온다.
        //값 설정이 기본 오퍼레이션인데 없으니 수행하지 않는것.
        target[key] = value + 200;
    }
};
const proxy = new Proxy(target,handler);
/*
create함수를 호출하여 proxy를 상속받은 인스턴스를 생성한다.

[주의사항]
새롭게 만든 인스턴스이므로 obj에는 point가 없다!
*/
const obj = Object.create(proxy, {
    bonus:{value: 500, writable:ture}
});

/*
1. 처음으로 obj.point를 할당하게 된다.
2. point가 없으므로 set 트랩이 호출된다.
3. set 트랩 : target[key] = value + 200;
4. target에 {point: 300}을 설정합니다.
*/
obj.point = 100;

/*
1. obj.point는 obj 인스턴스 property로 point를 검색한다.
2. obj에는 point가 없다. target에 있다.
3. target에서 point를 검색한다.
4. 300을 반환한다.
5. handler 에서 point를 검색하지 않는다. {point:700}이 있지만 반환하지 않는다.
*/
console.log(obj.point);
//실제 콘솔에 찍어서 확인하는 것을 추천. 이해가 어려움


<----------------------------------------->

const target = {};
const handler = {
    point : 700,
    set(target, key, value, receiver){
        target[key] = value + 200;
    }
};
const proxy = new Proxy(target,handler);
const obj = Object.create(proxy, {
    point:{value: 100, writable:ture};
});

/*
1. obj에 point가 있다!
2. set trap을 실행하지 않는다.
3. point 100이 obj 인스턴스 property로 설정되고, obj.__proto__에 handler와 target이 설정되므로 point를 먼저 인식하기 때문이다.
4. {point: 100}의 value를 700으로 변경한다.
*/
obj.point = 700;
//obj에는 point가 있다.
console.log(obj.point);
//target에는 point가 없다.
console.log(target.point);

/*
*	[결과]
*	700
*	undefined
*/

```

