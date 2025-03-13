route를 지정할 때 동적 route를 만들 수도 있고, route에 name을 붙여서 router에서 name을 활용 할 수도 있다.  
  
```javascript  
// index.js

import { createRouter, createWebHistory } from 'vue-router';
import PostDetailView from '@/views/posts/PostDetailView.vue';

const routes = [
  {
    path: '/posts/:id',
    component: PostDetailView,
    name: 'PostDetail',
  }
];

const router = createRouter({
  history: createWebHistory('/'),
  routes,
});

export default router;  
```  
  
이런식으로 route 설정을 할 경우 /posts/1, /posts/2, /posts/3 같은 URL은 모두 같은 PostDetailView.vue라는 컴포넌트로 매핑되게 된다. 파라미터는 콜론 : 으로 표시하면 된다.  
  
ex)  
  
  
그리고 해당 route를 호출하는 router에서 route에서 지정한 name을 가지고도 호출할 수 있다.  
```html  
<template>
	<div class="col-4" v-for="post in posts" :key="post.id">
	  <PostItem
	    :title="post.title"
	    :content="post.content"
	    :created-at="post.createdAt"
	      
@click="goPage(post.id)"  

	  ></PostItem>
	</div>
</template>

<script setup>
import { useRouter } from 'vue-router';
import PostItem from '@/components/posts/PostItem.vue';

const router = useRouter();
const goPage = id => {
	// 객체 형식으로 해당 route 호출 가능하다.
  router.push({
    name: 'PostDetail',
    params: {
      id: id,
    },
  });
};
</script>  
```  
  
router.push 를 사용해서 route를 호출할 때 파라미터는 문자열 또는 위의 방식처럼 객체가 될 수 있다.  
```javascript  
// 리터럴 문자열 경로
router.push('/users/eduardo')

// 경로가 있는 객체
router.push({ path: '/users/eduardo' })

// 이름을 가지는 라우트
router.push({ name: 'user', params: { username: 'eduardo' } })

// 쿼리와 함께 사용, 결과적으로 /register?plan=private가 된다
router.push({ path: '/register', query: { plan: 'private' } })

// 해시와 함께 사용, 결과적으로 /about#team가 된다
router.push({ path: '/about', hash: '#team' })  
```  
  
---  
## 📌 References  
  
  
  
