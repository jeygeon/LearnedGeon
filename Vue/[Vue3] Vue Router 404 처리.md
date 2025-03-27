지정된 페이지가 아닌 경로에 사용자가 접근 했을 시에 보여주는 404페이지를 만들어보려고 한다.  
  
Router를 정의해 놓은 js 파일에 아래와 같은 경로를 추가한다.  
```javascript  
// route.js

{
  path: '/:NotFound(.*)*',
  component: NotFoundView,  // 보여줄 컴포넌트
  name: 'NotFound',
},  
```  
  
path: '/:NotFound(.*)*'  에 대해서 간단히 설명하자면   
* NotFound → 단순한 변수 명 ( 실제 동작에는 영향을 주지 않는다. )  
* .* → 모든 경로를 매칭하는 정규 표현식  
* * → 0개 이상의 경로 세그먼트를 포함 가능  
* 따라서 이 경로는 정의되지 않은 모든 경로를 NotFoundView로 리다이렉트 한다.  
  
```javascript  
// NotFoundView.vue

<template>
  <div class="text-center py5">
    <h1>Oops!</h1>
    <h2>404 Not Found</h2>
    <div class="text-muted">요청한 페이지를 찾을 수 없습니다.</div>
    <div class="mt-4">
      <RouterLink to="/">
        <button class="btn btn-primary">HOME</button>
      </RouterLink>
    </div>
  </div>
</template>

<script setup></script>

<style lang="scss" scoped></style>  
```  
  
---  
## 📌 References  
  
  
  
