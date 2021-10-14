# Cooperative asynchronous Javascript Timeouts and intervals

* 참고 사이트 : https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Timeouts_and_intervals

일정한 시간이 흐른 뒤에 비동기적 코드를 실행할 수 있게 하는 다양한 함수가 있음
1) setTimeout() : 특정 시간 후에 코드 실행**(한번)**
2) setInterval() : 일정한 시간을 간격으로 코드 실행
3) requestAnimationFrame() : setInterval()의 최신 버전

⇒ 대부분 일정한 애니메이션 및 기타 배경 처리를 실행하는데 사용된다고 함.

## setTimeout()

사용 예제)
```js
let myGreeting = setTimeout(function() {
  alert('Hello, Mr. Universe!');
}, 2000)
```

사용 예제2) 매개변수 전달 또한 가능
```js
function sayHi(who){
	alert('Hello ' + who + '!');
}

let myGreeting = setTimeout(sayHi, 2000, 'Mr. Universe');
```

사용 예제3) timeout 취소 방법
```js
clearTimeout(myGreeting);
```

## setInterval()
일정한 시간을 두고 반복적으로 실행하는 것이 setTimout()함수와 다른 점이다.

사용 예제)
```js
function displayTime() {
   let date = new Date();
   let time = date.toLocaleTimeString();
   document.getElementById('demo').textContent = time;
}

const createClock = setInterval(displayTime, 1000);
```

사용 예제2) interval 취소하기
```js
clearInterval(myInterval);
```
취소 안되면 계속 실행 됨.






