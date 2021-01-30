---
layout: post
title:  "[spring] Filter와 Interceptor의 차이"
subtitle:   "Filter VS Interceptor"
categories: dev
tags: spring
---





jwt를 구현하면서 필터 관련 개념이 나왔고


지난 언젠가 XSS처리를 필터로 해준 기억이 있었기 때문에


다시한번 정리하려고 한다.



## Spring MVC request lifecycle
![img](https://chung10kr.github.io/assets/img/2021-01-29-1.PNG)




필터와 인터셉터의 가장 큰차이는 위 그림에서 볼 수 있다.


## 차이점
|  | Filter | Interceptor |
|---|:---:|---:|
| 위치 | Dispatcher servlet의 앞단 | Dispatcher servlet에서 Handler로 가기 전 |
| 기능| 웹 어플리케이션의 Context의 기능 | 스프링의 Spring Context의 기능|
| 제공 | J2EE 표준 스펙에 정의 | Spring Framework 자체적으로 제공 |
| 사용처 | webApp 전역적으로 처리해야 하는 로직(인코딩,보안관련 처리 XSS) | 디테일한 처리(인증,권한)  |


---------------------------------------

[filter sample code - XSS]()

[Interceptor의 sample code - XSS]()