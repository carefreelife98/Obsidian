---
Author: CarefreeLife98
Date: 2023-11-18T20:28:00
Agenda: 
tags:
  - Computer_Structure
---
# Micro-program Example
## Instruction Format
![[스크린샷 2023-11-18 오후 8.48.15.png]]
> **Instruction format (Memory, 즉 Main Program 에 적재됨)**
> ![[스크린샷 2023-11-18 오후 8.47.11.png]]
> - I Bit : 1-bit
> - **Opcode : 4-bit**
> - **Address : 11-bit**
> 
> <br>
> **4 개의 Computer Instruction 정의 (EA = Effective Address, I = 0)**
> 
> - **ADD**
> 	- Opcode 가 **0000** 인 경우
> 	- **AC <- AC + M\[EA]**
> 	- **Mapping 시 명령어의 시작 주소 (7-bit 로 변환)**: 0 **0000** 00
> - **BRANCH**
> 	- Opcode 가 **0001** 인 경우
> 	- **IF (AC < 0) then (PC <- EA)**
> 	- **Mapping 시 명령어의 시작 주소 (7-bit 로 변환)**: 0 **0001** 00
> - **STORE**
> 	- Opcode 가 **0010** 인 경우
> 	- **M\[EA] <- AC**
> 	- **Mapping 시 명령어의 시작 주소 (7-bit 로 변환)**: 0 **0010** 00
> - **EXCHANGE**
> 	- Opcode 가 **0011** 인 경우
> 	- **AC <- M\[EA], M\[EA] <- AC**
> 	- **Mapping 시 명령어의 시작 주소 (7-bit 로 변환)**: 0 **0011** 00

<br><br>

## Micro-Instruction Format
![[스크린샷 2023-11-18 오후 9.01.23.png]]
> **Micro-instruction format (Control Memory 에 저장됨 - Micro program 을 구성하는 명령어)**
> ![[스크린샷 2023-11-18 오후 8.50.43.png]]
> - **총 20-bit 로 구성**
> - **Micro-instruction 의 Field**
> 	- **Function Field (Microoperation field)**
> 		- F1, F2, F3
> 		- 최상위 9-bit
> 		- F_i = 000 인 경우 아무 동작도 수행하지 않는다.
> 	- **Condition for Branching (BRANCH 를 위한 조건 필드)**
> 		- CD
> 		- 2-bit
> 	- **Branch Field**
> 		- BR
> 		- 2-bit
> 	- **Address Field**
> 		- AD
> 		- 7-bit
> - **Micro-instruction 의 Function Field 가 세 개**
> 	- 하나의 Micro-instruction 은 **동시에 최대 세 개의 Microoperation 수행 가능.**
> - Microoperation (F1, F2, F3) 이 끝난 후 **CD, BR, AD 에 의해 어느 번지에 있는 것을 수행 할 지 정해진다.**

<br><br>
## Symbols and Binary Code for Microoperation Fields
### F

### CD
> **00 인 경우**
> - 조건 = always 1 (참)
> - **Unconditional Branch 수행**
> 	- BR 과 AD bit 를 참조하여 점프
> 
> <br>
> **01 인 경우**
> - 조건 = DR(15)
> 	- Data Register 의 최상위 bit 를 의미.
> 		- Instruction Register 가 따로 존재하지 않으므로 명령어를 DR 에 저장.
> 		- 따라서 명령어의 최상위 비트 I 를 참조하여 Direct / Indirect Address 를 구분.
> 	- 조건이 참 (DR(15) = 1) 이면 I = 1 이므로 해당 명령어는 Indirect Address.
> 	- 거짓이면 Direct Address.
> - **명령어의 최상위 비트 I 참조를 수행**

### BR