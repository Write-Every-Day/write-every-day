# 구문 
##### 스크립트는 컴퓨터가 단계별로 수행할 수 있는 일련의 명령이다 이중 각각의 명령이나 단계를 구문 statement 이라고 하고, 구문은 세미콜론 ; 으로 끝나야 한다. 아래의 코드의 실행 결과를 보기전에 다음의 내용을 기억해두자.
***
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
***
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
 ***
# 데이터 타입

#### 자바스크립트는 숫자 numnber와 문자열 string그리고 true 혹은 false 중 하나의 값을 갖는 불리언 boolean 이라고 알려진 형식을 말한다.

##### 숫자 데이터 타입 - 숫자 데이터는 숫자를 다루기 위한 데이터 타입이다. 0.75
##### 문자열 데이터 타입 - 문자열 데이터 타입은 글자와 기타 문자들로 구성된다. '안녕하세요 !'
##### 불리언 데이터 타입 - 불리언 데이터 타입은 true, flase중 하나의 값만 가질 수 있다.
* * *

# 변수에 숫자 저장하기
#### 이번 예제에서는 세 개의 변수를 선언하고 그 변수들에 값을 대입한다.

##### 주의할 것은 숫자는 따옴표로 둘러싸지 않는다는 점이다. 
##### 일단 변수에 값을 대입하고 나면 변수의 이름을 이용하여 저장된 값을 활용할 수 있다.
##### 전체 금액은 각 타일의 가격에 사용자가 사용하고자 하는 전체 타일의 개수를 곱하여 구할 수 있다.
***
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

# 변수에 문자열 저장하기
##### 먼저 자바스크립트의 처음 4줄의 코드부터 살펴보자.
#####  두개의 변수 (username과 message)를 선언한 후 문자열 값 (사용자의 이름과 표시할 메시지)을 대입했다. 
##### 중요한 점은 문자열은 반드시 따옴표로 둘러싸야 한다는 것이다.
##### 작은 따옴표를 사용하든 큰따옴표를 사용하든 관계없이 반드시 같은 따옴표로 둘러싸야 한다.
***
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
***
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
***
# 변수에 불리언 값 저장하기
#### 불리언 변수는 true 혹은 false 중 하나의 값만을 가질 수 있지만 
#### 매우 유용하게 사용되는 데이터 타입이다.
##### 아래 예제를 보면 true또는 false라는 값을 HTML 요소의 class특성 값으로 사용하고 있다.
##### 이 값들은 각기 다른 css 규칙을 해당 요소에 적용하게 한다.
***
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
***
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
***
# 변수의 값 변경하기
##### 일단 변수에 값을 대입한 후에는 같은 방식으로 변수에 저장된 값을 변경할 수 있다.
##### 한번 생성된 변수에 새로운 값을 대입할 떄에는 var 키워드를 다시 사용할 필요는 없다.
##### 단지 변수의 이름과 등호 기호 (이제는 등호 기호가 대입 연산자라는 것을 알고 있으리라 믿는다.)
##### 그리고 그 변수에 저장될 새로운 값만 나열하면 된다.
***
```js
    var inStock;
    var shipping;

    inStock = true;
    shipping = false;

    /* 결과에 따라 변수 값을 변경하는 다른 작업들을 이 부분에 작성한다. */
    inStock = false;
    shipping = true;

    var elStock = document.getElementByld('stock');
    elStock.className = inStock;
    var elship = document.getElementByld('shipping');
    elShip.className = shipping;
 ```
