# 1. 객체지향 언어

> ---

> ### 자바스크립트의 객체지향적 특성
>
> - 자바 스크립트는 기본적으로 <U>**객체지향 언어**</U>이다.
> - 기본적으로 object를 이용하여 프로그램을 작성할 수 있다.
> - 자바스크립트의 object는 property의 **집합**이다.
> - **형체, 실체**가 있다. $\rightarrow$ property의 집합니이때문이다. class로 사용한다.
> - **prototype**<sup >[1](#prototype)</sup> <U>**인스턴스**</U>를 생성한다.

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

# 2. 클래스 선언

> ---

> 클래스의 특징
>
> - 클래스 키워드를 만나면 object를 생성한다.
> - new 연산자를 생성하면 인스턴스를 생성한다.
> - 인스턴스를 생성하지 않고 method 나 property 를 불러오면 오류가난다

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

---

---

<a id="prototype"></a>[1](#a1)

## prototype이란?

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

# 3. 클래스의 작성기준

> ---

> - 세미콜론은 선택이다.
> - function 키워드를 사용하지 않는다.
> - 메소드와 메소드 사이에는 콤마 작성을 하지않는다.
> - 클래스의 type은 function이다.

> ---

### computed name

> ---

> - 메소드 이름을 조합하여 사용 가능하다.

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

# 4. Constructor

> ---

> constructor란?
>
> - constructor는 자바스크립트에서 생성자이다.

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
> 1.  new 연산자를 만나면 constructor를 호출한다.
> 2.  **빈 오브젝트**(인스턴스)를 생성한다.
> 3.  인스턴스에 property 이름과 값을 설정하여 인스턴스 구조를 만든다.
> 4.  proto, proto.constructor등의 구조가 생성된다.
> 5.  constructor를 실행한다.
> 6.  this를 실행을 한다. 이때 this는 인스턴스가 있으므로 스스로 참조가 가능하다.
> 7.  **_this.point (undefined)은 인스턴스의 property가 된다._**
> 8.  point의 값은 100이된다.
> 9.  생성한 인스턴스를 반환한다.

> ---

<br/>
<br/>

> ---

> ## constructor의 초기화
>
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



# 5. getter / setter, static메소드, 호이스팅

> ---

> ## getter / setter
>
> > - getter / setter 는 메소드를 호출하여 값을 구한다.
> >
> > - 메소드와 같이 ()를 작성하지만, 사용할때는 변수와 같이 사용한다.
> >
> > <br/><br/>

---

```javascript
class Point{
    constructor(point){
        this.point = point;
    }
    //getter의 키워드는 get이다.
    get getPoint(){
        return this.point;
    }
    set setPoint(point){
        this.point = point
    }
};

const obj = new Point(100);
obj.getPoint=40
log(obj.getPoint);
/*
*	[결과]
*	40
*/
```

---

>## static
>
>> - 구조적 특성은 prototype이 아닌 **클래스**에 직접 연결되어 있어 인스턴스에서는 호출할 수 없다.

---

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



---

> ## 호이스팅
>
> > - 클래스는 호이스팅되지 않는다.
> >
> > - const, let 변수처럼 class 키워드가 있는 시점에서 오브젝트를 생성하기 때문이다.
> >
> >   <br/><br/>

---

```javascript
try{
    const obj = Point;
} catch {
  log('호이스팅 불가')  
};
// 미리 값 불러오려하는 것, 호이스팅

class Point{
    static getPoint(){
        return 100;
    }
};

console.log(Point.getPoint());
// 선언 후에 불러오는 것
/*
*	[결과]
*	호이스팅 불가
*	100
*/
```

---

> ## new.target
>
> > - new.target 프로퍼티는 함수 or 생성자가 new 연산자로 호출된 여부를 반환한다.
> >
> > - new 연산자로 constructor를 호출하면 new.target은 constructor를 참조한다.
> > - 함수로 호출하면 undefined 반환

---

```javascript
class Point{
    construvtor(){
        log(new.target.name)
    }
};
new Point();
/*
*	[결과]
*	Point
*/

function book(){
    log(new.target);
};
book();

/*
*	[결과]
*	undefined
*/
```

