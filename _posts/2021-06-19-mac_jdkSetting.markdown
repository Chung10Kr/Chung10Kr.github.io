---
layout: post
title:  "mac jdk 설치 및 환경변수 설정 - 개인저장용"
subtitle:   ""
categories: dev
tags: etc
--- 


# JDK 설치

1. 오라클 홈페이지에서 mac용 jdk 설치
2. 하라는데로 설치
3. 터미널에서 
```
java -version 
```
으로 설치 확인

JDK 기본 설치 경로는 **/Library/Java/JavaVirtualMachines** 이다

# 환경변수 설정

1. 터미널이동
2. HOME으로 이동
```
cd ~/  
vi .bash_profile
```

아래늬 내용을 입력 후 저장한다.

* jdk 버전은 각자 상황에 맞게 수정해야함.

```
JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-16.0.1.jdk/Contents/Home
PATH=$PATH:$JAVA_HOME/bin
export JAVA_HOME
export PATH
```

3. vi편집기 저장 후 source명령어를 이용해 환경변구 설정 적용함
```
source .bash_profile
```

4. 확인
```
echo $JAVA_HOME
javac -version
```

5. Zsh에서 PATH기억시키기
```
//vi 에디터를 연다.
vi ~/.zshrc

// 작성
source ~/.bash_profile

```
