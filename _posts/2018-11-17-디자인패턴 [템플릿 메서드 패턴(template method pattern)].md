---
layout: post
title:  "디자인패턴[템플릿 메서드 패턴(template method pattern)]"
date:   2018-11-17
description: 템플릿 메서드 패턴에 대한 내용입니다.
tags: [디자인패턴, spring, java]
category: 디자인패턴
---




## 디자인패턴[템플릿 메서드 패턴(template method pattern)]



#### 정의

슈퍼클래스에 기본적인 로직의 흐름을 만들고, 그 기능의 일부를 추상메소드나 오버라이딩이 가능한 protected 메소드 등으로 만든 뒤 서브클래스에서 이런 메소드를 필요에 맞게 구현해서 사용하도록 하는 방법을 말한다. 

슈퍼클래스서의 기능 일부의 관심을 서브클래스한테 줌으로 써 클래스 계층구조(슈퍼클래스-서브클래스)를 통해 두 개의 관심이 독립적으로 분리되면서 변경 작업이 한층 용이해지고 확장이 쉬워진다. 


#### 예제

```java
// super클래스 스마트폰
public abstract class 스마트폰 {
    protected abstract 번호 번호찾기(String 친구이름);
    public final void 전화하기(String 친구이름){
        번호찾기(친구이름);
        ***
    }
    public final void 문자하기(String 친구이름){
        번호찾기(친구이름);
        ***
    }
}
// sub클래스 안드로이드 스마트폰
public class 안드로이드스마트폰 extends 스마트폰 {
    @Override
    protected 번호 번호찾기(String 친구이름){
        //안드로이드 전화부에서 번호 찾는코드
        ***
        return 번호;
    }
}

//sub클래스 애플 스마트폰
public class 애플스마트폰 extends 스마트폰 {
    @Override
    protected 번호 번호찾기(String 친구이름){
        //아이폰 전화부에서 번호 찾는 코드
        ***
        return 번호;
    }
}

//test
public class CodeTest {
    public static void main(String agrs[]){
		안드로이드스마트폰 갤럭시노트 = new 안드로이드스마트폰();
        갤럭시노트.전화하기(친구이름);
        
        애플스마트폰 아이폰 = new 애플스마트폰();
        아이폰.전화하기(친구이름);
    }
}

```















####자료 참조 

http://egloos.zum.com/iilii/v/3806897 

토비의 스프링 3.1

