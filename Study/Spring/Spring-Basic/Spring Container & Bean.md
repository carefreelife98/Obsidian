---
Author: CarefreeLife98
Date: 2024-02-19T01:06:00
Agenda: 
tags:
  - Spring
---
# 스프링 컨테이너
> **Application Context 를 스프링 컨테이너**라고 함. (더 정확히는 BeanFactory 와 구분 하지만 일반적으로 BeanFactory 를 직접 사용하는 경우는 거의 없음)
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
> - ApplicationContext 는  **Interface** 이다.
> - **스프링 컨테이너의 생성 방식 두 가지**
> 	- XML
> 	- Annotation 기반의 JAVA 설정 클래스

## Annotation 기반의 JAVA 설정 클래스를 통한 생성
```java
// 자바 기반 설정 클래스인 AppConfig.class 사용
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class)
```
> - **AnnotationConfigApplicationContext( )** 는 ApplicationContext **인터페이스의 구현체**.

# 스프링 컨테이너의 생성 과정

## 1. 스프링 컨테이너 생성
 > ![[스크린샷 2024-02-24 오후 8.17.57.png]]
 > 1. new AnnotationConfigApplicationContext(설정 / 구성 클래스)
 > 	- 위 구현체 생성 시  스프링 컨테이너가 생성됨.
 > 2. 스프링 컨테이너 내부에는 스프링 빈 저장소가 존재.(Key-Value 형태)
 > 	- Key : 스프링 빈의 이름
 > 	- Value : 스프링 빈 객체
 > 3. 스프링 컨테이너는 생성 시에 사용한 설정 / 구성 클래스 의 정보를 보고 스프링 빈 등록.
 
## 2. 스프링 빈 등록
> ![[스크린샷 2024-02-24 오후 8.24.56.png]]
> 스프링 컨테이너는 생성 시에 넘겨 받은 설정 정보 클래스에서 **@Bean Annotation**이 붙은 메서드들을 모두 찾아 **스프링 빈 저장소에 등록**한다.
> 	- **Key : 메서드 이름** (옵션 name="" 을 사용해 직접 부여도 가능)
> 		```java
> 		@Bean(name="carefreelife")
> 		```
> 		- **스프링 빈 이름은 유일한 이름을 부여**해야 한다.
> 			- 같은 이름을 가진 빈 존재 시 무시되거나 Overwrite 되는 빈이 생기고, 오류가 발생할 수 있다.
> 	- **Value : 메서드 리턴 값 (객체)**

## 3. 스프링 빈 의존관계 설정
> ![[스크린샷 2024-02-24 오후 9.25.57.png]]
> - 스프링 컨테이너는 등록된 설정 정보를 사용해서 의존 관계를 주입한다. (Dependency Injection)
> - 객체 인스턴스 간 **의존관계를 동적으로 주입**.

# 스프링 빈 조회

## Overview
```java
AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);  
  
@Test  
@DisplayName("모든 빈 출력하기")  
void findAllBean() {  
    String[] beanDefinitionNames = ac.getBeanDefinitionNames();  
  
    for (String beanDefinitionName : beanDefinitionNames) {  
        Object bean = ac.getBean(beanDefinitionName);  
        System.out.println("name = " + beanDefinitionName + " Object = " + bean);  
    }  
}
```

![[스크린샷 2024-02-25 오후 6.52.22.png]]
`실행 모습 - 직접 등록한 빈 뿐 아니라 스프링 내부 빈까지 출력되는 모습을 볼 수 있다.`
> - **모든 빈 출력하기**
> 	- 실행 시 **스프링에 등록된 모든 빈 정보를 출력**할 수 있다.
> 	- `ac.getBeanDefinitionNames()` : 스프링에 등록된 모든 빈 이름을 조회한다.
> 	- `ac.getBean()` : 빈 이름으로 빈 객체(인스턴스)를 조회한다.
<br><br>

```java
@Test  
@DisplayName("애플리케이션 빈 출력하기")  
void findApplicationBean() {  
    String[] beanDefinitionNames = ac.getBeanDefinitionNames();  
  
    for (String beanDefinitionName : beanDefinitionNames) {  
        // getBeanDefinition : 각 빈에 대한 MetaData 정보를 얻을 수 있음.  
        BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);  
  
        // getRole - BeanDefinition.ROLE_APPLICATION :  
        // - 스프링 내부 빈이 아닌 애플리케이션 개발을 위해 직접 등록한 빈.  
        // - 외부 라이브러리에 의해 생성되는 빈  
        // getRole - BeanDefinition.ROLE_INFRASTRUCTURE :  
		// - 스프링이 내부에서 사용하는 빈
        if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {  
            Object bean = ac.getBean(beanDefinitionName);  
            System.out.println("name = " + beanDefinitionName + " Object = " + bean);  
        }  
    }  
}
```

![[스크린샷 2024-02-25 오후 6.54.21.png]]
`실행 모습 - Bean 의 메타데이터 정보를 꺼내 ROLE_APPLICATION 에 해당하는 빈만 출력`
> - **애플리케이션 빈 출력하기**
> 	- 스프링이 내부에서 사용하는 빈은 제외하고, **개발자가 직접 등록한 빈 출력.**
> 	- 스프링이 내부에서 사용하는 빈은 **`getRole()` 메서드로서 구분**할 수 있다.
> 		- **`ROLE_APPLICATION` : 일반적으로 개발자가 개발을 위해 정의한 빈**
> 		- **`ROLE_INFRASTRUCTURE` : 스프링이 내부에서 사용하는 빈**

<br><br>

## 스프링 빈 조회 (기본)
```java
// 스프링 컨테이너에서 스프링 빈을 조회하는 가장 기본적인 방법
AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

// 방법 1
Object bean = ac.getBean(빈 이름, 타입);

// 방법 2
Object bean = ac.getBean(타입);

// 조회 대상 스프링 빈이 없으면 아래와 같은 예외가 발생한다.
`NoSuchBeanDefinitionException: No bean named '~' available`
```

### 스프링 빈 이름을 통한 빈 조회

```java
AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);  
  
@Test  
@DisplayName("빈 이름으로 조회")  
void findBeanByName() {  
	// ac.getBean("빈 이름", 빈 타입)
    MemberService memberService = ac.getBean("memberService", MemberService.class);  
    System.out.println("memberService = " + memberService);  
    System.out.println("memberService.getClass() = " + memberService.getClass());  
}
```

![[스크린샷 2024-02-25 오후 7.20.40.png]]
- 