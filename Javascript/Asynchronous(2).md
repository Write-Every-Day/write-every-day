# Javascript 비동기(2)

*참고 문헌*
* https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Introducing

## Synchronous Javascript
자바는 싱글 스레드이며 한번에 한 작업만 하나의 main thread에서 처리 될 수 있다. 각 작업이 처리되는 동안에는 렌더링이 일시 중지 됨
==**Javascript is synchrous => Execute the code block in order after hoisting**==

## Asynchronous Javascript
앞서 말한 동기 프로그래밍때문에 웹 API 기능은 현재 비동기 코드를 사용하여 실행되고 있다.

Javascript에서 볼 수 있는 비동기 스타일은 두 가지의 유형이 있다. ==callbacks== ==promise-style==

## Async callbacks
Async callbacks은 백그라운드에서 코드 실행을 시작할 함수를 호출할 때 인수로 지정된 함수이다. (Async callbacks are functions that are specified as arguments when calling a function which will start executing code in the background.)
==⇒ 이말이 너무 이해가 안된다.==

백그라운드 코드 실행이 끝나면 callback 함수를 호출하여 작업이 완료 됐음을 알리거나, 다음 작업을 실행할 수 있게 함. 단점은 약간 구식인 방식이나 여전히 다른곳에서 사용되고 있다.

참고로 동기적 callback 함수도 있다.

예시)
```js
btn.addEventListener('click', () => {
  alert('You clicked me!');

  let pElem = document.createElement('p');
  pElem.textContent = 'This is a newly-added paragraph.';
  document.body.appendChild(pElem);
});
``` 
위의 예시에서 addEventListner() 'click' 옆의 함수를 Async callback의 예시라고 볼 수 있다. (첫 번째 매개 변수는 이벤트 리스너, 두 번째 매개 변수는 이벤트 리스너가 실행될 때 호출되는 콜백 함수이다.)


예시2)
```js
function loadAsset(url, type, callback) {
  let xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.responseType = type;

  xhr.onload = function() {
    callback(xhr.response);
  };

  xhr.send();
}

function displayImage(blob) {
  let objectURL = URL.createObjectURL(blob);

  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}

loadAsset('coffee.jpg', 'blob', displayImage);
```

위의 코드가 다 이해되지는 않는다. 하지만 현재 이 단계에서 내가 이해하고 넘어가고 싶은 부분은 loadAsset() 함수의 3번째 매개 변수가 콜백함수라는 것이고 loadAsset의 함수가 요청할 때 까지 콜백 함수는 대기한다는 것이다.

**===> 궁금한점 콜백 여러개를 매개변수로 더 추가할 수도 있는지?==**


## Promises
Promises는 최신 Web API에서 볼 수있는 현대적인 비동기 프로그램이다. 좋은  Promises의 사용 예는 fetch() 이다. 


