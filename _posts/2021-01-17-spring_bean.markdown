---
layout: post
title:  "[spring] Spring Bean 이란"
subtitle:   "bean"
categories: dev
tags: spring
---

[콩 삶아먹기](https://m.blog.naver.com/PostView.nhn?blogId=busykoala&logNo=220410074074&proxyReferer=https:%2F%2Fwww.google.com%2F)







### 빈(Bean) 이란

- Spring IoC 컨테이너에 의해 인스턴스화,관리,생성된다.
- new 연산자로 어떤 객체를 생성했을 때 그 객체는 빈이 아니고

ApplicationContext.getBean()으로 얻어질 수 있는 객체를 빈이라고 한다.
   

### 빈(Bean) 정의

- XML 파일에 정의
```xml
<!--
class(필수): 정규화된 자바 클래스 이름
id: bean의 고유 식별자
scope: 객체의 범위 (sigleton, prototype)
constructor-arg: 생성 시 생성자에 전달할 인수
property: 생성 시 bean setter에 전달할 인수
init method와 destroy method
-->
<!-- A simple bean definition -->
<bean id="..." class="..."></bean>

<!-- A bean definition with scope-->
<bean id="..." class="..." scope="singleton"></bean>

<!-- A bean definition with property -->
<bean id="..." class="...">
	<property name="message" value="Hello World!"/>
</bean>

<!-- A bean definition with initialization method -->
<bean id="..." class="..." init-method="..."></bean>
```

- Component Scanning 
  
> Component-scan은 xml에 일일이 빈등록을 하지않고 각 빈 클래스에 @Component를 통해 자동 빈 등록이 된다.
> @Component @Controller @Service @Repository와 같은 어노테이션을 자동 등록 처리해준다




### Bean을 사용하는 이유

 - 자주 사용하는 객체를 singleton으로 만들어 놓고 어디서든 불러쓸 수 있도록 함




-참조1 [스프링 빈의 개념과 생성 원리](https://atoz-develop.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88Bean%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%83%9D%EC%84%B1-%EC%9B%90%EB%A6%AC)


-참조2 [Spring Bean의 개념과 Bean Scope 종류](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html)

-참조3 [코드로 익혀보는 Spring 기초1](https://dkkim2318.tistory.com/36)
