---
Author: CarefreeLife98
Date: 2024-03-09T19:54:00
Agenda: 
tags:
---
# 웹 애플리케이션과 싱글톤
> Web Application 특성 상 **대규모 트래픽**을 다루게 된다.
> - **이때 매 요청, 응답 과정 마다 새로운 객체를 만들어 반환하게 되면 JVM 메모리에 새로운 객체가 끊임없이 load 된다.**

## 다중 호출 후 생성된 객체 간 참조값 비교 테스트
```java
public class SingletonTest {  
  
    @Test  
    @DisplayName("스프링 없는 순수한 DI Container")  
    void pureContainer() {  
        AppConfig appConfig = new AppConfig();  
  
        // 1. 조회: 호출할 때마다 새로운 객체를 생성.  
        MemberService memberService1 = appConfig.memberService();  
  
        // 2. 조회: 호출할 때마다 새로운 객체를 생성.  
        MemberService memberService2 = appConfig.memberService();  
  
        // 참조 값이 다른 것을 확인  
        System.out.println("memberService1 = " + memberService1);  
        System.out.println("memberService2 = " + memberService2);  
    }  
}
```

### 테스트 결과
```
memberService1 = hello.core.member.MemberServiceImpl@71a794e5
memberService2 = hello.core.member.MemberServiceImpl@76329302

Process finished with exit code 0
```
- 같은 함수를 두번 호출했을때, 반환되는 객체는 서로 다른 객체임을 알 수 있다. (객체의 참조 값이 다름)
- 만약 내가 만든 Web Application 의 TPS(Transaction per Seconds) 가 50000 이라면, 초당 50000 개의 객체를 생성하여 JVM 의 메모리에 load 되는 것과 같다.
	- **매우 비효율적인 요청 처리방식. -> 메모리 낭비가 심함**
- **해결 방안:**
	- **공유할 수 있는 객체를 단 하나만 생성한다. (= 싱글톤 패턴)**

<br><br>

# 싱글톤 패턴 사용하기

```
싱글톤 패턴
- 클래스의 인스턴스가 단 1개만 생성되는 것을 보장하는 디자인 패턴.
- 객체 인스턴스를 2개 이상 생성하지 못하도록 막자.
	- private 생성자를 사용하여 외부에서 임의로 new 키워드를 사용하지 못하도록 막아야 한다.
```

