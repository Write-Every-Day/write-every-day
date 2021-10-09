## 디렉티브 문법
말 그대로 '지시어'라는 개념이다. `vue`내에서 HTML 엘리먼트 속성들을 제어하는 역할을 한다.
이벤트들을 트리거 시키거나, 요소 렌더링, css 스타일 제어 등 화면 동작들을 제어한다.

### Data Binding
머스타치 태그 `{{}}`를 이용해 데이터를 출력한다.
```html
    <template>
        <div>{{message}}</div>
    </template>
    <script>
    export default {
        data() {
            return {
                message : 'hello Vue!'
            }
        },
    }
    </script>
```

### computed
`data`내에 정의된 속성의 변화가 감지하면 바로 해당 속성을 계산한 값을 바로 적용시킬 수 있다. **반드시 return을 해주어야한다.**
```html
    <template>
        <div id="app">
            <p> {{ formatDate }} </p> <!--2021년 10월 08일 -->
        </div>
    </template>
    <script>
    export default {
        data(){
            return {
                today : '20211008'
            }
        },
        computed : {
            formatDate : function(){
                return `${date.substr(0,4)}년 ${date.substr(4,2)}월 ${date.substr(6,2)}일`;
            }
        }
    }
    </script>
```

### watch
`data`내에 정의된 속성의 변화가 감지하면 실행시키는 함수이다. 변화를 감지한다는 점에서 `computed`와 상당히 용도가 유사한데 `computed`는 직접 template내에 출력을 해야 메서드가 실행되지만, watch는 값의 변화에 따라 메서드가 실행된다. 
(매번 실행하기에 부담스러운 로직이나 API 요청시는 Watch, 단순한 값 계산에 대해서는 웬만하면 `computed`를 사용하자.)
```html
    <template>
        <div id="app">
            <input type="button" value="increase" v-on:click="increaseNumber">
            <p id="log">{{ number }}</p>
        </div>
    </template>
    <script>
    export default{
        data(){
            return {
                number : 0
            }
        },
        watch: {
            number: function(){
                if(this.number >= 10){
                    alert("10 이상 입니다!");
                }
            }
        },
        methods:{
            increaseNumber: function(){
                return this.number++;
            }
        }
    }
    </script>
```

### v-bind
화면의 속성(HTML 표준 속성)지정 또는 props(데이터 전달) 목적으로 사용한다. 
화면의 id, 클래스 및 여러 attribute들을 동적으로 지정할 수 있다.
```html
    <template>
        <div id="app">
            <p v-bind:id="elementId" v-bind:class="elementClass">{{ blueText }}</p>
        </div>
    </template>
    <script>
    export default{
        data(){
            return {
                blueText : 'blue font'
                elementId : 'text-area',
                elementClass : 'blue-text'
            }
        }
    }
    </script>
    <style>
        .blue-text {
            color : blue;
        }
    </style>
```

### Dynamic Binding
특정 `data`가 조건에 충족해야 클래스를 동적으로 bind시킬 경우 예제
```html
<!--existData 가 true 일 경우에만 text-blue 클래스 지정-->
<p id="text" v-bind:class="{text-blue : existData}">message</p>
```

### v-if
특정 상황에 따라 요소들을 보여준다. (`v-else`상태이면 해당 요소를 remove 시킨다.)
```html
    <template>
        <div v-if="existData">
            <h2>Exist Data!!</h2>
        </div>
        <div v-else>
            <h2>No Data!!</h2>
        </div>
    </template>
```

### v-show
`v-if`와 마찬가지로 특정 데이터에 따라 요소를 출력시킨다. (차이점은 스타일적으로 `display:none`으로 요소를 숨긴다.)

### v-html
html자체를 화면에 출력시키고 싶을 때 사용한다.
```html
    <template>
        <div v-html="content"></div>
    </template>
    <script>
    export default{
        data(){
            return {
                content: '<p>hello vue!!</p>',
            }
        }
    }
    </script>
```