  
---  
vue router란 컴포넌트들의 조합으로 이루어진 SPA에서 URL이 변경되면 어떤 컴포넌트들이 렌더링 될 지 보여주는 라이브러리이다.  
  
router를 이용하면 변경될 요소에 대해서 페이지를 새로고침하지 않고 컴포넌트를 변경 될 요소로 재 랜더링해서 사용자의 경험을 개선할 수 있다.  
  
* 라우터란? (Router)  
* 라우트란? (Route)  
  
## 1️⃣ vue-router 설치  
```bash  
npm install vue-router  
```  
  
## 2️⃣ 컴포넌트 구조  
router의 기본적인 실습은 아래처럼 공통된 Header 영역은 그대로 두고  
content영역을 “/” 로 접근 시 HomeView.vue를 보여지게 하고 “/about”으로 접근 시 AboutView.vue를 렌더링 해보도록 하겠다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/5fedf0a6-715b-44a4-b5a4-a4b1f3296fc4-image.png)  
  
## 3️⃣ 프로젝트 구조  
```javascript  
src
├── layouts
│   └── TheHeader.vue
│   └── TheView.vue
│
├── views
│   └── HomeView.vue
│   └── AboutView.vue
│
├── router
│   └── index.js // 라우터 정보가 있는 js 파일
│
├── main.js // index.js에서 생성한 router를 import해야 전역적으로 사용 가능하다.  
```  
  
## 4️⃣ 컴포넌트  
Vue Router는 <RouterLink>와 <RouterView>라는 두 가지 컴포넌트를 제공한다.  
* <RouterLink> : 페이지를 새로고침하지 않고 URL변경, 생성, 인코딩 등 기타 기능을 처리  
* <RouterView> : 렌더링할 위치를 Vue Router에게 알려줘서 현재 라우트 컴포넌트를 해당 자리에 렌더링한다.  
  
### HomeView.vue  
```html  
<!-- HomeView.vue -->

<template>
  <div>
    <h2>Home View</h2>
  </div>
</template>

<script setup></script>

<style lang="scss" scoped></style>  
```  
### AboutView.vue  
```html  
<!-- AboutView.vue -->

<template>
  <div>
    <h2>About View</h2>
  </div>
</template>

<script setup></script>

<style lang="scss" scoped></style>  
```  
### TheHeader.vue  
* TheHeader.vue 컴포넌트 안에 “/”, “/about”으로 URL을 변경할 수 있게 <RouterLink> 두 개를 만들었다.   
* to 속성을 통해 변경할 URL을 지정할 수 있다.  
```html  
<!-- TheHeader.vue -->

<template>
  <header>
    <nav>
	    <RouterLink to="/">Home</RouterLink>
	    <RouterLink to="/about">About</RouterLink>
    </nav>
  </header>
</template>

<script setup></script>

<style lang="scss" scoped></style>  
```  
### index.js  
* URL에 따라서 렌더링 route 정보를 정의하고 router를 생성하여 export 한다.  
```javascript  
import { createRouter, createWebHistory } from 'vue-router';

import HomeView from '@/views/HomeView.vue';
import AboutView from '@/views/AboutView.vue';

const routes = [
  {
    path: '/',
    component: HomeView,
  },
  {
    path: '/about',
    component: AboutView,
  },
];

const router = createRouter({
  history: createWebHistory('/'),
  routes,
});

export default router;
  
```  
### main.js  
* index.js에서 export한 router를 import한 뒤 .use(router)를 해주어야 전역적으로 설정한 route 정보를 사용할 수 있다.  
```javascript  
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap-icons/font/bootstrap-icons.css';
import { createApp } from 'vue';
import App from './App.vue';
import router from '@/router';

createApp(App).use(router).mount('#app');

import 'bootstrap/dist/js/bootstrap.js';  
```  
### TheView.vue  
* 이제 TheHeader.vue에서 <RouteLink>를 클릭하면 <RouterView> 자리에 해당 요소들이 렌더링 된다.  
```html  
<!-- TheView.vue -->

<template>
  <main>
    <RouterView></RouterView>
  </main>
</template>

<script setup></script>

<style lang="scss" scoped></style>
  
```  
  
  
  
---  
## 📌 References  
  
  
  
