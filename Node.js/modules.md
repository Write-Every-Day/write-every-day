## 모듈
자주 사용하거나 헷갈리는 모듈 정리

### global
브라우저의 `window`와 같은 전역 객체 역할, 생략해서 사용 가능하다.  
global에 값을 주입하면 해당 프로젝트 내 모든 파일에서 전역 변수로 사용할 수 있다.
```js
global.console.log("hello");
console.log("node!"); //생략
```
### console time
수행 시점에 time을 찍어 코드를 수행한 시간을 알아낼 수 있다. (효율성 측정)
```js
function add(a, b){
    console.timeEnd("time"); //time: 0.098ms
    return a+b;
}

console.time("add"); //측정 시작
add(1, 2);
```

### setImmediate
이벤트 루프내의 여러 호출 함수들이 `setTimeout` 순서를 보장하지 않아 즉시 함수를 출력하려면 `setImmediate`를 사용한다.  
첫 인자에 실행 함수를 전달하고, 두번째 인자에 전달할 값을 명시한다.
```js
setImmediate(()=>{
    console.log("hi");
});

setImmediate((message) => {
    console.log(message); //"hello"
}, "hello");
```

### this
```jsx
module.exports == exports == {}
```
웹 브라우저 상에서의 this 체계와 조금 다르다. (웹 브라우저에서의 전역 this == window 객체)
node.js 상에서는 전역 스코프상에서의 this는 `module 객체`를 가르킨다. 함수 내부에서의 this는 global 객체를 가르킨다. 
(나머지 내부함수에서의 this 참조 스코프, 객체에서의 this등 다른 부분에서는 동일하다.)

### process
현재 실행중인 노드 프로세스에 접근하는 모듈
+ **version :** 노드 버전
+ **platform :** 운영체제 정보
+ **pid :** 프로세스 id
+ **uptime() :** 프로세스 시작 후 흐린 시간(second)
+ **exePath :** 노드 실행 경로
+ **cwd() :** 프로세스 실행 위치
+ **env :** 환경 변수 객체 (외부에 노출되면 안되는 정보들을 주로 저장) 
+ **nextTrick() :** `Event Loop Task Queue`에서 우선권 부여 (promise-then과 같이 우선 처리)
+ **exit(0) :** node 종료 (exit(1)은 에러가 발생했음을 명시함)

### OS
운영체제에 대한 정보를 담은 객체
```js
const os = require('os');
```
+ **arch() :** 아키텍쳐
+ **freemem() :** 사용가능한 메모리 용량
+ **totalmemm() :** 전체 메모리 용량

### path
OS마다 다른 디렉토리 경로를 다루기위해 사용 (Window or POSIX 계열)
+ **parse('path..') :** 파일 구조 분해 (root-dir-base-ext..)
+ **isAbsolute('path) :** 절대경로인지 상대경로인지 true/fasle