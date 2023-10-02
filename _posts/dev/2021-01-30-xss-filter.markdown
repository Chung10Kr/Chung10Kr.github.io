---
layout: post
title:  "[Spring Boot]  XSS 처리 - Filter "
subtitle:   "Filter VS Interceptor"
categories: dev
tags: note
---

Filter와 Interceptor의 차이 포스팅 Sample Code




# Filter 구현

### XSS처리 

#### 1. FilterRegistrationBean으로 필터 등록

```java
@Configuration
public class FilterConfiguration implements WebMvcConfigurer {
	
	@Bean
	public FilterRegistrationBean getFilterRegistrationBean() {
		FilterRegistrationBean registrationBean = new FilterRegistrationBean(new Fileter());
		registrationBean.setOrder(Integer.MIN_VALUE);
		registrationBean.setUrlPatterns(Arrays.asList("/*"));
		return registrationBean;
	}
	
}
```

```java

public class Fileter implements Filter {
    
    private Logger log = LoggerFactory.getLogger("error");
    
	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		log.info("init Filter");
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		HttpServletRequest req = (HttpServletRequest) request;
		HttpServletResponse res = (HttpServletResponse) response;
		
		chain.doFilter(new RequestWrapper(req), res);
	}
	
	@Override
	public void destroy() {
		log.info("destroy Filter");
	}
	
}
```


```java
public class RequestWrapper extends HttpServletRequestWrapper {

 public RequestWrapper(HttpServletRequest servletRequest){
  super(servletRequest);
 }
 public String[] getParameterValues(String parameter){
  String[] values = super.getParameterValues(parameter);
  
  if(values == null){
   return null;
  }
  
  int count = values.length;
  String[] encodedValues = new String[count];
  for ( int i = 0; i<count; i++){
   encodedValues[i] = cleanXSS(values[i]);
  }
  
  return encodedValues;
 }
 
 public String getParameter(String parameter){
  String value = super.getParameter(parameter);
  if(value == null){
   return null;
  }
  return cleanXSS(value);
 }
 
 public String getHeader(String name){
  String value = super.getHeader(name);
  if(value==null){
   return null;
  }
  return cleanXSS(value);
 }
 
 
 /**
  * 크로스사이트 스크립팅 필터처리
  * @param value
  * @return
  */
 private String cleanXSS(String value){

  value = value.replaceAll("<"                                         , "<");
  value = value.replaceAll(">"                                         , ">");
  value = value.replaceAll("\\("                                       , "(");
  value = value.replaceAll("\\)"                                       , ")");
  value = value.replaceAll("'"                                         , "'");
  value = value.replaceAll("eval\\((.*)\\)"                            , "");
  value = value.replaceAll("[\\\"\\\'][\\s]*javascript:(.*)[\\\"\\\']" , "\"\"");
  value = value.replaceAll("[\\\"\\\'][\\s]*vbscript:(.*)[\\\"\\\']"   , "\"\"");
  value = value.replaceAll("script"                                    , "");
  value = value.replaceAll("onload"                                    , "no_onload");
  value = value.replaceAll("expression"                                , "no_expression");
  value = value.replaceAll("onmouseover"                               , "no_onmouseover");
  value = value.replaceAll("onmouseout"                                , "no_onmouseout");
  value = value.replaceAll("onclick"                                   , "no_onclick");
  value = value.replaceAll("<iframe"                                   , "<iframe");
  value = value.replaceAll("<object"                                   , "<object");
  value = value.replaceAll("<embed"                                    , "<embed");
  value = value.replaceAll("document.cookie"                           , "document.cookie");
  
  
  return value;
 }
 
}
```


#### 2.@WebFilter 애너테이션 필터 등록

```java
@WebFilter(urlPatterns= "/*")
public class Fileter implements Filter {
    
    private Logger log = LoggerFactory.getLogger("error");
    
	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		log.info("init Filter");
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		HttpServletRequest req = (HttpServletRequest) request;
		HttpServletResponse res = (HttpServletResponse) response;
		
		chain.doFilter(new RequestWrapper(req), res);
	}
	
	@Override
	public void destroy() {
		log.info("destroy Filter");
	}
	
}
```


```java
@ServletComponentScan
@SpringBootApplication
public class testApplication {

	public static void main(String[] args) {
		SpringApplication.run(testApplication.class, args);
	}

}
```