# 구문 
##### 스크립트는 컴퓨터가 단계별로 수행할 수 있는 일련의 명령이다
##### 이중 각각의 명령이나 단계를 구문 statement 이라고 한다.
##### 구문은 세미콜론 ; 으로 끝나야 한다.

##### 아래의 코드의 실행 결과를 보기전에 다음의 내용을 기억해두자.

```js
    var today = new Date();
    var hourNow = today.getHours();
    var greeting; 
 ```
```js
    if (hourNow > 18) {
        greeting = ‘Good evening’;
    }	else if (hourNow > 12){
        greeting = ‘Good afternoon’;;
    }	else if (hourNow > 0){
        greeting = ‘Good morning’;
    }	else{
        greeting = ‘Welcome’;
    }
        document.write(greeting);
 ```

# 주석
##### 작성한 코드가 어떤 일을 하는지 설명하기 위해 주석 (comments)를 작성해야 한다.
##### 잘 작성된 코드를 읽고 이해하는 데 도움이 되므로 다른 사람이 나의 코드를 더욱 쉽게 읽을 수 있다.

```js
    한줄 주석은 두개의 슬래시 // 다음에 작성한다.
    한 줄 주석은 주로 코드에 짧은 설명을 덧붙일 때 사용한다.


    var today = new Date(); //새로운 날짜 객체를 생성한다
    var hourNow = today.getHours(); //현재 시(Hour)를 구한다
    var greeting; 
 ```

```js
    // 현재 시간에 따라 적당한 인사말을 출력한다.

    여러 줄로 주석 처리를 하려면 
    /* 문자로 시작해서
    if (hourNow > 18) {
        greeting = ‘Good evening’;
    }	else if (hourNow > 12){
        greeting = ‘Good afternoon’;;
    }	else if (hourNow > 0){
        greeting = ‘Good morning’;
    }	else{
        greeting = ‘Welcome’;
    }
        document.write(greeting);
    */ 문자로 끝나는 주석을 사용한다.
    이 두 문자 사이에 존재하는 문자들은 그것이 무엇이든 자바스크립트 해석기가
    처리하지 않는다.
 ```
