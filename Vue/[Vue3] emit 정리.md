  
  
### 📌 사용  
💡 PostCreate.vue ( 자식 )  
```html  
<button @click="$emit('createPost', 1, 2, 3, '네번째')" >button</button>  
```  
  
💡 TheView.vue ( 부모 )  
```html  
<template>
	<PostCreate @create-post="createPost"></PostCreate>
</template>

<script setup>
import PostCreate from '@/components/PostCreate.vue';

const createPost = (a, b, c, d) => {
	console.log(a);
	console.log(b);
	console.log(c);
	console.log(d);
};
</script>  
```  
  
결과  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/6675041f-38f7-468d-9110-7bbcc2329037-image.png)  
  
자식 컴포넌트에서 emit 을 사용하는 방법은 위의 방법 이외에도 아래와 같이 여러 방법이 있다.  
```html  
<template>	
	<button @click="createPost">button</button>
</template>


<script setup>
const emit = defineEmits('');
const createPost = () => {
	emit('createPost', 1, 2, 3, '네번째');
};
</script>  
```  
  
### 📌 유효성 체크  
위의 코드에서 const emit = defineEmits(''); 로 선언만 해서 사용했는데 이 부분에 유효성 체크를 추가할 수 있다.  
```html  
<template>
	<input type="text" v-model="title" />
</template>  
```  
```html  
<script setup>
import { ref } from 'vue';

const title = ref('');

const emit = defineEmits({
	// 유효성 검사가 없다면 null로 선언
  createPost: null

  // 유효성 검사가 있다면 검사할 값(title)에 대한 함수 작성
	createPost: title => {
		// 유효성 체크에 걸려도 이벤트가 발생이 되긴 된다. 경고 표시만 나옴
		if (!title.value) {
			console.log('제목을 입력해주세요');
			return false;
		}
		return true;
	},
});

const createPost = () => {
	emit('createPost', title);
};
</script>  
```  
  
유효성 검사 는 작성하지 않아도 정상적으로 emit 이 동작하는 데에는 문제가 없지만 함수의 동작 방식을 더 잘 문서화하기 위해서 모든 이벤트를 정의하는 것이 좋다.  
  
이벤트 이름 관련 공식 문서 참고  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/d0c531ae-3adb-49ce-9ae2-8b93136dd1c1-image.png)  
  
---  
## 📌 References  
  
  
  
  
