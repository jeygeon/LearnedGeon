## 📌 v-if
Vue에서 특정 조건에 따라서 컴포넌트를 렌더링 할 때는 `v-if`를 사용한다. `v-if`안의 조건이 true이면 출력 false면 출력 되지 않는다.
```javascript
const lnag = ref('Vue3');
```
```html
<h2 v-if="lang === 'Vue3'">Hello Vue3</h2>
```
 if - else, if - else if - else 또한 사용 가능하다. 
```javascript
const lang = ref('Vue3');
```
```html
<h2 v-if="lang === 'Java'">Hello Java</h2> <h2 v-else-if="lang === 'Vue3'">Hello Vue3</h2> <h2 v-else>Hello React</h2
```
 <template> 태그를 사용하여 하나의 조건으로 여러 개의 HTML요소를 렌더링 할 수도 있다.
```html
<template v-if="true">   <h1>Title: 제목</h1>   <h2>author: 글쓴이</h2>   <h3>content: 내용</h3> </template>
```

## 📌 v-show
v-show를 사용해서도 요소를 아래와 같이 표현할 수 있다.
```html
<h2 v-show="true">Hello Vue3</h2>
```
이렇게만 봤을 때는 v-if와 사용법은 똑같고 큰 차이가 없는 것으로 보이는데, v-if와 v-show의 가장 큰 차이는 요소의 렌더링 여부이다.  v-if는 안에 조건이 true일 경우 렌더링 자체가 되지 않지만, v-show일 경우는 조건의 true, false 여부의 상관 없이 무조건 렌더링이 되고 조건에 따라서 display:none / display: block 속성 전환이 된다. 
```html
<h2 v-if="false">v-if</h2> <h2 v-show="false">v-show</h2>
```
 개발자 도구를 사용해서 위의 태그를 보면 아래와 같이 렌더링 된 것을 알 수 있다.
![TIL_IMAGE](./image/image.png)
 v-if는 조건이 자주 변경되지 않는 경우나, 무거운 컴포넌트나 리스트를 렌더링 할 때 적합할 수 있고, v-show는 요소가 자주 변경되는 경우에 적합할 수 있겠다.
