---
Author: CarefreeLife98
Date: 2024-03-02T18:40:00
Agenda: 
tags:
---
# BeanFactory 
- 스프링 컨테이너의 최상위 인터페이스.
- 스프링 빈을 관리하고 조회하는 역할을 담당한다.
- `getBean()` 메서드를 제공한다.

# ApplicationContext
- BeanFactory 기능을 모두 상속받아서 제공한다.
- BeanFactory 와의 차이점
	- 애플리케이션을 개발할 때에 빈을 관리하고 조회하는 기능 외에 추가적인 부가 기능을 필요로 한다.

## ApplicationContext 가 제공하는 부가기능
![[스크린샷 2024-03-02 오후 6.46.22.png]]
- **메시지 소스를 활용한 국제화 기능**
	- 한국에서 접근 시 한국어 출력
	- 영미권에서 접근 시 영어 출력
- **환경 변수**
	- Local, Dev, Release 등의 환경을 구분하여 처리가능.
- **Application Event**
	- 이벤트를 발행하고 구독하는 모델을 편리하게 지원.
- **편리한 리소스 조회**
	- File, Classpath, External 등의 Resource 들을 편리하게 조회.

# 정리
- Application Context 는 BeanFactory 의 기능을 상속받는다.
- **ApplicationContext 는 빈 관리기능 + 편리한 부가 기능을 제공한다.**
- BeanFactory 를 직접 사용할 일은 거의 없다. 부가기능이 포함된 ApplicationContext 를 사용한다.
- **BeanFactory / ApplicationContext 를 스프링 컨테이너라 한다.**
