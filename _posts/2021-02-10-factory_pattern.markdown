---
layout: post
title:  "[etc] 디자인 패턴"
subtitle:   "디자인패턴"
categories: dev
tags: note
---

디자인 패턴 - 객체 지향 프로그래밍 설계를 할 때 자주 발생하는 문제들을 피하기 위해 사용되는 패턴




# 디자인 패턴이란
- 객체 지향 프로그래밍 설계를 할 때 자주 발생하는 문제들을 피하기 위해 사용되는 패턴

> 다른 사람의 코드, 기존의 코드를 이해 하는것은 어렵다.
> 이런 코드를 수정하거나 새로운 기능을 추가하는데 의도치 않는 버그를  발생 시키고 성능을 취적화 시키기 어렵다.


> **디자인패턴**은 의사소통의 수단의 일종으로 이런 문제를 해결해준다.




## 1.생성패턴
#### 1.1 추상 팩토리(Abstract Factory)
- 구제적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공하는 패턴
#### 1.2 팩토리(Abstract Method)
- 객체 생성 처리를 서브 클래스로 분리해 처리하도록 캡슐화하는 패턴

### 적용안된 코드
```java
class Unit {
    Unit() {
        //생성자
    }
   //이하 유닛의 메소드들
};
class Marine extends Unit {
    Marine() {
        //생성자
    }
    //이하 마린의 메소드들
}
class Firebat extends Unit {
    Firebat() {
        //생성자
    }
    //이하 파이어뱃의 메소드들
}
```


```java
class Map {
    Map(File mapFile) {
        while(mapFile.hasNext() == true) {
            String[] unit = mapFile.getNext();
            if(unit[0].equals("Marine")) {
                Marine marine = new Marine(unit);
            } else if(unit[0].equals("Firebat")) {
                Firebat firebat = new Firebat(unit);
            }
           //기타 유닛의 생성자들
       }
       //유닛 초기화 이후 코드
    }
}

```
**이 경우 마린,파이어벳이 아닌 다른 유닛이 추가될 경우 Map 클래스를 수정해야 한다.!**

### 적용된 코드
```java
class UnitFactory {
    static Unit create(String[] data) {
        if(data[0].equals("Marine")) {
            return new Marine(data);
        } else if(data[0].equals("Firebat")) {
            return new Firebat(data);
        }
       //기타 유닛의 생성자들
    }
}
class Map {
    Map(File mapFile) {
        while(mapFile.hasNext() == true) {
            Unit unit = UnitFactory.create(mapFile.getNext());
       }
       //유닛 초기화 이후 코드
    }
}
```

#### 1.3 싱글톤(Singleton)
- 전역 변수를 사용하지 않고 객체를 하나만 생성하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴
```java
class Singleton {
    static final Singleton instance = new Singleton();
    private Singleton() {
        //초기화
    }
    public Singleton getInstance() {
        return instance;
    }
}

```
- 키보드 리더, 프린터 스풀러, 점수기록표 등 클래스의 객체를 하나만 만들어야 하는 경우 사용한다.
- 클래스 내에서 인스턴스가 단 하나뿐임을 보장하므로, 프로그램 전역에서 해당 클래스의 인스턴스를 바로 얻을 수 있고, 불필요한 메모리 낭비를 최소화한다.
- 생성자를 클래스 자체만 사용할 수 있도록 private 등의 접근제한자를 통하여 제한하여야 한다
- 또는  final로 reference를 변경 불가능하게 설정하여야 한다. 


## 2.구조패턴
#### 2.1 컴포지트
- 여러 개의 객체들로 구성된 복합 객체와 단일 객체를 클라이언트에서 구별 없이 다루게 해주는 패턴
#### 2.2 데코레이터
- 객체의 결합을 통해 기능을 동적으로 유연하게 확장할 수 있게 해주는 패턴

## 3.행위패턴
#### 3.1 전략
- 객체가 할 수 있는 행위들 각각을 전략으로 만들어 놓고, 동적으로 행위의 수정이 필요한 경우 전략을 바꾸는 것만으로 행위의 수정이 가능


### 적용안된 코드
```java
 public class Calculator {
    
    	public double calculate(boolean isFirstGuest, boolean isLastGuest, List<Item> items) {
    		double sum = 0;
    		for (Item item : items) {
    			if (isFirstGuest) {
    				sum += item.getPrice() * 0.9;
    			} else if (!item.isFresh()) {
    				sum += item.getPrice() * 0.8;
    			} else if (isFirstGuest) {
    				sum += item.getPrice() * 0.8;
    			} else {
    				sum += item.getPrice();
    			}
                // 새로운 가격등이 추가될 경우 수정하기 어렵다.
    		}
    		return sum;
    	}
    }
    
    public class Item {
    	private final String name;
    	private final int price;
    
    	public Item(String name, int price) {
    		this.name = name;
    		this.price = price;
    	}
    
    	public int getPrice() {
    		return price;
    	}
    
    	public boolean isFresh() {
    		return true;
    	}
    }
```

### 적용된 코드
```java
public interface DiscountPolicy {
    double calculateWithDisCountRate(Item item);
}

public class FirstCustomerDiscount implements DiscountPolicy{
    @Override
    public double calculateWithDisCountRate(Item item) {
        return item.getPrice() * 0.9;
    }
}

public class LastCustomerDiscount implements DiscountPolicy{
    @Override
    public double calculateWithDisCountRate(Item item) {
        return item.getPrice() * 0.8;
    }
}

public class UnFreshFruitDiscount implements DiscountPolicy{
    @Override
    public double calculateWithDisCountRate(Item item) {
        return item.getPrice() * 0.8;
    }
}
//==================================
public class Calculator {

    private final DiscountPolicy discountPolicy;

    public Calculator(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }

    public double calculate(List<Item> items) {
        double sum = 0;
        for (Item item : items) {
            sum += discountPolicy.calculateWithDisCountRate(item);
        }
        return sum;
    }
}
//==================================
public class FruitController {
    public static void main(String[] args) {
        Calculator calculator = new Calculator(new FirstCustomerDiscount());
        calculator.calculate(Arrays.asList(
            new Item("Apple", 3000),
            new Item("Banana", 3000),
            new Item("Orange", 2000),
            
        ));
    }
}
```



[참고 - kyle](https://velog.io/@kyle/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%A0%84%EB%9E%B5%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80)

[참고 - 나무위키](https://namu.wiki/w/%EB%94%94%EC%9E%90%EC%9D%B8%20%ED%8C%A8%ED%84%B4)