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
