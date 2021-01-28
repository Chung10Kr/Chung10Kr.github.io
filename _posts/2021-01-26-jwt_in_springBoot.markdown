---
layout: post
title:  "[spring boot] Spring Security + JWT with Spring Boot "
subtitle:   "인증2탄"
categories: dev
tags: spring
---

서버기반인증 VS 토큰기반인증 에 이어서 JWT + Spring Security를 알아보장.



[지난 포스팅 - 서버기반인증 VS 토큰기반인증](https://chung10kr.github.io/dev/2021/01/23/authJwt/)


# Spring Security
>Spring Security 는 엔터프라이즈 애플리케이션을위한 인증 , 권한 부여 및 기타 보안 기능을 제공 하는 Java / Java EE 프레임 워크입니다 . 이 프로젝트는 이름 충돌을 방지하기 위해 2003 년 말 'Acegi Security'( Ah-see-gee / ɑː s iː dʒ iː / 라고 발음 하며 영어 알파벳의 첫 번째, 세 번째, 다섯 번째 및 일곱 번째 문자로 발음 됨)로 시작되었습니다. [2] ) Ben Alex에 의해 2004 년 3 월 Apache 라이선스 에 따라 공개적으로 출시되었습니다 . 그 후 Acegi는 Spring 에 통합되었습니다. 공식 Spring 하위 프로젝트 인 Spring Security로 포트폴리오. 새 이름의 첫 번째 공개 릴리스는 2008 년 4 월 Spring Security 2.0.0이었으며 SpringSource 에서 상업적 지원 및 교육을 제공했습니다 . - 위키백과


- spring 기반의 **어플리케이션의 보안(인증,권한등)을 담당하는 스프링 하위 프레임워**크다.
- Spring Security는 **인증과 권한에 대한 부분을 Filter 흐름에 따라 처리**한다.

# JWT (JSON Web Token)
>JSON 웹 토큰(JSON Web Token, JWT, "jot”[1])은 선택적 서명 및 선택적 암호화를 사용하여 데이터는 만들기 위한 인터넷 표준으로, 페이로드는 몇몇 클레임(claim) 표명(assert)을 처리하는 JSON을 보관하고 있다. 토큰은 비공개 시크릿 키 또는 공개/비공개 키를 사용하여 서명된다. 이를테면 서버는 "관리자로 로그인됨"이라는 클레임이 있는 토큰을 생성하여 이를 클라이언트에 제공할 수 있다. 그러면 클라이언트는 해당 토큰을 사용하여 관리자로 로그인됨을 증명한다. 이 토큰들은 한쪽 당사자의 비공개 키(일반적으로 서버의 비공개 키)에 의해 서명이 가능하며 이로써 해당 당사자는 최종적으로 토큰이 적법한지를 확인할 수 있다. 일부 적절하고 신뢰할만한 수단을 통해 다른 당사자가 상응하는 공개키를 소유하는 경우 이 경우 또한 토큰의 적법성 확인이 가능하다. 토큰은 크기가 작고[2] URL 안전으로 설계되어 있으며[3] 특히 웹 브라우저 통합 인증(SSO) 컨텍스트에 유용하다. JWT 클레임은 아이덴티티 제공자와 서비스 제공자 간(또는 비즈니스 프로세스에 필요한 클레임)의 인가된 사용자의 아이덴티티를 전달하기 위해 보통 사용할 수 있다.[4][5] - 위키백과



- 전자 서명 된 URL-safe( URL로 이용할 수 있는 문자 만 구성된)의 JSON이다.
- 속성정보를 JSON으로 표현한 토큰으로 RFC7519표준이다.
- 사용자 인증에 필요한 정보를 토큰 자체에 포함하기 때문에 서버의 세션등을 저장 할 필요 없다.(stateless)



## JWT 인증 과정


![img](https://chung10kr.github.io/assets/img/2021-01-26-1.PNG)




# Sample Code

[Git Code](https://github.com/Chung10Kr/jwt_code.git)


1. request - /jwt/get/token with parameter(userId , userNm)
2. return token
3. request - /jwt/get/userInfo with header(token)
4. return userInfo

