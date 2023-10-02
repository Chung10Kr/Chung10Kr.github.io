---
layout: post
title:  "[Spring] DI , IOC 이란?"
subtitle:   "DI"
categories: dev
tags: note
---

# DI

## 의존성?


의존한다는것..누구에게 의지한다는것..지금 내가 누군가를 필요로 한다는것이다.


지금 내가(함수 or 클래스)가 누군가를 필요로 한다는뜻이다.

```java
class Baseball{
	private Ball ball;

	public Baseball(){
		this.ball = new Ball();
	} ;
	public playBall(){
		this.ball.hit();
	};
}
```

여기서 playBall가 실행되기 위해서는 Ball클래스가 필요하다.


***Baseball클래스는 Ball클래스의 의존성을 가진다.***


Ball을 수정할때 Baseball을 수정해줘야 할 수도 있는 문제가 발생!


즉, 결합도가 높아지게 된다.






## 의존성 주입? - 

객체 자체가 아니라 Framework에 의해 객체의 의존성이 주입되는 설계 패턴


위에서 본것처럼 의존성은 제거해야한다.!


의존성을 제거했다.


의존성을 제거했기 때문에 다시 의존성을 주입해줘야 한다. 


누가? Framework가

## 의존성 주입을 하는 이유?

-Unit Test가 용이해진다.
-코드의 재활용성을 높여준다.
-객체 간의 의존성을 줄이거나 없앨 수 있다.
-객체 간의 결합도를 낮추면서 유연한 코드를 작성 할 수 있다.



### 의준성 주입 방법


-Contructor Injection : 생성자를 통한 전달



```java
class Baseball{
	private Ball ball;

	public Baseball(Ball ball){
		this.ball = ball;
	} ;
	public playBall(){
		this.ball.hit();
	};
}
```


-Method(Setter) Injection : setter()를 통한 전달



```java
class Baseball{
	private Ball ball;
	
	@Autowired
	public setBall(Ball ball){
		this.ball = ball;
	} ;
}

```

-Field Injection : 멤버 변수를 통한 전달
```java
class Baseball{
	
	private Ball ball;

}

```





# IOC

IOC란  **I**nversion **o**f **C**ontrol의 줄임말로 제어의 역전이란 뜻이다.


## 제어의 역전
- 말 그대로 메소드, 객체의 호출작업(제어) 를 개발자가 결정하는 것이 아니라 외부에서 결정되는것(역전) 을 의미한다.

무슨말이냐 하면

예를 들어
클라이언트가 서버에 웹 페이지를 요청하면 서블릿 컨테이너가 스레드를 생성등 여러가지 작업 후
웹페이지가 실행된다.

우리 개발가 이 과정에 대한 작업을 전혀 하지 않는다.

**이렇게 원래 개발자가 가지고 있어야 할 객체의 생명주기 제어권이
컨테이너에게 넘어갔것이 제어가 역전되었다라고 한다.**


