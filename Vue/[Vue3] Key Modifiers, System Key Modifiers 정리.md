이벤트가 실행될 때 키보드의 특정 키로 실행했는지 확인해야 될 때도 종종 있다. Vue에서는 이러한 상황을 위해 키 수정자를 추가할 수 있다.  
  
```html  
<div>
	<input type="text" @keyup.enter="addTodo" />
	<ul>
		<li v-for="(todo, index) in todos" :key="index">{{ todo }}</li>
	</ul>
</div>  
```  
위의 코드와 같이 text에 todo를 입력하고 enter 를 눌러야 addTodo 함수가 실행도록 설정 할 수 있다.  
  
Vue에서 일반적으로 사용되는 키에 대한 수정자는 아래와 같다  
* .enter   
* .tab  
* .delete → (Delete키와 Backspace 키를 모두 수신한다.)  
* .esc  
* .space  
* .up  
* .down  
* .left  
* .right  
  
또한 키 수정자에 시스템 키를 추가할 수도 있다.  
```html  
<div>
	<input type="text" @keyup.alt.enter="addTodo" />
	<ul>
		<li v-for="(todo, index) in todos" :key="index">{{ todo }}</li>
	</ul>
</div>  
```  
이렇게 단순 enter 가 아니라 alt + enter 를 같이 눌렀을 때 실행되도록 설정 할 수도 있다.  
  
Vue에서 제공되는 시스템 키 수정자는 아래와 같다.  
* .ctrl  
* .alt  
* .shift  
* .meta ( Mac → 명령 키(⌘), Window → Windows 키(⊞) )  
  
---  
## 📌 References  
  
  
  
