  
  
반응형 상태가 변경되었을 때를 감지하여 다른 작업을 수행해야 하는 경우가 있다. ex) api call..  
Vue에서는 `watch()` 함수를 통해 해당 작업을 수행할 수 있다.  
  
## 1️⃣ Watch()<br>  
---  
```javascript  
const message = ref('hello');
watch(message, (newValue, oldValue) => {
	console.log(newValue);
	console.log(oldValue);
});  
```  
  
  
  
반응형 객체 또한 변경을 감지할 수 있다. 이때 중첩된 모든 속성도 트리거가 된다.  
```javascript  
const person = reactive({
	name: 'hong',
	age: 25,
	language: {
		HTML: true,
		CSS: true,
		JavaScript: true,
	},
});

watch(person, value => {
	console.log(value);
});  
```  
  
  
  
### 💡 deep<br>  
아래와 같이 `watch()` 의 세 번째 매개변수로 `deep` 값을 `false` 로 주게 되면 중첩 속성의 변화를 감지하지 않는다.  
```javascript  
watch(
	person,
	value => {
		console.log(value);
	},
	{ deep: false },
);  
```  
  
`deep: true` 로 해서 심층 분석을 했을 경우 공식 문서에서는 큰 용량의 데이터를 처리할 경우 비용이 많이 들 수 있다고 한다. 이 점 유의하면서 사용해야 한다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/20ac96fe-0504-4245-9e98-ab51122008bb-image.png)  
  
### 💡 immediate<br>  
`immedate` 를 세 번째 매개변수로 전달했을 때 `watch()` 최초에 바로 한번 실행할지 여부를 정할 수 있다.  
  
```javascript  
const message = ref('Hello');
const immediateMessage = ref('');

watch(
	message,
	value => {
		immediateMessage.value = value + ' Vue3';
		console.log(immediateMessage.value);
	},
	{ immediate: true },
);  
```  
  
이렇게 `immediate: true` 로 했을 경우 `message` 가 변경되기 전에 함수가 실행이 되어서 `Hello Vue3` 가 콘솔에 출력된다.  
  
### 💡 once<br>  
`once` 를 매개변수로 줄 경우에는 소스가 변경될 때 콜백이 한번만 실행된다.  
```javascript  
const message = ref('Hello');
const immediateMessage = ref('');

watch(
	message,
	value => {
		immediateMessage.value = value + ' Vue3';
		console.log(immediateMessage.value);
	},
	{ once: true },
);  
```  
  
## 2️⃣ WatchEffect()<br>  
---  
`watchEffect()` 는 콜백 함수 내부에 데이터의 변화가 감지되면 함수가 실행이 된다.  
  
```html  
<input type="text" v-model="title" />
<input type="text" v-model="contents" />  
```  
```javascript  
const title = ref('');
const contents = ref('');

// title과 contents가 변경되면 watchEffect 함수가 호출된다.
// watchEffect는 watch와는 다르게 렌더링시 한번 즉시 실행된다.
watchEffect(() => {
	console.log('title: ', title.value, ', contents: ', contents.value);
});  
```  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/1af29d58-6525-4521-992c-fcdea3c6caa4-image.png)  
  
## 3️⃣  Watch() vs WatchEffect()<br>  
---  
`watch()` 와 `watchEffect()` 는 모두 데이터의 변화를 감지해서 콜백 함수를 실행한다는 공통점이 있지만 가장 큰 차이는 추적하는 데이터를 명시적으로 하는지의 차이가 있다.  
  
### watch()<br>  
```javascript  
const message = ref('hello');
watch(message, (newValue, oldValue) => {
	console.log(newValue);
	console.log(oldValue);
});  
```  
  
### watchEffect()<br>  
```javascript  
const title = ref('');
const contents = ref('');
watchEffect(() => {
	console.log('title: ', title.value, ', contents: ', contents.value);
});  
```  
  
`watch` 에서는 `message` 라는 데이터를 명시적으로 지정하지만 `watchEffect` 에서는 명시적인 데이터 지정 없이 콜백 함수 안에서의 데이터 변화를 감지한다.  
  
그리고 `watch` 는 **변경 전 데이터**, **변경 후 데이터 **모두 확인 가능하지만, `watchEffect` 그렇지 않다는 특징이 있다.  
  
---  
## 📌 References<br>  
  
  
  
