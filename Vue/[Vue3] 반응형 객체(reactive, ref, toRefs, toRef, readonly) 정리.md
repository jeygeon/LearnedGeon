## 📌 reactive()
reactive()함수를 객체나 배열같은 레퍼런스 타입을 반응형 객체로 선언할 수 있다. 해당 함수로 선언한 객체의 값이 변경되면 자동으로 값이 렌더링 된다.
### 선언
```javascript
const user = reactive({   name: 'hong   age: '25' });
```
### 사용법
```html
<p>{{ user.age }}</p> // 25
```
## 📌 ref()
reactive()함수가 객체 타입에만 동작한다면, ref()함수는 number, string, boolean 같은 primitive 타입의 반응형 상태를 만들 때 사용한다.
### 선언
```javascript
const text = ref('Hello');
```
여기서 선언된 text는 value라는 단 하나의 속성을 갖는 객체가 된다. 여기서 value는 Hello이다. 그래서 자바스크립트 문법 내애서 text의 값을 가져오기 위해서는 text.value로 해당 데이터에 접근해야 한다. ex) console.log(text.value)
### 사용법
```html
<p>{{ text }}</p> // Hello
```
## 📌 ref → Object
ref()로 선언한 데이터를 반응형 객체의 속성으로 주입또한 가능하다. 아래 예제를 살펴보자
### 선언
```javascript
const name = ref('hong'); const user = reactive({     name: name })
```
ref로 선언된 데이터를 reactive로 선언된 객체의 속성으로 주입받게 되면 자동으로 unwrapping이 되어서 .value를 안붙이고도 일반 속성으로 사용이 가능하다. ex) console.log(user.name)
### 사용법
```html
<p>{{ user.name }}</p> // hong
```
## 📌 ref → Array
ref로 선언한 데이터를 Array같은 레퍼런스 타입의 객체에도 속성 주입이 가능한데, Object의 속성으로 주입받을때랑 차이가 있는데 자바스크립트 문법에서 해당 데이터에 접근하고 싶으면 .value를 붙여야 한다는 것이다.
.value를 안붙이면 객체 자체가 나오게 된다.
```javascript
const message0 = ref('Hello'); const message1 = ref('Vue'); const messageArr = reactive([message0, message1]); console.log(messageArr[0]); // RefImpl {dep: Dep, __v_isRef: true, __v_isShallow: false, _rawValue: 'Hello', _value: 'Hello'}console.log(messageArr[0].value); // Hello
```
## 📌 toRefs()
reactive()로 선언된 객체의 속성을 구조분해 할당으로 재 선언할 수 있는데, 이렇게 되면 단순 재선언된 값이기 때문에 반응성을 잃어버리게 된다.
```javascript
const user = reactive({ 	name: '홍길동',     	age: '25',    	address: 'seoul' })  /** * 단순 변수 재선언이라서 반응성이 없다 * age, address값이 변경되어도 user객체의 속성의 변화는 없음. */ const {age, address} = user;
```
이럴때 toRefs()를 사용하게 되면, 객체의 속성과 구조분해해서 재 할당한 값은 동기화 상태가 되서 값이 똑같이 변경된다.
```javascript
const {age, address} = toRefs(user); age.value = '30'; address.value = 'busan'; console.log(user.age); // 30 console.log(user.address); // busan
```
## 📌 toRef()
만약 한가지 속성만 재 할당하고 싶다면 toRef()를 사용하고 두번째 매개변수 자리에 가져오고 싶은 속성을 작성하면 된다.
```javascript
const age = toRef(user, 'age'); age.value = '40'; console.log(user.age); // 40
```
## 📌 readonly()
readonly() 함수는 이름에서부터 알 수 있듯이 재 할당한 변수의 변경을 막는 역활을 한다.
```javascript
const original = reactive({ 	name: '홍길동',     	age: '25' })  const copy = readonly(original); copy.age = '30'; // 여기서 warn 발생 console.log(copy.age); // 25
```
