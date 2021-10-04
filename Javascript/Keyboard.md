# Keyboard 개발일지

## Sat Oct 2, 2021
마크업 시 클래스 이름 명명을 위해 BEM 방법론을 알게 되었고, 이 방법론에 따라 클래스 이름을 지어봤다.

### BEM 기본 구조 공부(CSS 클래스 네임을 짓기 위한 방법론)
- Block, Element, Modifier 의 순서로 클래스 이름을 짓는다.
- 기본적으로 ID를 사용하지 않으며, class만을 사용함


## Mon Oct 4, 2021
## keyboard 입력 시 입력 값 출력 기능 추가

window.addEventListner("keydown") 이벤트를 사용하여 입력 값을 받아와 DOM에 출력

## Keyboard press한 자판에 효과 추가
입력 받은 키보드 값과 동일한 id를 가진 엘리먼트에 keypressed라는 클래스를 추가하여 자판에 효과를 주도록 함

## 소스 코드 참고
```
window.addEventListener("keydown",  (e)=>{

const  key=document.getElementById(e.key);

if(key) {key.classList.add("keypressed");

const  typed__content  = document.querySelector('.typed__content');

typed__content.innerHTML+=e.key;

//console.log(e.key);

}

})
```

