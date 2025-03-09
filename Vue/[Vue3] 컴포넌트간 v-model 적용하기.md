반응형 변수에 v-model 을 적용하여 <template> 태그 내에서 입력 요소의 값과 자바스크립트 값의 데이터를 양방향 바인딩 할 수 있다.  
  
이것과 마찬가지로 부모 컴포넌트와 자식 컴포넌트간에도 v-model 을 통해서 양방향 데이터 바인딩을 할 수 있다.  
  
```html  
<!-- MyComponent.vue : 자식 컴포넌트 -->

<template>
	<div>
		<input
			type="text"
			:value="modelValue"
			@input="$emit('update:modelValue', $event.target.value)"
		/>
	</div>
</template>

<script setup>
defineProps({
	modelValue: {
		type: String,
	},
});
defineEmits(['update:modelValue']);
</script>  
```  
  
```html  
<!-- App.vue : 부모 컴포넌트 -->
<MyComponent v-model="userName" />  
```  
이 경우 Vue는 자동으로 아래와 같이 동작하게 된다.  
```html  
<MyComponent :modelValue="userName" @update:modelValue="userName = $event" />  
```  
  
### 정리  
1. v-model 은 기본 props 로 modelValue 를 사용한다.  
1. 부모에서 입력한 값(username이라는 props)을 자식으로 전달한다.  
1. 자식에서는 기본 props인 modelValue를 <script setup> 태그 내에 정의 한다. 부모로 이벤트 전달을 위한 emit도 같이 정의한다. 이름은 update:modelValue  
1. 자식에서 입력한 값을 부모로 전달한다 @input="$emit('update:modelValue', $event.target.value)"  
1. 부모에서는 :modelValue=”userName” 이 부분으로 전달받은 값을 바인딩 한다.  
  
v-model은 기본 props로 modelValue를 사용한다고 했는데, 물론 다른 이름도 사용이 가능하다. 이 경우에는 이름을 명시해 주면 된다. (ex username으로 변수 명을 변경했을 때의 예시 코드)  
```html  
<!-- MyComponent.vue : 자식 컴포넌트 -->

<template>
	<div>
		<input
			type="text"
			:value="username"
			@input="$emit('update:username', $event.target.value)"
		/>
	</div>
</template>

<script setup>
defineProps({
	username: {
		type: String,
	},
});
defineEmits(['update:username']);
</script>  
```  
  
modelValue 말고 다른 변수명을 사용하고 싶을 때는 이런식으로 v-model 뒤에 :변수명을 붙이면 된다.  
```html  
<!-- App.vue : 부모 컴포넌트 -->
<MyComponent v-model:username="userName" />  
```  
이 경우 Vue는 자동으로 아래와 같이 동작하게 된다.  
```html  
<MyComponent :username="userName" @update:username="userName = $event" />  
```  
  
  
  
---  
## 📌 References  
  
  
  
  
