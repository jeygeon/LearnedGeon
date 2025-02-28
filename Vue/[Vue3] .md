이벤트를 조작할 때 event.preventDefautl(),  event.stopPropagation()  함수를 사용하는 것은 굉장히 일반적인 현상이다.  
하지만 함수 내부에서 이러한 이벤트 처리 보다는 순수한 비즈니스 로직이 들어가면 훨씬 효율적일 것이다.  
Vue에서는 이러한 문제 해결을 위해 수정자(Modifier)를 제공한다.   
## 📌 .stop  
---  
간단한 코드로 알아보자  
```html  
<div id="modifiers">
	<div @click="clickDiv">
		DIV 영역
		<p @click="clickP">
			P 영역
			<span @click="clickSpan">SPAN 영역</span>
		</p>
	</div>
</div>  
```  
```javascript  
const clickDiv = () => {
	console.log('click div !');
};

const clickP = () => {
	console.log('click p !');
};

const clickSpan = () => {
	console.log('click span !');
};  
```  
```css  
#modifiers div,
#modifiers p,
#modifiers span {
	padding: 40px;
}

#modifiers div {
	background-color: #ccc;
}

#modifiers p {
	background-color: #999;
}

#modifiers span {
	background-color: #666;
	display: block;
}  
```  
  
Modifier  테스트를 위해 간단한 요소를 만들었다. 각 요소를 클릭했을 시에는 클릭한 태그가 콘솔에 찍히도록 두었다. 화면은 아래와 같을 것 이다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/48e89425-1fbe-4c83-b82d-a84b84dbc46e-image.png)  
  
  
  
이럴 때 필요한 것이 event.stopPropgation() 함수인데 이 역할을 @click  디렉티브 뒤에 접미사 .stop 을 붙여서 @click.stop 으로 사용할 수 있다.  
```html  
<div id="modifiers">
	<div @click="clickDiv">
		DIV 영역
		<p @click="clickP">
			P 영역
			<span @click.stop="clickSpan">SPAN 영역</span>
		</p>
	</div>
</div>  
```  
  
  
  
## 📌 .prevent  
---  
.stop 이 event.stopPropagation() 의 역할을 수행했다면 .prevent 는 event.preventDefault() 의 역할을 한다.  
  
아래의 코드는 a 영역을 클릭했을 시 콘솔 창에 click a ! 라는 문구가 뜬 뒤 네이버로 이동할 것 이다.  
```html  
<a href="https://www.naver.com" @click="clickA">a 영역</a>  
```  
```javascript  
const clickA = () => {
	console.log('click a !');
};  
```  
  
여기서 .prevent 를 붙여서 네이버의 이동을 막을 수도 있다.  
```html  
<a href="https://www.naver.com" @click.prevent="clickA">a 영역</a>  
```  
