---
layout: post
title:  "예외(Exception)처리"
date:   2019-03-23
description: 오류와 예외에 대해서 정리한 내용입니다. 
tags: [Exception, java, 오류, 예외, Error]
category: java
---

제가 정말 중요한 순간에 Error에 대해서 정확한 이해가 없어서 저 자신한테 정말 큰 실망은 한 적이 있습니다. 그래서 블로그에 정리를 하고 다시는 까먹지 않도록 고정시키려합니다. 

<br/>

### 오류(Error)와 예외(Exception)

<br/>

**오류(Error)**는 시스템에 비정상적인 상황이 생겼을 때 발생합니다. 시스템 레벨에서 발생하기 때문에 개발자가 미리 예측하여 처리할 수 없기 때문에 오류에 대한 처리를 신경쓰지 않아도 됩니다. 

<br/>

**예외(Exception)**는 개발자가 구현한 로직에서 발생합니다. 그렇기 때문에 예외는 발생할 상황을 미리 예측하여 처리할 수 있습니다. 즉 예외에 대해서는 개발자가 신경을 써야합니다.

<br/>

<br/>

### 예외 클래스

<br/>

![image-20190323170906664](/assets/img/image-20190323170906664.png)

<br/>

여기서 우리가 주의깊게 봐야 하는 건 Exception클래스입니다. 여기서 더 주목해야 하는 건 RuntimeException입니다. RuntimeException은 CheckedException과 UncheckedException을 구분하는 기준입니다.

<br/>

RuntimeException과 이를 상속하는 모든 클래스는 CheckedException이며 그외의 Exception을 NonCheckedException이라고 합니다. 

<br/> <br/>

### CheckedException과 UncheckedException

|                           | Checked Exception                                            | Unchecked Exception           |
| ------------------------- | ------------------------------------------------------------ | ----------------------------- |
| 처리여부                  | 반드시 예외를 처리해야 함                                    | 명시적인 처리를 강제하지 않음 |
| 확인시점                  | 컴파일 단계                                                  | 실행단계                      |
| 예외발생 시 트랜잭션 처리 | roll-back하지 않음                                           | roll-back함                   |
| 대표 예외                 | Exception의 상속받는 하위 클래스 중 Runtime Exception을 제외한 모든 예외 | RuntimeException 하위 예외    |

<br/>

**Unchecked Exception** 

<br/>

RuntimeException을 상속받은 경우 명시적인 예외처리를 강제하지 않기 때문에 Unchecked Exception이라고 부릅니다. 

<br/>

에러와 마찬가지로 이 런타임 예외는 catch문으로 잡거나 threw로 넘기지 않아도 됩니다. 왜냐하면 RuntimeException에 속하는 클래스들은 주로 개발자의 실수(실수는 개발자가 직접 처리 가능)에 의해 발생되는 예외들이 많기때문입니다. 

<br/>

예를들어 ArithmeticException, IndexOutOfBoundsException, NullPointerException, IllegalArgumentException등이 있습니다.  

<br/>

한 가지 더 Uncheck Exception은 예외가 발생하면 트랜잭션을 roll-back합니다. 

<br/>

**Checked Exception**

<br/>

RuntimeException을 상속받지 않은 모든 Exception들은  CheckedException이라고합니다. CheckedException은 개발자의 실수보다는 사용자의 실수와 같은 외적인 요인 의해 발생하는 예외들이 많습니다. 예를 들어 FileNotFoundExcetion은 존재하지 않는 파일의 이름을 사용자가 입력하여 찾을 때 사용됩니다. 

<br/>

이렇게 때문에 CheckedException은 반드시 예외처리를 강제합니다. 

<br/>

**Unchecked과 Checked Exception의 트랜잭션 처리**

<br/>

 Uncheck Exception은 예외가 발생하면 트랜잭션을 roll-back합니다. 

<br/>

 Checked Exception은 예외가 발생하면 트랜잭션을 roll-back하지 않고 예외를 던져줍니다.

<br/>

위와같은 이유로 트랜잭션의 roll-back이 되는 범위가 달라질 수 있습니다. 이에 대한 방식을 개발자는 이해하고 있어야 실행결과가 예상치 못한 방향으로 가는 것을 막을 수 있습니다.

<br/>

<br/>

### Exception Handling

<br/>

이제 우리가 체크해야하는 Checked Exception을 어떻게 다뤄야 하는지에 대해서 얘기를 해보도록 하겠습니다.

<br/>

**예외복구**

<br/>

예외복구는 예외가 발생하여도 애플리케이션을 정상적인 흐름으로 진행하도록 하는 것을 말합니다. 

<br/>

**예외처리 회피**

<br/>

예외가 발생하면 throws를 통해서 호출한 쪽으로 예외를 던지고 그 처리에서 회피하는 것을 말합니다. 

<br/>

**예외 전환**

<br/>

예외가 발생했을 때 catch에서 다른 Exception을 던져서 예외를 전환하는 것을 말합니다. 

<br/>

<br/>

### 결론

<br/>

예외처리는 아주 신중히 접근해야 하는 문제입니다. 결국은 서비스에서 문제가 발생했다는 것을 말하니까요. 또한 예외처리를 통해서 문제를 얼마나 빨리 파악할 수 있느냐의 단서를 얻을 수 있기 때문에 예외처리를 할 때는 한 번더 생각을 해야할것입니다. 

<br/>

<br/>

### 참고자료

<br/>

http://www.nextree.co.kr/p3239/

http://blog.naver.com/PostView.nhn?blogId=serverwizard&logNo=220789097495