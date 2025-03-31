  
  
Vue3 프로젝트를 만들면서 Vuetify까지 설치해서 사용하는 방법을 정리해보겠다.   
Vuetify란 구글의 Material Design가이드라인을 기반으로 스타일과 컴포넌트를 제공하여 UI를 보다 쉽게 만들 수 있다.  
```plain text  
💡 Material Design이란?

구글에서 만든 디자인 시스템으로, 웹과 모바일 애플리케이션의 사용자 경험을 향상하기 위해 설계되었다.
이 시스템은 UI의 일관성, 직관성, 그리고 미적인 부분에 집중한다.  
```  
  
### 1️⃣ 프로젝트 생성  
(나는 참고로 3.1.10 버전의 vue 프로젝트를 만들었다.)  
```bash  
npm create vue@3.1.10 vue3-vuetify
cd vue3-vuetify
npm install  
```  
  
### 2️⃣ Vuetify 설치  
프로젝트에서 Vuetify와 관련된 종속성을 설치한다.  
```bash  
npm install vuetify@next @mdi/font  
```  
* vuetify@next → Vuetify 3의 최신 버전을 설치  
* @mdi/font → Material Design Icons 폰트 패키지이며, 아이콘 사용을 위해 설치  
  
### 3️⃣ Vuetify 설정  
1. Vuetify를 프로젝트에서 사용하기 위해 설정 파일을 만들고, 필요한 구성을 추가한다.  
src 디렉토리 안에 plugins 디렉토리를 만들고, 그 안에 vuetify.js 파일을 생성한다.  
```javascript  
// src/plugins/vuetify.js

import { createVuetify } from "vuetify";
import "vuetify/styles";
import { aliases, mdi } from "vuetify/iconsets/mdi";
import * as components from "vuetify/components";
import * as directives from "vuetify/directives";

const vuetify = createVuetify({
  components,
  directives,
  icons: {
    defaultSet: "mdi",
    aliases,
    sets: {
      mdi,
    },
  },
});

export default vuetify;  
```  
  
1. main.js에서 Vuetify을 불러와서 적용  
```javascript  
// src/main.js

import { createApp } from "vue";
import App from "./App.vue";
import vuetify from "./plugins/vuetify";
import "@mdi/font/css/materialdesignicons.css";

createApp(App).use(vuetify).mount("#app");  
```  
  
###  4️⃣ 테스트  
간단한 버튼을 하나 생성해보도록 하겠다.  
```html  
<!-- src/App.vue -->

<template>
  <v-btn color="primary">Hello, Vuetify!</v-btn>
</template>

<script setup></script>

<style>
@import "vuetify/styles";
</style>
  
```  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/fc2d47e9-911b-47af-9646-a8a354aa5298-image.png)  
  
---  
### 📌 References  
  
