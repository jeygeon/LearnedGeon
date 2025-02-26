목록을 출력할 때는 `v-for`디렉티브를 통해서 렌더링 할 수 있다.  
```javascript  
const items = ref([
	{ id: 1, language: 'Java' },
	{ id: 2, language: 'HTML' },
	{ id: 3, language: 'CSS' },
	{ id: 4, language: 'JavaScript' },
	{ id: 5, language: 'Vue3' },
]);  
```  
```html  
<ul>
  <li v-for="item in items" :key="item.id">
    id: {{ item.id }}, language: {{ item.language }}
  </li>
</ul>  
```  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/04ca6840-b00e-4359-a1cc-2bb026c60e61-image.png)  
  
표현할때는 아래와 같이 리스트의 인덱스를 가져올 수도 있다.  
```html  
<ul>
  <li v-for="(item, index) in items" :key="item.id">
    id: {{ item.id }}, language: {{ item.language }}, index: {{ index }}
  </li>
</ul>  
```  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/cccbfa4d-e621-4985-a135-560a66f8693b-image.png)  
  
`v-for`를 사용할 때 한 가지 주의 할 점은 같이 태그에서 `v-if`랑 같이 사용하는 것을 금지하고 있다. 공식문서에 의하면 `v-if`는 `v-for`보다 우선순위가 높기 때문에 아래와 같은 에러가 발생 할 수 있다고 한다.  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/d0cbdc58-ab0f-422c-afa9-60212e9d439c-image.png)  
  
그래서 `v-for`와 `v-if`가 같이 사용이 필요할 경우에는 아래와 같이 사용하라고 말해주고 있다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/60cfb39e-5804-441e-af0e-2bce20eacdfd-image.png)  
