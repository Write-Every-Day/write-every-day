## Vue 컴포넌트
화면(view)의 영역을 분리하여 개발, 최상위 컴포넌트는 `root`이다.

### 전역 컴포넌트
주로 플러그인, 라이브러리 등 전역으로 사용 할 공통 컴포넌트를 지칭한다.

`~vue 2`버전까지는 **new Vue** 생성자 형식로 전역 컴포넌트 인스턴스를 생성했지만, `vue3`버전부터는 createApp 메서드로 생성 방식이 바뀌었다. (코드는 상당히 직관적이다.)
```js
//vue 2.X
const myComponent = Vue.component("app-header", {
    template: "<h1>header</h1>", //해당 컴포넌트에 들어갈 태그
});

new Vue({
  el: '#app',
  render: h => h(myComponent),
});
```
vue3에서는 독립 인스턴스 형태로 컴포넌트를 구성하기 때문에 모듈 분리가 편하다. (vue2에서는 거의 모든 속성을 서로 공유하는 문제가 있어 분리가 어려움)
```js
//vue 3
import myComponent from "./App.vue";
createApp(myComponent).mount("#main");
```
```js
import { createApp } from "vue";
import globalUtil from './modules/util.js';
import headerComponent from "./components/header.vue";
const config = {/* 전역 설정 객체*/}
const componentOne = Vue.createApp(config);
componentOne.compnent('header-component', headerComponent);
componentOne.use(globalutil);

const componentTwo = Vue.createApp(config);
componentOne.use(globalutil);
```

### 지역 컴포넌트
vue 인스턴스 내에 직접 삽입하는 방식으로 `components` 속성에 정의한다. [Vue스타일가이드](https://v3.ko.vuejs.org/style-guide/)에 따르면 컴포넌트 네이밍 규칙은 케밥케이스 또는 카멜케이스로 작성하는 것을 권고한다. 또한 부모의 컴포넌트 내 하위 컴포넌트를 작성할 때에도 부모 컴포넌트의 이름을 따르는 것을 권장한다. (컴포넌트 파일의 순서 유지를 위함)
```html
<!--PostBox.vue -->
<template>
  <post-box-header></post-box-header>
  <post-box-list></post-box-list>
  <post-box-footer></post-box-footer>
</template>
<script>
import PostBoxHeader from './components/PBHeader.vue'
import PostBoxList from './components/PostBoxList.vue'
import PostBoxFooter from './components/PostBoxFooter.vue'
export default {
  name: 'PostBox',
  components: {
    PBHeader,
    PostBoxList,
    PostBoxFooter
  }
}
</script>
<style>
</style>
```