부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달할 때는 props를 통해서 전달이 가능하다. 그렇다면 만약 전달해야하는 데이터가 아래와 같이 컴포넌트 간에 Deep가 깊어지면 어떻게 전달해야 할까  
![이미지 출처 : https://ko.vuejs.org/guide/components/provide-inject.html](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/8fb26541-cdd9-4bdd-9935-12328750848f-image.png)  
  
이럴때 provide를 통해 전달이 가능하다. 부모 컴포넌트에서 provide로 정의된 데이터에 대해서는 자식 컴포넌트나 그 하위 컴포넌트까지도 모두 inejct를 통해서 접근이 가능하다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/2c4b7762-9bf8-4a17-9ff4-2be4b0759f75-image.png)  
  
최상위 부모 컴포넌트인 TheView.vue에서 provide(’변수 명’, 전달할 인자)를 정의하기만 하면 그 하위 컴포넌트에서는 inject(’변수 명’)을 통해서 해당 데이터에 접근할 수 있다.  
  
```html  
<!-- TheView.vue -->

<template>
	<div class="contaier py-4">
		<div class="card">
			<div class="card-header">ProvideInject Component</div>
			<div class="card-body">
				<Child></Child>
			</div>
		</div>
	</div>
</template>

<script setup>
import { provide, ref } from 'vue';
import Child from './Child.vue';

const staticMessage = 'static message';
const count = ref(10);
provide('static-message', staticMessage);
provide('count', count);
</script>  
```  
  
```html  
<!-- Child.vue -->

<template>
	<div class="card">
		<div class="card-header">Child Component</div>
		<div class="card-body">
			<DeepChild></DeepChild>
		</div>
	</div>
</template>

<script setup>
import DeepChild from './DeepChild.vue';
</script>  
```  
  
```html  
<!-- DeepChild.vue -->

<template>
	<div class="card">
		<div class="card-header">Deep Child Component</div>
		<div class="card-body">
			<P>staticMessage : {{ staticMessage }}</P>
			<P>count : {{ count }}</P>
		</div>
	</div>
</template>

<script setup>
import { inject } from 'vue';

const staticMessage = inject('static-message');
const count = inject('count');
</script>  
```  
  
TheView.vue에서는 message를 provide하지 않았는데 그럴 때 하위 컴포넌트에서 만약 전달받지 못한 데이터라면 두 번째 매개변수로 디폴트 값을 정해서 사용할 수 있다.  
```javascript  
// 전달받지 못한 인자라면 두 번째 매개변수로 디폴트 값을 정할 수 있다.
const message = inject('message', 'default message');  
```  
  
그리고 반응형 데이터로 만들어서 전달해야 할 경우가 있을 경우 provide 하는 컴포넌트에서 모든 변경사항을 제공해야 향후 유지관리 측면에서 좋다.  
```html  
<script setup>
import { ref, provide } from 'vue';

const pole = ref('남극');
const updatePole = () => {
	pole.value = '북극'
};

provide('location', { pole, updatePole }); 
</script>  
```  
```html  
<script setup>
import { inject } from 'vue';

const { pole, updatePole } = inject('location');
</script>  
```  
  
  
---  
## 📌 References  
  
  
