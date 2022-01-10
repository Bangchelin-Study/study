# promise

## promise?
- 비동기 - 연속으로 처리하지 않고 중간에 끊어졌다가 연결된 코드를 처리할 수 있는 환경이 될 때 실행
```js
let instance = new promise((resolve,reject) => {
    resolve(param1,param2,...);
    reject(param1,param2,...);
});
```
## then?
- then(성공 시 실행될 핸들러 함수, 실패시 실행될 핸들러 함수)
핸들러 함수에서 받을 매개변수에 resolve,reject 함수 중 호출된 함수의 매개변수의 가장 첫 번째값부터 들어간다.
- return 값은 [[PromiseValue]]값에 저장이 된다. then의 return 값은 promise 인스턴스 형태라 연속해서 then을 호출 가능하다. [[PromiseValue]] 값에 저장된 값이 return되므로 다음 then에서 매개변수로 받아 값 사용 가능.
```js
let obj = new promise((resolve) => {
    resolve(100);
});
obj.then((value) => {
    return value + 50;
    // 150이 반환되는 것이 아니라 [[PromiseValue]]에 150을 저장시킴.
}).then((param) =>{
    console.log(param);
});
// 150 출력.
```
## catch?
- then에서 두 번째 param을 작성하지 않고 reject의 경우 catch를 이용해서 에러를 잡아 실행.

## finaly?
- then과 catch와 달리 성공 여부와 상관없이 finally()에 인자로 받은 핸들러 함수를 실행시킨다. 성공 여부가 확실하지 않으므로 앞에서 return한 값에 대해 상태를 모르기 때문에 param으로 넘어오지 않는다.
```js
obj.then((value) => {return 200;}).catch((param) => {
    // 앞에서 실패하지 않았으므로 실행 안됨.
    return 100;
}).finally((param) => {
    //여기서 param은 then에서 return 해준 값이 들어가지 않는 undefined. finally는 어떤 값도 현재 param의 위치에 받지 않기 때문.
    //평소엔 그냥 매개변수 없이 사용.
    console.log(param);
})
```
## all?
- promise.all 형태로 사용하며 all함수의 파라미터 안의 모든 promise 처리를 기다려서 모두 성공했을 때 넘어갈 수 있도록. 만약 실패한다면, 바로 넘어간다.

## race?
- promise.race 형태로 받은 인스턴스형태의 매개변수들 중 제일 먼저 끝난 promise에 대해 then을 실행하고 더 이상 then을 실행하지 않음.