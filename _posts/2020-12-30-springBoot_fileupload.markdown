---
layout: post
title:  "[Spring Boot] MultipartFile 업로드"
subtitle:   "Spring Boot MultipartFile 업로드"
categories: dev
tags: spring
---

Spring Boot MultipartFile 업로드 및 Postman에서 MultipartFile Send하기!

파일업로드 구현을 간단하게 구현해봤습니다.

1. FileController

```java
@RequestMapping(value = "/save", method = RequestMethod.POST)
public String saveSell(@RequestParam("filename") MultipartFile files) throws Exception {
    //파일 input name가 filename으로 되어야 합니다.

    //파일이 저장될 위치
    String baseDir = "C:\\DEV\\project\\files";;
    
    //baseDir의 날짜별로 폴더 셋팅
    Date dt = new Date();
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
    String datefolder =sdf.format(dt).toString();
    File dir = new File(baseDir, datefolder);
    //날짜별 폴더가 없을경우 생성
    if(!dir.exists()){
        dir.mkdirs();
    };

    String filePath = dir +"\\"+ files.getOriginalFilename();

    //파일 저장
    files.transferTo(new File(filePath));
    
    /*
    파일 정보 DB 저장 생략
    */
    return "SAVE OK";
}

```

2. application.properties
``` 
# 파일 업로드 용량 제한
spring.servlet.multipart.maxFileSize: 10MB
spring.servlet.multipart.maxRequestSize: 10MB
```



3. postman 에서 MultipartFile Send 하여 Test

평소 postman 사용하듯이 기본정보 셋팅하고


Body -> form-data -> key 입력 -> 파란색 원 잘보면 select box의 file 선택 -> value에서 파일 선택 -> sned


크게 어렵거나 다른것은 없다.

![img](https://chung10kr.github.io/assets/img//2020-12-30-1.png)


