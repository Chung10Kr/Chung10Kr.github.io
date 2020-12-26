---
layout: post
title:  Gradle + Spring Boot Jar 파일 만들고 실행하기
subtitle:   Gradle + Spring Boot Jar 파일 만들고 실행하기
categories: dev
tags: spring
---

Spring Boot는 내장 서버가 있기 때문에 Jar 파일을 만들어 배포가 쉽다.


톰캣설치부터 이것저것 할 필요 없이 기본 Jar로 배포 해보자

1\. build.gradle 수정

```
 apply plugin: 'io.spring.dependency-management'
```

2\. CMD로 해당 경로에서

```
gradle bootjar
```

3\. 생성된 jar파일 실행

생성된 파일은 build/libs에 있다.

```
 java -jar project-0.1.jar
```