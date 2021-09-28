# 비동기 프로그래밍(1)
## 1. 비동기/동기
참고 : https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Concepts

동기적 프로그래밍이 일반적으로 순차적으로 코드가 실행되는 것으로 볼 수 있다면 비동기 프로그래밍은 병렬적으로 두 개 이상의 코드가 실행되는 것으로 볼 수 있다.

동기적 프로그래맹의 단점은 현재 실행 중인 프로그램이 끝날 때 까지 다음 프로그램이 기다려야 한다는 것이다. (효율성이 떨어짐)
기다리는 동안 다른 작업을 하자 => 비동기 프로그래밍의 개념

## 2. Blocking code
웹 앱이 브라우저에서 특정 코드를 실행하느라 브라우저에게 제어권을 돌려주지 않으면 브라우저가 마치 정지된 것 처럼 보이는데 => 이러한 현상을 blocking이라고 함.

예시)
```
const btn = document.querySelector('button');
btn.addEventListener('click', () => {
  let myDate;
  for(let i = 0; i < 10000000; i++) {
    let date = new Date();
    myDate = date
  }

  console.log(myDate);

  let pElem = document.createElement('p');
  pElem.textContent = 'This is a newly-added paragraph.';
  document.body.appendChild(pElem);
});
```

다음코드에서 날짜를 천만번 계산이 끝날 때 까지 그 이후의 코드가 실행되지 않음. 브라우저가 멈춰있다.


