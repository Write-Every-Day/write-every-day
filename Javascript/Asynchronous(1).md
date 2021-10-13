# 자바스크립트 비동기(1)
## 1. 비동기/동기
참고 : https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Concepts

동기적 프로그래밍이 일반적으로 순차적으로 코드가 실행되는 것으로 볼 수 있다면 비동기 프로그래밍은 병렬적으로 두 개 이상의 코드가 실행되는 것으로 볼 수 있다.

동기적 프로그래맹의 단점은 현재 실행 중인 프로그램이 끝날 때 까지 다음 프로그램이 기다려야 한다는 것이다. (효율성이 떨어짐)
기다리는 동안 다른 작업을 하자 => 비동기 프로그래밍의 개념

## 2. Blocking code
웹 앱이 브라우저에서 특정 코드를 실행하느라 브라우저에게 제어권을 돌려주지 않으면 브라우저가 마치 정지된 것 처럼 보이는데 => 이러한 현상을 blocking이라고 함.

예시1)
```js
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

다음코드에서 날짜를 천만번 계산이 끝날 때 까지 그 이후의 코드가 실행되지 않음. => 브라우저가 멈춰있다.(blocking 현상)

예시2)
```js
function expensiveOperation() {
  for(let i = 0; i < 1000000; i++) {
    ctx.fillStyle = 'rgba(0,0,255, 0.2)';
    ctx.beginPath();
    ctx.arc(random(0, canvas.width), random(0, canvas.height), 10, degToRad(0), degToRad(360), false);
    ctx.fill()
  }
}

fillBtn.addEventListener('click', expensiveOperation);

alertBtn.addEventListener('click', () =>
  alert('You clicked me!')
);
```

fillBtn 버튼이 눌릴 경우 expensiveOperation() 함수가 끝날 때 까지는 alertBtn을 클릭해도 반응이 일어나지 않는다. => 기본적으로 자바스크립트는 **==Single threaded==** 이기 때문이다. (각 task들이 순차적으로 실행된다는 것)

해결방법으로 Web workers 라는 툴이 있지만 DOM 에 접근할 수 없는 여러가지 한계가 있음.

## 3. Promises
동기적으로 코드를 실행하는 방법에는 위에서 언급했듯이 Web workers라는 자바스크립트에서 사용 가능한 툴이 있다.

하지만, 앞서 언급한 DOM에 접근 할 수 없다는 문제 이외에 다른 문제도 있다.

> Main thread: Task A --> Task B

이 예시에서 예를 들자면 Task B가 Task A의 결과를 return 받아야 하는데 결과를 반환할 시간도 없이 실행되버리면 에러가 발생할 것이다. 

이러한 문제를 해결하기 위해 특정 작업을 비동기적으로 실행하는 것이 필요한데 이때 사용하는 것이 Promises 객체이다.(response를 제공하여 영향을 받을 다음 task에게 반환 될 때 까지 기다리도록 처리하는 것이 필요함)




