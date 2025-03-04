form에서 입력 양식을 처리할 때 입력 요소의 값과 자바스크립트 요소의 값을 동기화 해야 하는 경우가 많다.  
  
## 1️⃣ input  
---  
아래 태그에서 처럼 input 태그의 입력이 있을 때마다 @input 디렉티브를 통해서 value 를 동기화 해주어야 한다.  
```javascript  
const inputValue = ref('');  
```  
```html  
<input
	type="text"
	:value="inputValue"
	@input="event => (inputValue = event.target.value)"
/>  
```  
  
이 작업을 단축한 것이 v-model 디렉티브이다. v-model 디렉티브를 사용하면 아래와 같이 코드를 줄일 수 있다.  
```html  
<input type="text" v-model="inputValue" />  
```  
  
이렇게 v-model 을 사용하면 자동으로 양방향 데이터 바인딩이 가능하다.  
## 2️⃣ checkbox  
---  
```javascript  
const checkboxValue = ref(true);  
```  
```html  
<input type="checkbox" id="checkbox" v-model="checkboxValue" />
<label for="checkbox">{{ checkboxValue }}</label>  
```  
  
이렇게 했을 때는 checked 상태가 어떤가에 따라서 보여지는 화면은 아래와 같을 것 이다.  
  
  
## 3️⃣ Radio  
---  
```javascript  
const radioValue = ref(true);  
```  
```html  
<label>
	<input type="radio" name="type" value="HTML" v-model="radioValue" />
	HTML
</label>
<label>
	<input type="radio" name="type" value="CSS" v-model="radioValue" />
	CSS
</label>
<div>{{ radioValue }}</div>  
```  
  
  
  
## 4️⃣ Select  
```javascript  
const selectValue = ref('HTML');  
```  
```html  
<select v-model="selectValue">
	<option value="HTML">HTML</option>
	<option value="CSS">CSS</option>
	<option value="JavaScript">JavaScript</option>
</select>
<div>{{ selectValue }}</div>  
```  
  
  
  
  
이렇게 v-model 은 HTML 요소가 무엇이냐에 따라서 서로 다른 속성과 이벤트를 처리한다.  
  
---  
## 📌 References  
  
  
  
