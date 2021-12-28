| <a id="a1"></a>목차                                          |
| ------------------------------------------------------------ |
| [1. 객체지향 프로그래밍](#1)<br/>                                  |
| [2. Javascipt 클래스](#2)<br/>                                    |
#| [3. 클래스의 작성기준](#3)<br/>                              |
#| [4. Constructor](#4)<br/>                                    |
#| [5. getter / setter, static메소드, 호이스팅](#5)<br/>        |
#| [6. 상속](#6)<br/>                                           |
#| [7. Super 키워드, constructor 호출](#7)<br/>                 |
#| [8. Built-in 오브젝트 상속, Object 상속, Image 오브젝트 상속, Audio 오브젝트 상속](#8)<br/> |
#| [9. this 참조, Generator](#9)<br/>                           |
#| [10. Proxy](#10)                                             |
#| [11. Proxy 논리](#11)                                        |
#| [12. handler, trap](#12)                                     |
#| [13. Proxy 인스턴스 생성](#13)                               |
#| [14. Proxy set trap](#14)                                    |
#| [15. Proxy get trap](#15)                                    |

<br/>

# <a id="1"></a>[1](#a1). 객체지향 프로그래밍
> ---
### Javascipt는 객체지향 프로그래밍
> - 이 때 Javascript의 object는 OOP의 object가 아님
>	- Javascript의 object는 property의 집합
>		- 형체와 실체가 있음
>		- property는 key-value, name-value로 구성
>		- OOP의 object보다 javascript의 object가 더 구체
>	- OOP의 object는 형체, 실체가 없음
>		- Behavior
>			- 행위
>			- 동적인 모
>		- Attribute
>			- 속성
>			- 행위의 대상
> ---
# <a id="2"></a>[2](#a1). Javascript의 class
> ---
### Prototype and Class
> - Javascript는 class 기반 언어가 아닌 prototype 기반
> - ES6로 넘어오며 class가 추가되었지만 이는 기존 prototype을 응용하여 만든 형태
>
#### Prototype?
- ##### https://velog.io/@turtle601/Prototype이란
- ##### https://ui.toast.com/weekly-pick/ko_20160603
> - Proto type 언어는 원형 객체를 복사하여 새로운 객체를 생성
> - 하지만 javascript는 원형 객체 복제가 아닌 프로토탕입 링크를 통해 원형을 참조한다
>   ![f14b233e-2c8e-11e6-988d-6ea081b4c984](https://user-images.githubusercontent.com/26926966/147544038-61ef36a1-a9ea-4318-807b-dae6ca023a98.png)

>   
>		- javascipt에서 원시타입을 제외한 모드 타입은 객체
>		- 객체를 생성할 때 해당 객체의 원형으 지속적을 체이닝 한 형태로 만들어짐
#### Prototype Chainning and Class
> ##### https://velog.io/@sik2/JS-CoreJavaScript-프로토타입-체이닝Prototype-Link-Prototype-Object

> ![image](https://user-images.githubusercontent.com/26926966/147547700-705a6271-7b06-4d3b-bcb0-cb8ec6e04cd1.png)
>   
>		- class를 instance화 하면 다음고 같이 __proto__가 생성

> ![image](https://user-images.githubusercontent.com/26926966/147547830-3d6a70ed-1d0c-4934-ba71-01c6fe3b846b.png)
>   
>		- javascript에서 객체 속성은 객체 -> __proto__ -> __proto__의 __proto__ ... 순으로 전파됨
>		- 만약 끝까지 찾았어도 없다면 undefind가 return
> ---
