# notes


# 스프링 시큐리티 설정하기
스프링이 제공하는 기본 유저 정보 활용.

- spring security default id/pw: user / generated security password(서버 시작시 콘솔에 출력된 로그에서 제공)
- log에는 비번과 같은 보안 정보를 절대 출력하거나 담지 않는다. → log파일이 탈취당했을 시 위험
- override methods : ctrl o

```java
// 핵심
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
            .mvcMatchers("/", "/info").permitAll()
            .mvcMatchers("/admin").hasRole("ADMIN") // admin 권한 별도 필요
            .anyRequest().authenticated();
        http.formLogin(); // spring security 기본제공 폼 제공
        http.httpBasic();
    }
}
```
- 문제점: 계정이 하나만 존재


# 스프링 시큐리티 커스터마이징: 인메모리 유저 추가
스프링이 제공하는 기본 유저 정보를 사용하지 않고 직접 설정한 유저 정보 사용. 아주 다양한 방법들 존재.

서버 구동시 콘솔창에 출력된 아래 클래스에 들어가면 스프링 시큐리티가 default user를 어떻게 생성하는지 알 수 있다.
```
2025-08-16 17:24:42.587  INFO 44095 --- [           main] .s.s.UserDetailsServiceAutoConfiguration : 
```

아래와 같이 초기 default user의 정보를 바꿀 수도 있다(안전한 방법은 아니다 - 소스 누출시 문제)

```properties
# application.properties
spring.security.user.name=admin
spring.security.user.password=123
spring.security.user.roles=ADMIN
```
- 문제점
  - 유저 정보가 그대로 노출 가능
  - 여전히 유저계정을 한 개만 사용가능

원하는 유저 정보 여러 개 이미지로 설정하고 싶을 시 security configuration 파일에서 `configure(auth:AuthenticationManagerBuilder):void`메서드 오버라이드로 가능.





