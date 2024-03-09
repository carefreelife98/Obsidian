---
Author: CarefreeLife98
Date: 2024-03-09T18:09:00
Agenda: 
tags:
  - Spring
---
# 다양한 설정 형식(Configuration) 지원 - 자바코드 및 XML

```
스프링 컨테이너는 다양한 형식의 설정 정보를 사용할 수 있도록 유연하게 설계 되어 있다.
- Java
- XML
- Groovy
```

![[스크린샷 2024-03-09 오후 6.15.00.png]]
- **AnnotationConfigApplicationContext**
	- Java 의 설정 클래스를 사용하여 스프링의 설정 정보를 구성.
- **GenericXmlApplicationContext**
	- XML 파일을 사용하여 스프링의 설정 정보를 구성.
- ~ ApplicationContext
	- **사용자가 Customizing 한 파일로 스프링의 설정 정보를 구성할 수도 있다.**

<br><br>

## Annotation 기반 Java 클래스를 활용한 설정 사용하기
```java
// AppConfig.class 라는 설정 클래스를 통해 설정 정보 구성
AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class)
```
- `AnnotationConfigApplicationContext` 를 사용하여 설정 클래스를 파라미터로 넘겨주면 된다.

## XML 설정 사용

> 근래에는 스프링 부트를 많이 사용하면서 XML 기반 설정 정보 구성 방식은 잘 사용하지 않는다고 한다.
> - 하지만 어느정도 규모 및 연차가 있는 회사에 취업한다면, 대부분 XML 설정을 사용하고 있을 것이다. (본인도 취직 후 Spring Framework / XML 사용 중에 있다.)

```java
// GenericXmlApplicationContext 를 사용하며 .xml 로 만들어진 설정 파일을 넘겨주자.
GenericXmlApplicationContext xac = new GenericXmlApplicationContext("classpath:applicationContext.xml"
```

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <bean id="memberService" class="hello.core.member.MemberServiceImpl">  
        <constructor-arg name="memberRepository" ref="memberRepository"/>  
    </bean>  
  
    <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository"/>  
  
    <bean id="orderService" class="hello.core.order.OrderServiceImpl">  
        <constructor-arg name="memberRepository" ref="memberRepository"/>  
        <constructor-arg name="discountPolicy" ref="discountPolicy"/>  
    </bean>  
  
    <bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy"/>  
  
</beans>
```

- \<bean> 태그를 사용하여 스프링 빈을 등록.
- Java 설정 클래스를 사용할 때와 비슷하게 1 : 1 매핑되는 것을 볼 수 있다.

# 스프링 빈 설정 메타 정보 - BeanDefinition

![[스크린샷 2024-03-09 오후 7.10.32.png]]
- 스프링은 `BeanDefinition` 을 통한 `추상화` 로서 다양한 설정 형식을 지원한다.
	- `추상화 : 역할과 구현을 개념적으로 나눈 것.`
		- XML 을 읽어서 BeanDefinition 을 생성.
		- Java 코드를 읽어서 BeanDefinition 을 생성.
		- **스프링 컨테이너는 설정 정보가 어떤 형식으로 정의 되어 있는지 알 필요 없이, BeanDefinition 만 알고 있으면 된다.**
- `BeanDefinition : 빈 설정 메타정보`
	- **`@Bean`, `<bean>` 당 각각 하나씩 메타 정보 생성.**
- **스프링 컨테이너는 이 메타 정보를 기반으로 스프링 빈을 생성.**

## ApplicationContext - AnnotationConfigApplicationContext
```
조금 더 깊이 있게 들어가보자.
```

![[스크린샷 2024-03-09 오후 7.14.18.png]]
`AnnotationConfigApplicationContext > AnnotatedBeanDefinitionReader`
- 위와 같이 AnnotationConfigApplicationContext 를 타고 들어가보면 **`AnnotatedBeanDefinitionReader`** 가 존재하는 것을 볼 수 있다.
	- `AnnotatedBeanDefinitionReader : 스프링 설정 정보를 읽어 적절한 BeanDefinition 을 생성하는 역할.`
	- 