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
# 데이터 타입

#### 자바스크립트는 숫자 numnber와 문자열 string그리고 true 혹은 false 중 하나의 값을 갖는 불리언 boolean 이라고 알려진 형식을 말한다.

##### 숫자 데이터 타입 - 숫자 데이터는 숫자를 다루기 위한 데이터 타입이다. 0.75
##### 문자열 데이터 타입 - 문자열 데이터 타입은 글자와 기타 문자들로 구성된다. '안녕하세요 !'
##### 불리언 데이터 타입 - 불리언 데이터 타입은 true, flase중 하나의 값만 가질 수 있다.
* * *
* * *
# 변수에 숫자 저장하기
#### 이번 예제에서는 세 개의 변수를 선언하고 그 변수들에 값을 대입한다.

### 주의할 것은 숫자는 따옴표로 둘러싸지 않는다는 점이다. 
### 일단 변수에 값을 대입하고 나면 변수의 이름을 이용하여 저장된 값을 활용할 수 있다.
### 전체 금액은 각 타일의 가격에 사용자가 사용하고자 하는 전체 타일의 개수를 곱하여 구할 수 있다.

```js
    var price;
    var quantity;
    var total;

    price = 5;
    quantity = 14;
    total = price * quantity;

    var el = document.getElementByld('cost');
    el.textContent = '$' + total;
 ```

##### price 변수는 각 타일의 가격을 저장한다.
##### quantity 변수는 사용자가 원하는 타일의 개수를 저장한다.
##### total 변수는 타일의 전체 가격을 저장한다.
* * *
* * *
# 변수에 문자열 저장하기
##### 먼저 자바스크립트의 처음 4줄의 코드부터 살펴보자.
#####  두개의 변수 (username과 message)를 선언한 후 문자열 값 (사용자의 이름과 표시할 메시지)을 대입했다. 
    
#### 중요한 점은 문자열은 반드시 따옴표로 둘러싸야 한다는 것이다.
#### 작은 따옴표를 사용하든 큰따옴표를 사용하든 관계없이 반드시 같은 따옴표로 둘러싸야 한다.

```js
    var username;
    var message;
    username = '웹지니';
    message = '우리의 변화에 주목하라';

    이 코드는 id 특성 값을 이용하여 두 개의 요소를 선택하고
    그 콘텐츠를 변수에 저장된 값으로 변경한다.

    중요한 점은 문자열은 반드시 따옴표로 둘러싸야 한다는 것이다

    var elName = document.getElementById('name');
    elName.textContent = username;
    var elNote = document.getElementById('note');
    elNote.textContent = message;
 ```

# 문자열 내에서 따옴표 사용하기
##### 간혹 문자열 내부에서 작은따옴표나 큰따옴표를 사용 해야 할 경우가 있다.
##### 하지만 문자열 자체는 작은따옴표나 큰따옴표로 둘러쌀 수 있기 때문에
##### 문자열 내에서 큰따옴표를 사용하고 싶다면 전체 문자열을 작은 따옴표로 둘러싸면 된다.

 ```js
    var title;
    var message;
    title = "웹지니의 특별한 제안";
    message = '<a hre=\ "sale.html\"> 25% 할인! </a>';
    
    var elTitle = document.getElementByld('title');
    elTitle.textContent = title;
    var elNote = document.getElementByld('note');
    elNote.textContent = message;
 ```

```Html
    var title;
    var message;
    title = "웹지니의 특별한 제안";
    message = '<a hre=\ "sale.html\"> 25% 할인! </a>';
    
    var elTitle = document.getElementByld('title');
    elTitle.textContent = title;
    var elNote = document.getElementByld('note');
    elNote.textContent = message;
 ```

# 변수에 불리언 값 저장하기
#### 불리언 변수는 true 혹은 false 중 하나의 값만을 가질 수 있지만 
#### 매우 유용하게 사용되는 데이터 타입이다.

##### 아래 예제를 보면 true또는 false라는 값을 HTML 요소의 class특성 값으로 사용하고 있다.
##### 이 값들은 각기 다른 css 규칙을 해당 요소에 적용하게 한다.

```js
    var inStock;
    var shipping;
    inStock = true;
    shipping = false;
    
    var elStock = document.getElementByld('stock');
    elStock.className = inStock;
    
    var elShip = document.getElementByld('shipping');
    elShip.className = shipping;
 ```









# 변수에 문자열 저장하기
```js
    먼저 자바스크립트의 처음 4줄의 코드부터 살펴보자.
    두개의 변수 (username과 message)를 선언한 후
    문자열 값 (사용자의 이름과 표시할 메시지)을 대입했다.

    var username;
    var message;
    username = '웹지니';
    message = '우리의 변화에 주목하라';

    이 코드는 id 특성 값을 이용하여 두 개의 요소를 선택하고
    그 콘텐츠를 변수에 저장된 값으로 변경한다.

    중요한 점은 문자열은 반드시 따옴표로 둘러싸야 한다는 것이다

    var elName = document.getElementById('name');
    elName.textContent = username;
    var elNote = document.getElementById('note');
    elNote.textContent = message;
 ```

# 변수 사용을 더욱 간편하게
#### 간혹 개발자들은 변수를 생성할 때 더욱 간편한 방법을 사용하기도 한다.
#### 변수를 생성하고 값을 대입하는 방법은 다음과 같이 여러 가지가 존재한다.
```js
    var price = 5;
    var quantity = 14;
    var total = price * quantity;

    var price, quantity, total;
    price = 5;
    quantity = 14;
    total = price * quantity;

    var price = 5, quantity = 14;
    var total = price * quantity;

    // id 특성 값이 cost인 요소에 전체 금액을 출력한다.
    var el = document.getElementByld('cost');
    el.textContent = '$' + total;
 ```