  
---  
props 는 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하는 방식을 말한다.  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/075be2eb-93e2-4d6d-9c2f-965b69cc14a1-image.png)  
  
TheView.vue 라는 부모 컴포넌트에서 각각의 AppCard.vue 라는 자식 컴포넌트로 데이터를 전달하는 상황을 가정해보자  
  
props 를 전달하는 방식은 여러가지가 있다.  
  
## 1️⃣ 부모 컴포넌트 ( TheView.vue )  
---  
### 1. 정적 데이터 전달  
```html  
<script setup>
import AppCard from '@/components/AppCard.vue';
</script>  
```  
```html  
<AppCard title="제목1" content="내용1"></AppCard>  
```  
  
<AppCard> 컴포넌트에 title="제목1 과 content="내용1" 을 직접 전달한다.  
  
### 2. 동적 데이터 전달  
```html  
<script setup>
import AppCard from '@/components/AppCard.vue';
import { reactive } from 'vue';

const post = reactive({
	title: '제목2',
	content: '내용2',
});
</script>  
```  
```html  
<AppCard :title="post.title" :content="post.content"></AppCard>  
```  
이런식으로 : (콜론)을 사용하여 데이터 바인딩(v-bind:)을 적용해서 post 객체의 값을 props 로 전달할 수도 있다.  
  
만약 <AppCard> 컴포넌트가 여러 개일 경우 v-for 디렉티브를 통해 아래와 같이 표현 할 수도 있다.  
```html  
<div v-for="(post, index) in posts" :key="index">
  <AppCard
    :title="post.title"
    :content="post.content"
  ></AppCard>
</div>
  
```  
  
## 2️⃣ 자식 컴포넌트 ( AppCard.vue )  
---  
TheView.vue 에서 전달 받은 props 를 자식 컴포넌트에서 표현할 때는 아래와 같이 표현 가능하다.  
  
```html  
<template>
	<div>
		<h5>{{ title }}</h5>
		<p>{{ content }}</p>
	</div>
</template>  
```  
  
Vue 컴포넌트는 외부에서 컴포넌트로 props 를 넘길 때 어떤 속성이 처리 되어야 하는지 알기 위해 명시적인 props 선언을 아래와 같이 요구한다.  
```javascript  
// <script setup>에서는 defineProps()를 통해 props 선언이 가능하다.

defineProps({
	title: { type: String, required: true },
	content: { type: String, required: true }
});  
```  
  
### 1. 컴포넌트 속성 정의  
 props 를 정의할 때 사용할 수 있는 속성들은 아래와 같다.  
1. type (데이터 타입 지정)  
1. required (필수 여부)  
1. default (기본값 설정)  
1. validator (유효성 검사)  
required , default , validator 속성이 조합된 props 조합 예제  
```javascript  
defineProps({
  title: { type: String, required: true }, // 필수 값
  count: { type: Number, default: 0 },     // 기본값 설정
  status: {
    type: String,
    default: 'pending',
    validator: (value) => ['pending', 'success', 'error'].includes(value) // 유효성 검사
  }
});  
```  
  
## 3️⃣ 단방향 데이터 흐름  
---  
  
  
  
  
