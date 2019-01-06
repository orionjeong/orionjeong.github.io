---
layout: post
title:  "디자인패턴[팩토리 메서드 패턴(factory method pattern)]"
date:   2018-11-19
description: 템플릿 메서드 패턴에 대한 내용입니다.
tags: [디자인패턴, spring, java]
category: 디자인패턴
---
#### 정의

팩토리메서드패턴은 서브클래스에서 구체적인 오브젝트 생성 방법을 결정하게 하는 것을 말한다. 즉 원래는 슈퍼클래스에서 어떠한 오브젝트를 생성해서 사용하고 있었는데 이를 서브클래스에 위임하는 것을 말한다. 



![image-20181117235157600](/assets/img/팩토리메서드패턴.png)

쉽게 UML 클래스 다이어그램을 설명하자면 먼저 Creator를 super클래스 Creator1을 Creator를 상속받는 sub클래스라고 생각해보자. super클래스에서는 action메서드 안에서 factoryMethod를 호출해서 product를 가져오고 있다. 그리고 factoryMethod()를 sub클래스에서 구현하고 있다. 즉 위에서 설명한대로 Product오브젝트를 생성하는 구체적인 방법을 super클래스에서 하지 않고 sub클래스에 맡긴 모습이다. 



#### 예제

```java
public interface Product {
 	int getPrice();
}

public class ComputerProduct implement Product{
    @Override
    int getPrice(){
        return 5000;
    }
}

public class SmartPhoneProduct implement Product{
    @Override
    int getPrice(){
        return 4000;
    }
}

public abstract class Dealer{
    public Product sale(int price){
        ***
        Prodcut product = factoryProdcut();
        ***
    }
    // super클래스에서 sub클래스한테 Product 객체의 관심을 넘김
	protcted abstract Product factoryProduct();
}

public class SmartPhoneDealer extends Dealer{
    // sub클래스에서 Product 객체의 설계를 관리
    @Override
   	public Product factoryProduct(){
		***
        Prodcut product = new SmartPhoneProdcut();
        ***
        return product
    }
}
 
```









#### 자료참조

토비의 스프링 3.1

https://en.wikipedia.org/wiki/Factory_method_pattern 