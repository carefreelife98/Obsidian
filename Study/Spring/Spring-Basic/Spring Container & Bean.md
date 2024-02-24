---
Author: CarefreeLife98
Date: 2024-02-19T01:06:00
Agenda: 
tags:
  - Spring
---
# 스프링 컨테이너
> **Application Context 를 스프링 컨테이너**라고 함.
> - 기존에는 애플리케이션 구성 클래스(AppConfig) 등을 사용해서 직접 객체를 생성하고 DI(Dependency Injection)를 함.
> - **스프링에서는 구성 정보 설정을 스프링 컨테이너를 통해서 사용.**
> - 스프링 컨테이너는 **`@Configuration`** 이 붙은 구성 클래스를 설정(구성)정보로 사용.
> 	- **`@Bean`** 이라 적힌 메서드를 **모두 호출해서 반환된 객체를 스프링 컨테이너에 등록.**
> 	- 위처럼 **스프링 컨테이너에 등록된 객체를 Spring Bean 이라고 함.**
> - 스프링 빈은 **`@Bean` 이 붙은 메서드의 명을 스프링 빈의 이름으로 사용.**
> - 스프링 컨테이너를 통해서 필요한 스프링 빈(객체) 를 찾아야 함.
> 	- **`applicationContext.getBean("빈 이름", 반환 클래스)`** 를 통해 스프링 빈을 찾을 수 있다.


```
[기존]:
기존에는 개발자가 직접 자바코드로 모든 것을 함. 

[스프링 사용]:
스프링 컨테이러에 객체를 스프링 빈으로 등록하고, 스프링 컨테이너에서 스프링 빈을 찾아 사용하도록 변경되었다.
```


# ApplicationContext
> - ApplicationContext 는  Interface 이다.
> - 스프링 컨테이너의 생성 방식 두 가지
> 	- XML
> 	- Annotation 기반의 JAVA 설정 클래스

## Annotation 기반의 JAVA 설정 클래스를 통한 생성
```java
ApplicationContext
```