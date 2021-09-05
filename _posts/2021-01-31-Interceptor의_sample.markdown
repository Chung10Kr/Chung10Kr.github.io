---
layout: post
title:  "[Spring Boot] 관리자 권한 체크 - Interceptor"
subtitle:   "Filter VS Interceptor"
categories: dev
tags: lang
---


Filter와 Interceptor의 차이 포스팅 Sample Code



관리자 페이지 접근하기 전에 관리자 인증 기능 Sample Code





# Interceptor 구현

#### 1. HandlerInterceptorAdapter 를 상속받은 클래스


```java

@Slf4j
public class Interceptor extends HandlerInterceptorAdapter {
    
    /*
    컨트롤러 실행 직전에 동작
    반환 값이 true -> 정상 진행
    반환 값이 false -> 실행 멈춤(컨트롤러 진입 X)
    */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
		log.info("컨트롤러 실행 직전에 동작");
		return true;
    }

    /*
    컨트롤러 진입 후 view가 랜더링 되기 전에 수행
    데이터 조작 가능
    */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info("컨트롤러 진입 후");
    }

    // 컨트롤러 진입 후 view가 정상적으로 랜더링 된 후 제일 마지막에 실행
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object object, Exception arg3) throws Exception {
        log.info("제일 마지막" );
    }

}
```

#


### 2.WebMvcConfigurerAdapter를 상속받은 설정 클래스 구현

```java

@Configuration
public class WebConfig implements WebMvcConfigurer {
  @Override
  public void addInterceptors(InterceptorRegistry registry) {
      registry.addInterceptor(new Interceptor())
              .excludePathPatterns("/users/login"); // 예외 처리할 부분
  }
}

```