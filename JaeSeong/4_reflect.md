# 1. Reflect의 특징
빌트인 오브젝트이다.  
constructor가 없으므로 인스턴스 생성이 안된다.  
Reflect.get() 형태로 호출.  
에러는 try-catch로 대응한다. boolean형으로 반환한다.  

Reflect 오브젝트의 함수는 Proxy 트랩과 이름이 같다.  
즉, Proxy 트랩을 모두 모아 미리 만들어둔 클래스가 Reflect.