  
## 1️⃣  컴포넌트 정의  
---  
Vue3에서 컴포넌트 는 UI를 재사용 가능하게 하고 독립적인 단위로 나누는 핵심 개념이다. Vue 하나의 최상위 컴포넌트(App.vue)에서 시작되며 여러 개의 하위 컴포넌트로 구성된다.  
  
컴포넌트의 정의는 싱글 파일 컴포넌트(SFC)와 문자열 컴포넌트로 정의하는 두 가지 방법이 있는데 싱글 파일 컴포넌트로 정의하는 것이 일반적이다.  
  
싱글 파일 컴포넌트는 <template> , <script>, <style> 이러한 코드를 갖는 .vue 파일 확장자를 갖는 파일을 의미한다.  
  
  
  
## 2️⃣ 컴포넌트 등록 및 사용  
---  
Vue에서는 컴포넌트를 전역 또는 지역으로 등록해서 사용할 수 있다.  
### 📌 전역  
전역으로 등록된 컴포넌트는 Vue 앱 전체에서 자유롭게 사용이 가능하다.   
```javascript  
// main.js

import { createApp } from 'vue';
import App from './App.vue';
import MyComponent from './MyComponent.vue';

const app = createApp(App);
app.component('MyComponent', MyComponent);
app.mount('#app');  
```  
  
전역적으로 컴포넌트를 등록하게 될 경우, 해당 컴포넌트를 사용하지 않더라도 최종적으로 빌드에 해당 컴포넌트가 추가되게 된다. 이는 사용자가 다운로드 하는 자바스크립트의 파일을 불필요하게 증가 시키게된다. 그리고 소스코드를 볼 때 컴포넌트간의 종속 관계를 파악할 수 어렵다는 특징도 있다.  
### 📌 지역  
```html  
<!-- App.vue -->

<script setup>
import MyComponent from './MyComponent.vue';
</script>

<template>
  <MyComponent />
</template>
  
```  
지역적으로 등록 할 때는 import 를 통해서 해당 파일에서만 사용할 수 있도록 등록 할 수 있다.  
