---
layout: post
title:  "Gradle + Spring Boot Jar 파일 만들고 실행하기"
subtitle:   "Gradle + Spring Boot Jar 파일 만들고 실행하기"
categories: dev
tags: spring
---

### Gradle + Spring Boot Jar 파일 만들고 실행하기

## build.gradle 수정

 apply plugin: 'io.spring.dependency-management'
 

## CMD로 해당 경로에서

gradle bootjar

## 생선된 jar파일 실행
생성된 파일은 build/libs에 있다.

 java -jar project-0.1.jar