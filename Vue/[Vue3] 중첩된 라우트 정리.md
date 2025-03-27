Vue Router를 사용할 때 중첩 라우트(Nested Routes)를 사용하여 계층적인 컴포넌트 구조를 보여줄 수 있다.  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/ef2acc5e-27f5-46a2-8b8b-2c2505a86e14-image.png)  
  
위와 같이 헤더에 **Menu1**, **Menu2**, **Menu3** 이 있고 클릭하는 Menu에 따라서 보여줄 컴포넌트가 `<RouterView>` 에 렌더링 되는 구조가 있다고 가정했을 때, 중첩된 컴포넌트를 사용하면 아래와 같이 `<RouterView>` 안에서 또 다시 렌더링 될 컴포넌트들을 보여줄 수 있다.  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/9bb8cba1-bfcb-4727-a2bc-d89518942737-image.png)  
  
```javascript  
{
  path: '/nested',
  component: NestedView,
  name: 'NestedView.vue',
    
children: [
    {
      path: 'one',
      component: NestedOneView,
      name: 'NestedOne',
    },
    {
      path: 'two',
      component: NestedTwoView,
      name: 'NestedTwo',
    },
  ]  

}  
```  
중첩된 라우트 설정하는 방법은 `children` 속성으로 라우트를 넣어주면 된다.  
위와 같이 설정된 라우트는 아래와 같이 보여진다.  
* `/nested` → NestedView 컴포넌트 렌더링  
  
```html  
<!-- NestedView.vue -->

<template>
  <div>
    <ul class="nav nav-pills">
      <li class="nav-item">
        <RouterLink class="nav-link" active-class="active" to="/nested/one">
          Nested One
        </RouterLink>
      </li>
      <li class="nav-item">
				<RouterLink class="nav-link" active-class="active" to="/nested/two">
          Nested Two
        </RouterLink>
      </li>
    </ul>
    
    <hr class="my-4" />
    
    <!-- 클릭한 RouterLink에 따라서   
NestedOneView  
 or   
NestedTwoView  
가 렌더링  
   
-->
    <RouterView></RouterView>
  </div>
</template>

<script setup></script>

<style lang="scss" scoped></style>
  
```  
  
  
---  
## 📌 References<br>  
  
  
  
