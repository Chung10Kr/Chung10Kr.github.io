---
layout: post
title:  "[spring] @RequestBody vs @RequestParam"
subtitle:   "@RequestBody vs @RequestParam"
categories: dev
tags: note
---

이 전의 [GET , POST](https://chung10kr.github.io/dev/2021/01/02/get_post/) 포스팅 을 했었습니다.


HTTP 규칙을 통해 클라이언트와 서버가 데이터를 주고받는데



프로젝트 진행중 관련된 이슈가 발생하여 정리해보겠습니다.


##### 스프링에서 데이터를 받는 대표적인 방법은 **@RequestBody** 와 **@RequestParam** 가 있습니다.
 

## @RequestBody

어노테이션 이름만 봐도 Body에서 데이터를 받는 느낌입니다. 그렇기 때문에 Body가 존재 하지 않는 **GET방식에서는 사용 불가능** 합니다.

- GET방식은 Request Packert에 Body가 존재하지 않기 때문에 @RequestBody로 받으려먼 반드시 **POST** 여야 한다.
- HTTP 요청의 body 부분을 Java Object로 변환 시켜준다. 주로 객체 단위로 받아 사용 (ex name,과 age 를 필요로 하는 Member 클래스가 있고 getter로 구현되어 있다면)

아래 소스를 보면서 한번더 설명 하겠습니다.


```java
//바디에 담긴 데이터 = name=Lee&age=28 

@PostMapping("/receive")
public String age(@RequestBody String req) {
    System.out.println(req); //name=Lee&age=28 -> String으로 출력
    return "result";
}
@PostMapping("/receive")
public String test(@RequestBody Member Member) {
    System.out.println(Member);
    //Member 객체를 자동으로 생성해 준다.
    return "result";
}
@PostMapping("/receive")
public String test(@RequestBody Map<String, Object> commandMap) {
    System.out.println(commandMap); // HashMap {name=LEE,age=28} 출력 
    //여기서 주의점!! 28은 Integer 형식으로 출력됨
    return "result";
}
```


## @RequestParam


- 요청 파라미터를 메소드에서 1:1로 받기 위해서 사용한다, 기본적으로 반드시 해당 파라미터가 전송 되어야 한다.
- 기본적으로 url 상에서 데이터를 찾는다.
  

아래 소스를 보면서 한번더 설명 하겠습니다.


```java
//헤더 url뒤에 붙은 데이터 = name=Lee&age=28 

@PostMapping("/receive")
public String test(@RequestParam String name) {
    System.out.println(name); // Lee 출력
    return "result";
}
@PostMapping("/receive")
public String test(@RequestParam String name2) {
    System.out.println(name2); // 에러남 -> 기본적으로 url 상에서 데이터를 찾기 때문에 name2가 없으니 에러가 난다.
    //기본적으로 반드시 name2 파라미터가 전송되어야 한다.
    return "result";
}
@PostMapping("/receive")
public String test(@RequestParam Map<String, Object> commandMap) {
    System.out.println(commandMap); // HashMap {name=LEE,age=28} 출력 
    //여기서 주의점!! 28은 String 형식으로 출력됨
    return "result";
}

```


## 정리면서


정리하면서 테스트를 하다보니 


생각해보니 POST방식은 Body에 데이터를 담아 전송시켜야하는데


@requestParam는 url 상에서 데이터를 찾고


RequestParam와 Post랑은 같이 사용하면 안되는거 아닐까? 라고 생각했습니다.


그래서 이 부분은


추후에 content-type 정리하면서 같이 한번더 정리하겠습니다.


