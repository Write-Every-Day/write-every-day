## 컴포넌트 간 데이터 전달
컴포넌트 간 데이터를 주고받으려면 emit과 props를 사용하면 된다. 컴포넌트는 상하 관계로 되어있는데, 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하려면 `props`를 사용하고 반대로 자식 컴포넌트에서 부모 컴포넌트로 데이터를 전달하려면 `emit`을 사용하면된다.

그럼 자식 컴포넌트 사이의 데이터를 직접 주고받을 수 있을까? JQuery 같이 DOM 조작을 직접 하지 않는 이상 Vue에서는 제공하지 않는다. 따라서 자식 컴포넌트에서 먼저 부모 컴포넌트로 데이터를 전달한다음, 데이터를 다시 자식 컴포넌트로 내려주는 방법으로 공유할 수 있다.


### props
```html
<!-- App.vue (Root Component)-->
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <PBHeader></PBHeader>
  <PBMain></PBMain>
  <PBPopup></PBPopup>
  <PBFooter></PBFooter>
</template>
```
위와 같은 컴포넌트 구조에서 root component인 App.vue에서 PBMain이라는 자식 컴포넌트로 데이터를 전달하려면 아래와 같은 방법으로 전달하면 된다.

1. 부모 컴포넌트에서 전달할 자식 컴포넌트 태그에 `props` 키를 bind시킨다.
    ```html
    <!--📁 Parent Component-->
    <template>
        <PBMain v-bind:title="title"></PBMain>
    </template>
    <script>
    import PBMain from './components/PBMain.vue'
    export default {
        name: 'App',
        data: ()=>{
            return {
            title : "제목이에요~"
            }
        },
        components: {
            PBMain
        }
    }
    </script>
    ```

2. 자식 컴포넌트에서 `props`속성으로 부모 컴포넌트에서 bind 속성을 배열에 추가한다.
    ```js
    export default {
        props: ["title"]
    }
    ```
3. 템플릿에서 쌍괄호로 data에 접근하듯이 `props`속성에 접근할 수 있다.
   ```html
   <!--📁 Children Component-->
    <template>
            <main v-bind:class="{popup : popupFlag}">
                <h2 class="hide">Recent POST</h2>
                <ol class="list">
                    <li class="post-list">
                        <details>
                            <summary>{{title}}</summary>
                            <p>내용</p>
                        </details>
                        <div class="post-info">
                            <p><span class="author">admin</span> | <span class="date">2021. 09. 23</span></p>
                        </div>
                    </li>
                </ol>
            </main>
    </template>
   ```

### emit
하위 컴포넌트에서 상위 컴포넌트로 데이터 전달 시 `emit`을 사용해 이벤트를 전달할 수 있다.
1. 자식 컴포넌트에서 부모에 전달할 이벤트를 `$emit`키워드로 명시한다.
    ```html
    <!--📁 Children Component-->
    <template>
            <header v-bind:class="{popup : popupFlag}">
                <a href="#">
                    <i class="fas fa-4x fa-envelope"></i>
                    <h1>Welcome to VUE-POST BOX.</h1>
                    <strong>write your message.</strong>
                </a>
                <button type="button" @click="togglePopup">Write</button>
            </header>
    </template>

    <script>
    export default {
        data() {
            return {
                popup : true,
            }
        },
        props: ["popupFlag"],
        methods:{
            //'Write'버튼을 누를 시 'popStauts'이벤트를 부모에 전달한다.
            togglePopup(){ 
                this.$emit('popStatus');
            }
        }
    }
    </script>
    ```
2. 부모 컴포넌트에서 트리거된 이벤트를 받아 메서드 함수를 실행시킨다. 
   (자식의 `popStatus`이벤트가 트리거되면 togglePopup 메서드 실행)
    ```html
    <!--📁 Parent Component-->
    <template>
        <PBHeader v-on:popStatus="togglePopup"></PBHeader>
    </template>
    <script>
    import PBMain from './components/PBMain.vue'
    export default {
        name: 'App',
        data: ()=>{
            return {
                popup : false,
            }
        },
        components: {
            PBMain
        },
        methods: {
            togglePopup(){
                return this.popup = !this.popup; //header 컴포넌트의 팝업 상태에 따라 popup 상태 값 변경
            }
        },
    }
    </script>
    ```