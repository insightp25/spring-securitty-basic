# notes


# 스프링 시큐리티 설정하기
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