##### 예를 들어, shipping 변수의 초깃값이 false인 경우를 생각해보면,
##### 어떤 스크립트를 실행하고 나닌 제품의 배송이 가능해져서 이 변수 값을 true로 바꾸어야 할 수도 있다.
##### 위에 코드에서는 단지 두 변수 값을 true에서 flase로, 그리고 flase에서 true로 바꾸어주었다.
***
# 이쁜 변수 이름을 만들기 위한 규칙들
##### 변수에 이쁘게 이름을 지어주고 싶다면 규칙을 잘 따라야 한다.
- ##### 변수의 이름은 반드시 문자, 달러기호($) 혹은 언더스코어(_)문자로 시작해야 한다. 절대로 숫자로 시작해서는 안된다.
- ##### 변수의 이름은 문자, 숫자, 달러기호, 언더스코어 문자를 포함할 수 있다. 그러나 대시(-)나 마침표(.) 같은 기호들은 사용할 수 없다는 점을 명심하자.
- ##### 키워드나 예약어는 변수 이름으로 사용할 수 없다. 키워드는 자바스크립트 해석기가 미리 정해진 행동을 하도록 사전에 정해둔 특별한 단어이다. 예를 들어 var 키워드는 변수를 선언하기 위해 사용되는 단어이다. 예약어는 자바스크립트가 향후 버전에서 사용할 것을 대비하여 미리 확보해둔 단어를 말한다.
- ##### 모든 변수는 대/소문자를 구분하므로 score와 Score는 각기 다른 변수의 이름으로 사용할 수 있다. 그러나 단지 대/소문자만 다른 동일한 이름의 변수를 만드는 것은 그리 좋은 습관이라고 할 수 없다.
- ##### 변수가 저장할 정보의 종류를 잘 표현하는 단어를 사용하자. 예를들어, 사람의 이름을 저장할 변수는 firstNmae, 성을 저장할 변수는 lastName, 그리고 나이를 저장할 변수는 age라는 이름을 사용한다.
- ##### 변수의 이름에 두 개 혹은 그 이상의 단어를 사용하게 될 경우에는 첫 번째 단어를 제외한 나머지 단어의 첫 글자는 대문자를 사용하도록 하자. 예를 들어,firstname 대신 firstName이라는 이름을 사용한다. (이를 캐멀케이스 표기법이라고도 한다). 또한 두 단어를 언더스코어 문자로 연결해도 된다(하지만 대시는 사용할 수 없다.)
***
# 배열 - 배열은 특별한 종류의 변수이다. 이 변수는 하나의 값이 아니라 값의 목록을 저장한다.

##### 만일 목록이나 서로 관련된 일련의 값을 다루어야 한다면 배열의 사용을 고려해보아야 한다. 배열은 얼마나 많은 값의 목록을 처리해야 할지 알 수없을 떄 특히 유용하다. 그 이유는 배열을 생성할 떄 몇 개의 데이터를 처리할 것인지 미리 지정할 필요가 없기 때문이다.
##### 몇개의 데이터를 다룰지 정확히 모른다는 이유로 엄청나게 많은 변수를 생성해두는 것보다는 배열을 사용하는 것이 훨씬 효율적인 방법이다. 예를 들어, 배열은 장보기 목록의 개별 아이템처럼 서로 연관된 아이템들을 저장하기에 적합하다. 게다가 장보기 목록은 매번 만들때마다 그 내용이 달라질 것이다. 곧 알게 되겠지만 배열에 저장된 값들은 쉼표로 구분한다.
***
# 배열 만들기 
```js
    var colors;
    colors = ['white', 'black', 'custom'];

    var el = document.getElementByld('colors');
    el.textContent = colors[0];
 ```
 ##### 배열에 값을 대입할 때는 대괄호[] 안에 쉼표로 구분하여 값을 나열하면 된다. 배열의 값은 반드시 같은 데이터 타입일 필요는 없으므로 문자열과 숫자, 불리언 값 등을 하나의 배열에 담을 수도 있다. 이와 같이 배열을 생성하는 방법을 배열식 이라고 하며, 보편적으로 사용되는 방법이다. 배열의 값들은 한줄에 하나씩 나열해도 무방하다.
```js
    var colors = new Array('white',
                           'black',
                           'custom');

    var el = document.getElementByld('colors');
    el.textContent = colors[0];
```
##### 위의 예제와 같이 배열 생성자 (array constructor)를 이용하여 배열을 만들수도 있다. 이 경우에는 new 키워드와 Array(); 생성자를 사용한다. 그리고 배열의 값들은 괄호 안에 쉼표로 구분하여 나열하면 된다 (이 경우는 대괄호를 사용하지 않는다). 또한 items() 메서드를 호출하여 배열내의 데이터를 조회할 수 있다.
