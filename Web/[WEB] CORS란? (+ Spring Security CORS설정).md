개인 프로젝트로 JWT로그인을 구현하던 도중, 서버에서 JWT 토큰을 발급하고 헤더에 담아서 프론트로 전달한 뒤 response에서 확인이 되지 않는 이슈가 있어서 해당 문제를 해결하면서 학습한 내용을 정리하려고 한다.  
  
## 💡 CORS란?<br>  
### SOP<br>  
---  
일단 CORS에 대해서 알기 전에 SOP에 대해서 알아야 한다.  
  
`SOP(Same-Origin Policy)`는 **동일 Origin에 대해서만 자원공유를 허용**한다는 브라우저 보안 정책이다.  
  
URL을 구성하는 protocal과 host, port를 모두 합한 것을 Origin 이라고 부른다. 그리고 Origin을 구성하는 요소들 중 하나라도 다른 경우 다른 Origin으로 인식된다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/9dc7ed8e-1ecc-4419-9a33-5e3556097c95-image.png)  
  
한가지 상황을 가정해보자  
한 대의 서버에서 리액트를 기동하여 웹 접속 후 동일 서버에서 기동중인 서버로 요청을 보내는 상황이다.  
  
당연히 포트만 변경되어서 아래와 같이 프로세스가 돌아가고 있을 것이고 이 두개의 프로세스는 다른 Origin이다.  
  
그러면 SOP 정책에 의해서 정상적으로 요청이 가지 않을 것이다.  
(서버끼리 통신은 가능하지만 웹 접속을 한 뒤 서버로 요청을 보내는 경우이다. SOP는 브라우저 보안 정책임을 생각하자.)  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/b5513aa9-0183-410b-98a2-1c5a06aec9bd-image.png)  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/4c072086-c16c-4222-b6ed-df09969000be-image.png)  
  
위와 같은 에러는 개발하는 사람이라면 어쩌다가 한번쯤 마주쳐봤을 에러일 것이다.  
  
저기서 중요한 점은 **CORS error**라고 적혀있어서 cors 때문에 뭐가 에러가 났나보다~ 라고 생각할 수 있지만  
  
실제로는 SOP 정책때문에 발생된 에러고 저 에러를 해결하려면 서버에서 CORS관련한 설정을 해주어야 한다.  
  
### CORS<br>  
---  
CORS(Cross-Origin Resource Sharing)는 SOP와 달리 서로 다른 Origin이라도 자원을 공유할 수 있게 허용하는 정책이다.  
  
브라우저에서 서버로 요청을 보내면 Request Header에 요청을 보낸 브라우저의 Origin이 담겨져 전송이 된다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/3dda2a42-6b4a-46be-bb0b-d79d169fac02-image.png)  
  
서버에서는 요청을 허용할 Origin, 허용할 메소드(GET, POST 등..), 허용할 헤더 등등을 설정할 수가 있다.  
  
## Spring Security CORS 설정<br>  
---  
아래는 Spring Security에서 내가 해놓은 CORS 설정이다.  
```java  
@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {

  @Bean
  public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http.cors(cors -> cors
           .configurationSource(corsConfigurationSource())); // CORS 설정 적용

    return http.build();
  }

  @Bean
  public CorsConfigurationSource corsConfigurationSource() {
    CorsConfiguration config = new CorsConfiguration();
    config.setAllowedOrigins(List.of("http://localhost:5173"));
    config.setAllowedMethods(List.of("GET", "POST", "PUT", "DELETE"));
    config.setAllowedHeaders(List.of("X-access-token", "X-refresh-token", "Content-Type", "Authorization"));
    config.setExposedHeaders(List.of("X-access-token", "X-refresh-token", "Content-Type", "Authorization"));

    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/**", config);
    return source;
  }  
```  
### 1️⃣ setAllowedOrigins :<br>  
* 허용할 origin을 지정  
### 2️⃣ setAllowedMethods :<br>  
* 허용할 method를 지정  
### 3️⃣ setAllowedHeaders :<br>  
* 허용할 header를 지정  
### 4️⃣ setExposedHeaders :<br>  
* 클라이언트가 읽을 수 있도록 헤더를 지정  
  
`setExposedHeaders()`에 대해서 부가적으로 설명하자면, 나 같은 경우는 사용자가 로그인하면 서버에서는 사용자 인증이 성공할 시 JWT토큰을 response 헤더에 담아서 클라이언트로 보내는 과정이 있다.  
  
하지만 클라이언트에서 아무리 해당 토큰을 조회해도 `undefined`가 떠서 삽질해보니 위의 저 메소드를 추가해주어야 한다.  
