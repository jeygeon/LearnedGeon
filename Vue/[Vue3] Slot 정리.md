  
  
## 1️⃣ Slot이란?<br>  
부모 컴포넌트에서 자식컴포넌트를 사용할 때 컨텐츠를 전달할 수 있는데 이 때 사용하는 것이 `<slot>` 이다.  
  
예를들어  
```html  
<!-- 자식 컴포넌트 -->

<button class="fancy-btn">
	<slot></slot>
</button>  
```  
```html  
<!-- 부모 컴포넌트 -->

<FancyButton>
	클릭하기!
</FancyButton>  
```  
  
최종적으로 렌더링되는 모습은  
```html  
<button class="fancy-btn">클릭하기!</button>  
```  
  
이런식으로 내부 컨텐츠를 자식 컴포넌트에게 제공할 수 있다.  
![이미지 출처 : https://v3-docs.vuejs-korea.org/guide/components/slots.html#slot-content-and-outlet](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/08c77644-c15b-4652-89db-a31658d0a5b6-image.png)  
  
## 2️⃣ slot name 지정<br>  
그리고 `<slot>`에 `name`을 적용하여 내가 원하는 `slot`에 원하는 컨텐츠 전달도 가능하다.  
```html  
<!-- 자식 컴포넌트 : AppCard.vue -->

<div>
	<slot name="header">#Header</slot>
</div>
<div>
	<slot name="content1">#Title</slot>
	<slot>#Content</slot> <!-- name을 지정하지 않을경우 기본은 default이다. -->
</div>
<div>
	<slot name="footer">#Footer</slot>
</div>  
```  
```html  
<!-- 부모 컴포넌트 -->

<AppCard>
	<template v-slot:header>제목</template>
	<template v-slot:[slotArgs]>내용1</template> <!-- slot이름은 동적으로 적용도 가능하다. -->
	<template v-slot>내용2</template> <!-- name을 지정하지 않을 경우 기본은 default이다. -->
	<template #footer>시간</template> <!-- slot의 단축표현으로 #을 사용할 수 있다. -->
</AppCard>

<script setup>
import AppCard from './AppCard.vue';

const slotArgs = ref('content1');
</script>
  
```  
  
최종적으로 렌더링은 아래와 같이 될 것이다.  
```html  
<div>
	제목
</div>
<div>
	내용1
	내용2
</div>
<div>
	시간
</div>  
```  
  
## 3️⃣ 자식에서 부모로 데이터 전달<br>  
자식 컴포넌트에서 정의한 데이터를 부모 컴포넌트로 `slot`을 이용해서 전달할 수 있다.  
```html  
<!-- 자식 컴포넌트 : AppCard.vue -->

<div>
	<slot name="content3" content-message="내용3"></slot>
</div>  
```  
```html  
<!-- 부모 컴포넌트 -->

<AppCard>
	<template #content3="{ contentMessage }">
		{{ contentMessage }}
	</template>
</AppCard>  
```  
  
자식 컴포넌트에서 전달한 `내용3` 을 부모 컴포넌트에서 구조 분해 할당을 통해서 전달받아서 표현 할 수 있다.  
  
## 4️⃣ 렌더링 여부 결정<br>  
만약 `slot`이 정의되지 않았다면 아래와 같이 `v-if`를 통해 렌더링 여부를 결정할 수 도 있다.  
```html  
<!-- 자식 컴포넌트 : AppCard.vue -->

<div>
	<slot name="content1"></slot>
	<slot name="content2"></slot>
	<slot v-if="$slots.content3" name="content3"></slot>
</div>  
```  
```html  
<!-- 부모 컴포넌트 -->

<AppCard>
	<template v-slot:content1>내용1</template>
	<template v-slot:content2>내용2</template>
</AppCard>  
```  
  
이렇게 되어있을 경우 `content1`, `content2`만 데이터가 상속 받고 `content3`은 정의되지 않았기 떄문에 렌더링이 되지 않아 최종적으로 아래와 같이 렌더링 된다.  
```html  
<div>
	내용1
	내용2
</div>  
```  
  
---  
## 📌 References<br>  
  
  
