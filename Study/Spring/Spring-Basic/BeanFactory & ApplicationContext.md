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
