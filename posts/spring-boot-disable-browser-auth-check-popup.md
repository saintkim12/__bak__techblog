---
title: "[Spring Boot] 브라우저 인증체크 팝업 비활성화"
description:
date: 2021-09-02
tags:
  - Spring Boot
  - Spring Security
layout: layouts/post.njk
---

## 용도
스프링 시큐리티 사용시, 인증 크롬 팝업을 비활성화하여 바로 401(Unauthorized)에러를 띄우고 싶을 때

## code
```java
// CustomizedAuthenticationEntryPointConfig.java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.AuthenticationEntryPoint;

/**
 * (크롬) 인증팝업 비활성화 처리
 *
 */
public class CustomizedAuthenticationEntryPointConfig implements AuthenticationEntryPoint {
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response,
            AuthenticationException authException) throws IOException, ServletException {
        response.sendError(HttpServletResponse.SC_UNAUTHORIZED, authException.getMessage());
    }
}
```
```java
// WebSecurityConfig.java
// (org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter를 상속받은 설정 클래스)
@Configuration
@EnableWebSecurity
@Order(SecurityProperties.BASIC_AUTH_ORDER)
public class AuthorizationServerConfig extends AuthorizationServerConfigurerAdapter {
    // ..
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 다른 중간 설정은 참고만
        http
        // .csrf().disable().headers().frameOptions().disable().and().authorizeRequests()
        //         .requestMatchers(CorsUtils::isPreFlightRequest).permitAll()
        //         .antMatchers(HttpMethod.OPTIONS).permitAll()
                // .antMatchers("/**").permitAll()
                // .antMatchers("/").permitAll()
                // .anyRequest().authenticated() //
                // .and()
                .httpBasic()
                .authenticationEntryPoint(new CustomizedAuthenticationEntryPointConfig()); //<-- new 방식이던 @Component 로 등록 후 @Autowired로 하던 될 거로 보임
    }
}
```
