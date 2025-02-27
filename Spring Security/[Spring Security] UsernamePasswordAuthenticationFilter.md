스프링 시큐리티는 `AbstractAuthenticationProcessingFilter`를 사용자의 인증을 진행하는 기본 필터로 사용한다.  
  
`UsernamePasswordAuthenticationFilter`는 `AbstractAuthenticationProcessingFilter`를 확장한 클래스로서 Request에서 들어온 사용자의 아이디, 패스워드에 대한 인증을 수행한다.  
  
물론 AuthenticationFilter를 커스텀으로 만들어서 직접 만든 커스텀 필터로 타게 할 수도 있다. 커스텀 필터를 만들 때는 `attemptAuthentication()` 메소드를 재 정의하면 된다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/be5fdddd-e2af-44ca-b089-dad44e9f6d8d-image.png)  
  
사용자 인증을 진행하는 순서는 아래와 같다.  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/488d4192-7923-4bb6-8f4d-33358bc8566a-image.png)  
  
아래의 설명은 그림을 보면서 순서대로 읽으면 이해가 쉬울 것 이다.  
1. 사용자의 로그인 요청을 `UsernamePasswordAuthenticationFilter`가 가로챈다.  
1. `Matcher`가 정상적인 로그인 요청인지 첫 번째로 검증 (ex. url 확인 등..)  
1. `username`과 `password`만 담긴 **인증 받지 않은 **단순 객체 하나 생성  
1. `AuthenticationManager`를 호출해서 이 객체에 대한 인증 진행  
1. 인증 성공시 DB에서 관리되는 유저 객체가 생성된다  
1. 새로운 로그인을 알리고 세션관련 작업들을 수행한다..  
1. 5번에서 생성된 객체를 `SecurityContext`에 저장된다.  
1. 로그인 인증 성공 이후 로직이 실행된다.  
  
아래는 실제로 실행되는 코드이니 한번 코드 분석해보는 것도 좋은 경험일 것 같다.  
### 1️⃣ AbstractAuthenticationProcessingFilter<br>  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/dacf379f-9190-4239-9e83-6d0056b3d62e-image.png)  
  
### 2️⃣ UsernamePasswordAuthenticationFilter<br>  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/39ba72ad-72ae-422a-9548-584c8fe56246-image.png)  
