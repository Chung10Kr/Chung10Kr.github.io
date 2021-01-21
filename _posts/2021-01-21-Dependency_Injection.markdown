---
layout: post
title:  "[spring] Dependency Injection이란?"
subtitle:   "DI"
categories: dev
tags: spring
---


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


