---
layout: post
title:  "[spring boot] 내장 톰캣 인코딩 문제"
subtitle:   "삽질2시간"
categories: dev
tags: spring
---

테스트 서버에 셋팅할때 Jar파일을 만들어 실행했다.


[참고 - Jar 파일 만들고 실행](https://chung10kr.github.io/dev/2020/12/26/springBoot_makJar/)


```
java -jar project.jar
```
로 실행하였지만 한글 깨진다.


별거 다해봤다....

```
java -Dfile.encoding=UTF-8 -jar project.jar
```

으로 하면 되드라.

결국 **파일 인코딩** 문제였다.

