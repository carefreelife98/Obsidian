---
Author: CarefreeLife98
Date: 2023-11-20T13:25:00
Agenda: 
tags:
  - CPP
---
> **I/O**
> - **User & Kernel 사이의 syscall** 을 통해 이루어짐.
> - **syscall 은 kernel 에 무리**가 많이 가므로 **stream** 이라는 것을 씌워 **kernel 을 도와주는 동시에 여러 기능을 구현.**
> - stream 과 user 는 **api 를 통해 통신.**
> 
> <br>
> **Stream**
> - **Buffer 가 존재.**
> 	- Buffer 에 데이터를 적재해두었다가 한번에 syscall 을 호출하여 한번에 처리.
> - **운영 체제 측면에서 자원 관리를 효율적으로 하도록 도와주는 하나의 객체.**


> **직렬화 (Serialize) 개념 중요**

> **binio.cpp 예제 꼭 확인!!**