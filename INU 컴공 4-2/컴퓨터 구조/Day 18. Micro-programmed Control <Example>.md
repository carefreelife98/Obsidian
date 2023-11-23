---
Author: CarefreeLife98
Date: 2023-11-23T10:02:00
Agenda: 
tags:
  - Computer_Structure
---
# Micro-programmed Control 예시
![[스크린샷 2023-11-23 오전 10.03.33.png]]
`좌측 하단 회로: 전체 하드웨어 구조 / 우측 하단 회로: Control Unit 의 상세 구조`<br>
`우측 상단 값: Memory 의 주소 - 저장된 값`

## 1. Micro-program 분석
![[스크린샷 2023-11-23 오전 10.03.33.png]]
> **Memory 의 크기: 2048 x 16**
> - **명령어 : 1000 0000 1000 0000 (16-bit)**
> 	- I bit = 1 (1-bit)
> 	- Opcode = 0000 (4-bit)
> 	- **Address = 000 1000 0000(11-bit)**
> - **Memory 의 크기는 2^(address bit 크기) * 명령어의 bit 크기**
> 	- **2^11 * 16 = 2048 x 16 bit**
> 
> <br>
> **AR, PC 의 크기: 명령어의 주소를 저장하므로 11 bit**
> <br>
> **DR, AC 의 크기: Data / 명령어 값을 가지는 Register - 16bit**
> <br>
> **Control Memory 의 크기**
> - **Micro-instruction 의 크기를 20 bit 로 정의**
> 	- Microoperation(F1, F2, F3)
> 		- 3 x 3 = 9-bit
> 	- CD = 2-bit
> 	- BR = 2-bit
> 	- **AD = 7-bit**
> - **따라서, 2^(AD) * 명령어의 bit 크기**
> 	- **Control Memory 의 크기 : 2^7 * 20**

<br><br>

## 2. Micro-program 수행 과정
> 1. **PC 값 확인을 통한 현재 실행해야 하는 명령어 파악**
> 	- PC = 000 0001 0000
> 	- **실행해야 하는 명령어 : M\[PC] = 1000 0000 1000 0000**
> 	<br><br>
> 2. **명령어 구조 파악: 1000 0000 1000 0000**
> 	![[스크린샷 2023-11-23 오전 10.37.02.png]]
> 	- I-bit = 1 (= Indirect)
> 	- Opcode = 0000 (= ADD)
> 	- AD = 000 1000 0000
> 		- **Indirect 주소(000 1000 0000)를 가진 ADD 명령어임.**
> 	- Indirect 주소인 000 1000 0000 에는 0000 0001 0000 0000 이 존재.
> 		- **해당 값에 의해 실제 유효 주소는 001 0000 0000 임을 알 수 있음.**
> 	- **유효 주소인 001 0000 0000 에는 0000 0000 0000 0101 이 저장되어 있으며, 이는 ADD 의 피연산자 값이 된다.**
> 	<br><br>
> 3. **명령어 수행**
> 	- **ADD: AC <- AC + M\[EA]**
> 		- AC = 3
> 		- M\[EA] (유효 주소에 저장된 값) = 0000 0000 0000 0101 = 5
> 	- **따라서, AC 에는 3+5 연산 수행의 결과인 8이 저장됨.**
> 	<br>
> 4. **명령어 수행 후: Always FETCH**
> 	- **하나의 명령어가 수행 후 종료되면 CAR 은 FETCH 위치에 존재.**
> 		- 예: ADD 명령어의 마지막 Microoperation
> 			- CD(00) BR(00) AD(1000000) = Unconditionally Jump to FETCH
> 		- FETCH: 1000000 (64)