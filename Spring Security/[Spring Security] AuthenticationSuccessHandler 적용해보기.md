Spring Security에서는 사용자 인증에 대한 처리가 UsernamePasswordAuthenticationFilter에서 자동으로 이루어지는데 해당 필터에서 사용자 인증이 성공이 된 이후 실행하고 싶은 핸들러를 직접 추가할 수 있다.  나 같은 경우는 사용자 인증 방식을 JWT로 할 것이기 때문에 사용자 인증 성공 이후 JWT를 생성해주는 핸들러를 추가해 볼 것이다.  전체적인 Flow는 아래와 같이 진행할 것이다.
![TIL_IMAGE](./image/image.png)

### 1️⃣ JwtGenerateHandler
1. AuthenticationSuccessHandler 인터페이스를 상속 받은 JwtGenerateHandler 생성 2. 실행 시키고 싶은 코드 onAuthenticationSuccess() 메소드 안에 재 정의

```java
@Component @RequiredArgsConstructor public class JwtGenerateHandler implements AuthenticationSuccessHandler {      @Override     public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {         System.out.println("jwt 생성 성공~");     } }
```
### 2️⃣ SecurityConfig
1. handler 의존성 추가 2. .successHandler(jwtGenerateHandler) 추가 
```java
@Configuration @EnableWebSecurity @RequiredArgsConstructor public class SecurityConfig {      private final JwtGenerateHandler jwtGenerateHandler;      @Bean     public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {         http             .formLogin(form -> form                 .loginProcessingUrl("/api/user/login")                 .usernameParameter("id")                 .passwordParameter("password")                 .defaultSuccessUrl("/")                 .successHandler(jwtGenerateHandler) // 이 부분 추가             )             .cors(cors -> cors                 .configurationSource(corsConfigurationSource()));          return http.build();     } }
```

![TIL_IMAGE](./image/image.png)

