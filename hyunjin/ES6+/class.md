# Class

- ECMAScript is an object-oriented programming language
- javascript는 객체 지향 프로그래밍 언어입니다.

ES5 에서는 class에 메서드를 추가할 때 prototype에 추가하는 식으로 작성하였지만, ES6부터는 class에 등록하면 엔진이 prototype에 추가시켜 줍니다. prototype 의 구조는 변하지 않았습니다.

## Class 표현식

```js
const Name = class {body}
```

- 클래스 작성 방법
  - 변수 이름 Name이 클래스 이름이 됩니다.
  - 변수에 Class 오브젝트를 할당하는 형태입니다. 

클래스는 property 와 prototype, _ _proto__ 로 구성됩니다.

언더바 proto 에서 빌트인 Function 오브젝트의 prototype에 연결된 메서드를 참조합니다. 

인스턴스에는 __ proto __ 가 있으며 그안에는 constructor 와 메서드들이 들어간다. (실제 클래스의 메서드를 참조)

## Class 작성 기준

- 클래스는 strict 모드에서 실행되므로 이에 맞추어서 코드를 작성해야합니다.
- 클래스에 메소드 작성 방법
  - fucntion 키워드를 작성하지 않습니다.
  - 메소드와 메소드 사이에 콤마를 작성하지 않습니다.
  - 세미콜론 작성은 선택입니다.
- 클래스의 typeof는 function입니다.
  - Class 타입이 별도로 있지 않습니다.

### computed name
- 메서드 이름을 조합하여 사용
  - 대괄호 안에 조합할 이름을 작성
  - 조합한 결과가 메소드 이름이 됩니다.

```js
const name = 'Point'
class Point {
  static["get" + name] (add) {
    return add ? 100: 50
  }
}
console.log(Point["get" + name](true))
```

static 은 정적메소드이고 클래스이름에 메소드이름으로 호출할 수 있다.

## constructor

constructor를 작성하지 않은 상태에서 new 연산자로 인스턴스를 생성하면 prototype 에 연결된 constructor가 호출됩니다. 따라서 인스턴스를 생성할수는 있지만 인스턴스에 초깃값을 설정할 수 없습니다. 클래스에 constructor 를 작성하면 prototype.constructor를 오버라이드하게 됩니다.


### constructor 반환
- constructor 에 return 문을 작성하지 않으면 생성한 인스턴스를 반환합니다.
- constructor 에서 Number,String 을 반환하면 이를 무시하고 인스턴스를 반환합니다.
- constructor 에서 Object를 반환하면 인스턴스를 반환하지 않고 Object를 반환합니다.

