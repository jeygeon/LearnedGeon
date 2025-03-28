템플릿 문법 {{ }}은 간단히 사용하면 매우 편리하지만, 템플릿 표현식 내의 코드가 너무 길어질 경우 가독성이 떨어지고 유지보수하기 어려워 질 수 있다.  아래 코드는 간단하게 num이 짝수면 짝수 아니면 홀수를 출력하는 표현식이다. 템플릿 표현식안에서 삼항연산자를 사용해서 이렇게 표현해도 큰 문제는 없지만 가독성이 떨어진다는 단점이 존재한다. 
```javascript
const num = ref(5);
```
```html
<p>{{ (num % 2 == 0) ? "짝수" : "홀수" }}</p>
```
 위의 코드를 computed를 사용하여 아래와 같이 변경 할 수 있다.
```javascript
const num = ref(5); const result = computed(() => {     return (num % 2 == 0) ? "짝수" : "홀수" })
```
```html
<p>{{ result }}</p
```
 아래 코드와 같이 computed사용 없이도 충분이 구현이 가능하기 때문에 꼭 computed를 써야되나 라는 생각이 들 수 있다. 
```javascript
const num = ref(5); const result = () => {     return (num % 2 == 0) ? "짝수" : "홀수" }
```
```html
<p>{{ result() }}</p>
```
 결론적으로는 computed를 사용하는 것이 성능상 훨씬 좋다. 그 이유는 computed를 사용했을 경우에 나온 데이터가 캐싱되기 때문이다. 그리고 computed함수가 실행되는 시점은 함수 안의 데이터가 변경되는 시점이다.  위의 예시에서는 `num`이 변경되는 경우이다.  그리고 computed는 읽기 전용 함수이기 때문에 아래와 같이 값을 변경하려고 하면 경고 창이 발생한다.

```javascript
const num = ref(5); let result = computed(() => {     return num.value % 2 == 0 ? '짝수' : '홀수'; }); result.value = '변경'; // 여기서 warn 발생 console.log(result.value); // 홀수
```

![TIL_IMAGE](./image/image.png)
 그래서 computed의 값을 변경하고 싶다면 get, set 함수를 사용해야 한다.

```javascript
const num = ref(5); let result = computed(() => {     return num.value % 2 == 0 ? '짝수' : '홀수'; });  result = computed({     get() {         return num.value % 2 == 0 ? '짝수' : '홀수';     },     set(value) {         if (value == '짝수') {             num.value++;         } else {             num.value--;         }     }, });  console.log('변경전: ', num.value); // 5 result.value = '짝수'; console.log('변경후: ', num.value); // 6
```
 get() 함수는 computed의 값을 반환하는 역할을 하고, set() 함수는 computed의 값을 변경할 때 실행되는 함수이다.  즉, get()은 값을 조회할 때 실행되며, set()은 값을 변경할 때 실행되어 내부의 ref 값(num.value)을 변경하는 방식으로 동작합니다.  위 코드에서 result.value = '짝수';를 실행하면 set() 함수가 동작하여 num.value를 6으로 변경한다. 마찬가지로, result.value = '홀수';를 실행하면 num.value가 5로 변경된다.  이러한 방식으로 computed 속성을 단순한 읽기 전용 속성이 아니라, 동적으로 값을 조정할 수 있는 속성으로 활용할 수 있다.
