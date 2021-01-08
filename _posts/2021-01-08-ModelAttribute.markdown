---
layout: post
title:  "[spring] @ModelAttribute"
subtitle:   "@ModelAttribute"
categories: dev
tags: spring
---

# @ModelAttribute


DOC를 구글 변역기로 돌려보면


> @ModelAttribute컨트롤러에는 두 가지 사용 시나리오가 있습니다.
>
> 메소드 매개 변수에 배치하면 @ModelAttribute모델 속성을 주석이 달린 특정 메소드 매개 변수에 맵핑합니다
> 이것이 컨트롤러가 양식에 입력 된 데이터를 보유하는 객체에 대한 참조를 얻는 방법입니다.
> 
> @ModelAttribute메서드 수준에서 사용 하여 모델 에 대한 참조 데이터 를 제공 할 수도 있습니다
>이 사용법을 위해 메소드 서명은 @RequestMapping주석 에 대해 이전에 문서화 된 것과 동일한 유형을 포함 할 수 있습니다 .
>
라고 한다.


# 매개변수에 배치

#### @RequestParam
```java
@Controller
public class testController{
    @getMapping("/")
    public String getTest(@RequestParam("name") string name){
        System.out.println("이름 : " + name);
        return "test";
    }
}
/*
요청값 -> /?name=Lee
결과값 -> 이름 : Lee
*/
```

#### @ModelAttribute
```java
@Getter
@Setter
public class TestModel{
	private String name;
	private int age;
}

@RestController
public class TestController{
	@GetMapping("/")
	public String getTest(@ModelAttrivute TestModel testmodel){
		System.out.println("이름 : " + testmodel.getName());
		System.out.println("나이 : " + testmodel.getAge());

        

	}
}
/*
/*
요청값 -> name=Lee&age=10
결과값 -> 이름 : Lee
          나이 : 28
*/
```
### 언듯보면 @RequestParam 과 같으면서 다르다. 
- 파라미터로 넘겨 준 타입의 오브젝트를 자동으로 생성한다
- @RequestParam에서 유저를 만들려면 각각의 파라미터를 전부 받아서 모델을 생성해줘야한다.
- 유저를 만들기 위해 (@ModelAttrivute User user) 이면 끝


```java
@RequestMapping("/test")
public ModelAndView draw(@ModelAttribute("specified") MyModel myModel) {
    // ...
}
```
### 이후 VIEW에선 'specified'를 request.attribute에서 가져올 수 있다





# 매서드 수준에서 사용


-컨트롤러에서 뷰에 전달할 일종의 공통 모델을 설정한다.


-메서드가 리턴하는 값은 Request객체에 전달되며 전달 범위는 해당 메소드가 존재하는 컨트롤러 전체에 해당된다. 


-@ModelAttribute 어노테이션이 적용된 메서드는 컨트롤러가 자동으로 호출한다. 단, 매 요청마다 반복 실행되므로 효율성을 고려해야 한다.

```java
@ModelAttribute("myObject")
public String refModelTest() {
    return "hello-o";
    // request.setAttirubte("myObject", "hello-o") 와 같으며
    // 선언된 클래스의 전역으로 실행된다.
}
```
```java
@ModelAttribute("user")
public Department getUserInfo(@RequestParam("userId") String userId){
	return userService.getUserInfo(userId); //DB에서 부서정보 데이터를 가져온다.
}
/*
화면에서 ${user.user_name} , ${user.user_age} 이렇게 쓰겠지
*/

```


## 정리 
- 파라미터로 넘겨 준 타입의 오브젝트를 자동으로 생성한다
- 객체가 자동으로 Model객체에 추가되고 뷰로 전달된다.
