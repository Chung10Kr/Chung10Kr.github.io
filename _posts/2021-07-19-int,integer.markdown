---
layout: post
title:  "[JAVA] Integer VS int"
subtitle:   ""
categories: dev
tags: note
--- 

## Int, Integer

|Int|Integer|
|---|---|
|primitive 자료형|Wrapper 클래스(객채)|
|산술연산 가능|Unboxing을 하지 않으면 산순 연산이 불가능|
|null로 초기화 할 수 없다.|null값을 처리할 수 있다.|

## Boxing, Unboxing

|Boxing|Unboxing|
|---|---|
|Primitive 자료형 -> Wrapper 클래스 |Wrapper 클래스->Primitive 자료형|


```java
    //Int to Integer
    Integer li = new Integer(5);
    Integer li2 = new Integer("5");
    Integer li2 = new Integer("오"); // 숫자로 파싱할 수 없는 String은 에러
    /*
    The constructor Integer(int) has been deprecated since version 9 and marked for removalJ (자바 9부터 new Integer는 추천안함)
    Integer li2 = Integer.valueOf(5); -> 이거를 추천함
    */

    // Integer to Int
    int i = li.intValue();
```

```java
    Integer li1 =  Integer.valueOf("3"); 
    Integer li2 =  Integer.valueOf("3");
    System.out.println(  li1+li2 ); // 6
```

?? Integer는 산술연산 안된다면서 산술연산 결과값이 제대로 나오는데? -> Javasms


## Autoboxing 

산술연산 안된다면서 되는것은 Java에서 Autoboxing을 지원하기 때문

![img](https://chung10kr.github.io/assets/img/2021-07-19-1.PNG)

(번역하면, Autoboxing은 자바 컴파일러가 primitive형과 Wrapper 클래스 사이에서 만드는 자동 변환 입니다. 예를 들면 
int 에서 Integer로 변환, dobble에서 Double로 변환, 기타 등등)

int -> Integer 로 변환 (Auto boxing)
Integer -> int 로 변환 ( Auto unboxing)

