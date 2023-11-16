---
Author: CarefreeLife98
Date: 2023-11-16T17:22:00
Agenda: 
tags:
---
> 하 이게 뭐라고..
> - 기존 코드에 있던 Swagger 를 기존 개발자 분이 Security 랑 JWT Token 적용 해놓고 바꿔놓지 않아서 많이 헤맸다.
> - 여튼 방금 연동 성공.

# Swagger Error: unable to infer base url / 401 / 403

## 현재 Dependency
```java
// Swagger 2  
implementation group: 'io.springfox', name: 'springfox-swagger2', version: '3.0.0'  
implementation group: 'io.springfox', name: 'springfox-swagger-ui', version: '3.0.0'  
implementation group: 'io.springfox', name: 'springfox-boot-starter', version: '3.0.0'  
  
// Spring Security 추가  
implementation 'org.springframework.boot:spring-boot-starter-security'  
  
// token  
implementation "io.jsonwebtoken:jjwt:0.9.1"
```

## SwaggerConfig
```java
package bewater.petqua.config;  
  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import springfox.documentation.service.ApiInfo;  
import springfox.documentation.service.Contact;  
import springfox.documentation.spi.DocumentationType;  
import springfox.documentation.spring.web.plugins.Docket;  
import springfox.documentation.swagger2.annotations.EnableSwagger2;  
  
import java.util.ArrayList;  
import java.util.Arrays;  
import java.util.HashSet;  
import java.util.Set;  
  
@Configuration  
@EnableSwagger2  
public class SwaggerConfig {  
    private static final Contact DEFAULT_CONTACT = new Contact("petqua",  
            "https://github.com/petqua/petqua_backend",  
            "team@petqua.co.kr");  
  
    private static final ApiInfo DEFAULT_API_INFO = new ApiInfo("Title",  
            "Description",  
            "1.0",  
            "urn:tos",  
            DEFAULT_CONTACT,  
            "Apache 2.0",  
            "http://www.apache.org/licenses/LICENSE-2.0",  
            new ArrayList<>());  
  
    private static final Set<String> DEFAULT_PRODUCES_AND_CONSUMES  
            = new HashSet<>(Arrays.asList("application/json", "application/xml"));  
  
    @Bean  
    public Docket api() {  
        return new Docket(DocumentationType.SWAGGER_2)  
                .apiInfo(DEFAULT_API_INFO)  
                .produces(DEFAULT_PRODUCES_AND_CONSUMES)  
                .consumes(DEFAULT_PRODUCES_AND_CONSUMES);  
    }  
}
```

## SecurityConfig
> 사실 진짜 문제는 SecurityConfig 였다.

```java
package bewater.petqua.config;  
  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.security.config.annotation.web.builders.HttpSecurity;  
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;  
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;  
import org.springframework.security.config.annotation.web.configuration.WebSecurityCustomizer;  
import org.springframework.security.web.SecurityFilterChain;  
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;  
import org.springframework.security.web.firewall.DefaultHttpFirewall;  
import org.springframework.security.web.firewall.HttpFirewall;  
  
import java.util.Arrays;  
import java.util.List;  
  
@Configuration  
@EnableWebSecurity  
public class SecurityConfig {  
    @Value("${jwt.secret}")  
    private String jwtSecret;  
  
    private static final String[] PERMIT_URL_ARRAY = {  
            /* swagger v2 */  
            "/v2/api-docs",  
            "/swagger-resources",  
            "/swagger-resources/**",  
            "/configuration/ui",  
            "/configuration/security",  
            "/webjars/**",  
            "/swagger-ui/**",  
            /* health check, sms */  
            "/actuator/health",  
            "/sms/**"  
    };  
  
    @Bean  
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
        http  
                .cors().and()  
                .addFilterBefore(jwtTokenFilter(), UsernamePasswordAuthenticationFilter.class)  
                .authorizeRequests()  
                .antMatchers(PERMIT_URL_ARRAY).permitAll()  
                .anyRequest().authenticated();  
  
        http.csrf().disable();  
  
        http.headers().frameOptions().disable();  
        return http.build();  
    }  
  
    @Bean  
    public JwtTokenFilter jwtTokenFilter() {  
        List<String> permitAllEndpoints = Arrays.asList(PERMIT_URL_ARRAY);  
        return new JwtTokenFilter(jwtSecret, permitAllEndpoints);  
    }  
    @Bean  
    public WebSecurityCustomizer webSecurityCustomizer() {  
        return (web) -> web.ignoring().antMatchers(PERMIT_URL_ARRAY);  
    }  
}
```

> **요청을 허용하는 URL 을 모아놓은 "PERMIT_URL_ARRAY" 를**
> - SecurityFilterChain
> - JwtTokenFilter
> - WebSecurityCustomizer
> 
> <br>
> **세 곳에 모두 적용하니 해결되었다.**

![[스크린샷 2023-11-16 오후 5.34.23.png]]
> 영롱하구만..