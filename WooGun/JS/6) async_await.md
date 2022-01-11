# 6. async/await

## async?
- async는 키워드가 아니며 async function이 한 묶음.
- 비동기 환경에서 실행하지만 await를 이용하여 실행이 끝나야 다음을 실행하도록 함.
- Promise에서 reject()가 발생했을 때 에러 대처 형태.
1. try-catch 사용 형태
```js
async function func1(param){
    try{
        await func2(param); // promise 함수
    }catch(error){
        log(error);
    };
};
```
2. catch 사용 형태
```js
async function func1(param){
    await func2(param).catch(error){
        return 200;
    }.then((param) => {
        console.log(param);
    })
}
// 200 출력. catch문에서 return은 [[promiseValue]] 인스턴스 형태로 반환해주므로 then 한 번 더 사용 가능.
```

3. Promise가 아닌 value 자체를 반환하는 형태
```js
async function func1(param){
    const value = await func2(param).catch(error){
        return 200;
    }
    console.log(value);   
}
// 200 출력. value에 그냥 [[PromiseValue]]값을 넣어서 설정
```
