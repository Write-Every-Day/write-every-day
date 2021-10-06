## 예외처리
### 예외처리의 중요성
`node.js`는 기본적으로 싱글 쓰레드로 동작하기 때문에 에러가 발생하면 서버 자체가 죽어버린다.  
따라서 예외 처리는 노드 프로세스를 보호하기 위해 필수적으로 핸들링 해주어야한다.

### 예외와 에러 차이
+ **예외:** 어떠한 로직을 수행할 때 예기치 않는 동작 발생을 대비해 미리 보완을 해주는 것
+ **에러:** 메모리 부족 및 코드 오탈자 등 즉시 복구하기 쉽지 않은 상황

### 예외처리 방법
#### try-catch
에러가 발생할 만한 곳을 `try-catch`문법으로 감싸준다.
+ **try:** 에러가 발생하게되면 더 이상 try블록을 시행하지 않고, `catch`블록을 실행시킴
+ **catch:** 에러를 처리하는 구문
+ **finally:** 성공적으로 구문을 수행하든지 에러가 발생하든지 동작을 마치고 무조건 실행시키는 구문 (선택사항)
```js
    setInterval(function(){
        cosole.log("start");
        try {
            throw new Error("Error!!");
        } catch(error){
            console.error(error);
        } finally {
            console.log("done");
        }
    }, 1000);
``` 

#### process 객체 활용
에러 처리로도 발생시키지 못한 에러들도 핸들링해줄 수 있다.
이 방법은 무분별하게 사용하는 것 보다 로깅 용도로하는 것을 추천한다.
(모든 에러들을 수집하면서 콜백 순서를 보장하지 않기 때문!)
```js
    process.on("unCaughtException", function(error){
        console.error("알 수 없는 오류");
    });
```