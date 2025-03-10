  
  
Non-Prop 속성이란 부모 컴포넌트에서 자식 컴포넌트로 prop이나 event를 상속할 때 자식 컴포넌트에서 명시적으로 선언되지 않는 속성을 의미한다.  
  
### 📌 요소 상속  
---  
```html  
<!-- MyButton.vue : 자식 컴포넌트 -->

<template>
	<button class="btn btn-primary" type="button">My Button</button>
</template>

<script setup></script>

<style lang="scss" scoped></style>
  
```  
  
```html  
<!-- TheView.vue : 부모 컴포넌트 -->

<template>
	<MyButton class="hello-btn" id="hello-btn" @click="sayHello"></MyButton>
</template>

<script setup>
import MyButton from './MyButton.vue';

const sayHello = () => {
	alert('안녕하세요');
};
</script>

<style lang="scss" scoped></style>  
```  
  
이런식으로 버튼을 클릭했을 때 안녕하세요 라는 alert창을 띄어주는 컴포넌트가 있다고 가정했을 때 부모 컴포넌트에서 class=”hello-btn”, id=”hello-btn”, @click=”sayHello” 라는 속성과 이벤트를 자식 컴포넌트로 상속 해주었다.  
  
그리고 이미 자식 컴포넌트 buttton 에는 btn, btn-primary 라는 클래스가 이미 존재하는 상태이다.  
  
이 경우 최종적으로 렌더링은 아래와 같이 된다.  
```html  
<button type="button" class="btn btn-primary hello-btn" id="hello-btn"></button>  
```  
그리고 해당 버튼에는 클릭했을 때 안녕하세요라는 alert을 띄우는 sayHello 라는 이벤트 리스너가 적용되어 있을 것 이다.  
  
이렇게 자식 컴포넌트에서 props로 명시하지 않아도 상속받는 id, class, event 들을 Non-props 속성이라고 하는데  
  
문제는 속성 상속은 자식 컴포넌트의 Root 요소에 속성이 자동으로 추가된다는 것이다.  
  
### 📌 부모 요소가 변경될 때  
---  
부모 컴포넌트는 그대로인 상태에서 만약 자식 컴포넌트가 아래와 같이 Root 요소가 button이 아니라 div로 바뀌게 된다면  
```html  
<!-- MyButton.vue : Root 요소가 변경된 자식 컴포넌트 -->

<template>
	<div>
		<button class="btn btn-primary" type="button">My Button</button>
	</div>
</template>

<script setup></script>

<style lang="scss" scoped></style>
  
```  
  
최종적으로 렌더링되는 결과는 아래와 같을 것이다.  
```html  
<div class="hello-btn" id="hello-btn">
	<button type="button" class="btn btn-primary"></button>
</div>  
```  
  
이처럼 요소의 상속은 Root 요소로 가게 된다.  
  
### 📌 inheritAttrs: false  
이때 inheritAttrs를 이용하여 자동 적용을 방지할 수 있다.  
---  
```html  
<!-- MyButton.vue : inheritAttrs가 false로 적용된 자식 컴포넌트 -->

<template>
	<div>
		<button class="btn btn-primary" type="button">My Button</button>
	</div>
</template>

<script setup>
defineOptions({
	inheritAttrs: false, // 자동 적용 방지
});
</script>

<style lang="scss" scoped></style>
  
```  
  
이 경우 최종 렌더링 되는 결과는 아래와 같다.  
```html  
<div>
	<button type="button" class="btn btn-primary"></button>
</div>  
```  
  
아무런 요소도 상속받지 않고 이벤트도 리슨되어있지 않은 상태가 된다.  
  
### 📌 v-bind=”$attrs”  
---  
우리가 원하는 결과는 Root요소에 상속받은 요소들이나 이벤트가 적용되지 않고 내가 원하는 태그에 적용되는 모습일 것이다.  
  
그럴 때 v-bind=”$attrs” 을 사용하면 된다.  
```html  
<!-- MyButton.vue : v-bind="$attrs"가 적용된 자식 컴포넌트 -->

<template>
	<div>
		<button class="btn btn-primary" type="button"   
v-bind="$attrs"  
>My Button</button>
	</div>
</template>

<script setup>
defineOptions({
	inheritAttrs: false, // 자동 적용 방지
});
</script>

<style lang="scss" scoped></style>
  
```  
  
이 경우 최종 렌더링 되는 모습  
```html  
<div>
	<button class="btn btn-primary hello-btn" type="button" id="hello-btn">My Button</button>
</div>  
```  
  
