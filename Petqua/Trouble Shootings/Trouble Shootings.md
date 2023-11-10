---
Author: CarefreeLife98
Date: 2023-11-10T01:54:00
Agenda: 
tags:
  - Petqua
---
# Error creating bean with name 'securityConfig'
![[스크린샷 2023-11-10 오전 1.53.56.png]]
> IllegalArgumentException: Could not resolve placeholder 'jwt.secret' in value "${jwt.secret}"

## 해결
> application.yml 에 있던 jwt.secret 부분을 주석 처리하고 실행시켜서 에러 발생 했던 것.
> 주석 해제 후 잘 참조하여 실행하고 있다.

