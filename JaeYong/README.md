# 1. 객체지향 언어

> ### 자바스크립트의 객체지향적 특성
>
> - 자바 스크립트는 기본적으로 <U>**객체지향 언어**</U>이다.
> - 기본적으로 object를 이용하여 프로그램을 작성할 수 있다.
> - 자바스크립트의 object는 property의 **집합**이다.
> - **형체, 실체**가 있다. $\rightarrow$ property의 집합니이때문이다. class로 사용한다.
> - **prototype**에 메소드를 연결하는 구조이며 이 구조로 <U>**인스턴스**</U>를 생성한다.

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

> 클래스의 특징
>
> - 클래스 키워드를 만나면 object를 생성한다.
> - new 연산자를 생성하면 인스턴스를 생성한다.
> - 인스턴스를 생성하지 않고 method 나 property 를 불러오면 오류가난다

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

# 클래스의 작성기준

> - 세미콜론은 선택이다.
> - function 키워드를 사용하지 않는다.
> - 메소드와 메소드 사이에는 콤마 작성을 하지않는다.
> - 클래스의 type은 function이다.

### computed name

> - 메소드 이름을 조합하여 사용 가능하다.

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
