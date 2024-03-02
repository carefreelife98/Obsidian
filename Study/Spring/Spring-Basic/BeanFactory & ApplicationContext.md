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
- 빈을 관리하고 검색하는 기능은 BeanFactory 가 제공하는 기능이다