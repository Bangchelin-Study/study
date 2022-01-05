# 2. Proxy

## Proxy란?
 - 기본 오퍼레이션을 중간에 가로채서 대신 실행
 - ``` Object.key == Object.GET(key)``` 요런 느낌
 - 왼쪽 형태를 오른쪽 형태로 바꿔주는게 proxy.

 ```js
    const target = {food: "밥"};
    const middle = new Proxy(target,{}); 
    // ^ 얘를 써줌으로써 내부 함수 사용해서 food 반환함
    cosnt left = middle.food;
    log(left);
 ```
- 핸들러를 작성하여 트랩을 이용해 사용

## Use Proxy 
 - new Proxy(Object, handler) 꼴로 선언. handler는 없을 경우 {}로 넣어준당
 - revocable 용도는 모르겠음. obj.proxy.key 꼴로 get 호출.
