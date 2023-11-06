---
Author: CarefreeLife98
Date: 2023-11-06T11:23:00
Agenda:
  - 1. Instruction Codes
  - 2. Computer Registers
tags:
  - Computer_Structure
---
# Instruction Codes
> **Instruction : 명령어.**
> - 일련의 명령어가 하나의 프로그램이 된다.
> - Microoperation 이 모여 명령어가 되고 명령어가 모여 하나의 프로그램이 됨.

## 명령어의 형식 (16-Bit Computer)
![[스크린샷 2023-11-06 오전 11.39.34.png]]
> **메모리 참조 명령어
> - 각 명령어의 Word 는 16 Bit 로 이루어짐.**
> 	- **최상위 1 Bit : I-Bit**
> 		- **유효 주소 설정 방식**을 나타냄
> 			- **I = 0 : Direct** Address
> 			- **I = 1 : Indirect** Address
> 	- **상위 3 Bit : Opcode Field**
> 		- **명령어의 종류**를 나타냄
> 	- **하위 12 Bit : Address Field**
> 		- **피 연산자의 주소**를 나타냄
> - 피 연산자는 16 Bit로 구성됨.
> <br><br>
> **메모리의 크기**
> 	- 각 명령어의 **Address Field 크기가 12 Bit** 이므로, **2^12 개의 주소를 16 Bit 만큼** 가지게 됨.
> 		- **메모리의 크기는 4096 (2^12) * 16** 의 크기를 가짐
> <br><br>
> **Processor register - Accumulator (AC)**
> - **처리된 결과가 저장되는 Register.**
> - 산술연산, 논리연산 등이 계산되어 저장되어 있는 Register.
> - **누산기** 라고 함.

<br><br>
## Effective Address (유효 주소)
![[스크린샷 2023-11-06 오전 11.52.13.png]]
> **실제 Operand (피연산자) 의 주소를 의미.**
> - **주소를 지정하는 세 가지 방식**이 존재.
> 	1. **Direct Address (직접 주소)**
> 		- 해당 **Address Field 값이 실제 Operand 의 주소**가 되는 경우.
> 		<br><br>
> 	2. **Indirect Address (간접 주소)**
> 		![[스크린샷 2023-11-06 오전 11.49.55.png]]
> 		- 해당 **Address Field 위치에 (다른 위치에 존재하는) Operand 의 주소가 존재**하는 경우.
> 		<br><br>
> 	3. **Immediate**
> 		- **Address Field 값 자체가 Operand** 가 되는 경우.

<br><br>
### 유효 주소 사용 예시
![[스크린샷 2023-11-06 오후 12.03.48.png]]
> **Direct address (직접 주소) 방식**
> - **I = 0 이므로 직접 주소 형식으로 ADD Operand 가 저장**되어 있음.
> - **실제 AC 에 있는 값을 유효 주소의 번지에 있는 Operand(ADD)를 AC 에 더해서 다시 AC에 저장**하는 과정.
> 	- **AC <- AC + M\[EA]**
> <br><br>
> **Indrect address (간접 주소) 방식**
> - **I = 1 이므로 간접 주소 형식으로 ADD Operand 가 저장**되어 있음.
> - **300 번지에 존재하는 값이 실제 유효 주소**가 됨.
> - **1350 번지에 존재하는 Operand 값을 기존 AC 값과 합한 후 AC 에 다시 저장.**

<br><br>

# Computer Registers
> 범용 Register 가 아닌 각각의 특수한 기능을 수행하는 Register 를 가지고 있는 16-Bit Computer 에 대한 것.

<br><br>
## List of Registers
> **PC (Program Counter)**
> - **명령어의 주소가 저장**되는 Register.
> - Address 가 저장되므로 12 Bit 로 이루어짐.
> <br><br>
> **AR (Address Register)**
> - 메모리가 참조하게 되는 유효 주소가 저장되는 Register.
> - 12 Bit
> <br><br>
> **Data 나 명령어를 다루는 나머지 Register 들은 전부 16 Bit 로 구성.**
> - **IR (Instruction Register)**
> 	- 명령어를 저장하는 Register.
> - **DR (Data Register)**
> 	- 피연산자가 주로 저장됨.
> - **TR (Temporary Register)**
> 	- 임시 저장을 위한 Register.
> - **AC (Accumulator)**
> 	- 기본적으로 산술, 논리 연산 처리가 저장되는 Register
> - **INPR, OUTR (Input, Output Register)**
> 	- Serial 통신을 가정하기 때문에 8 Bit Regiter 이다.
> <br><br>
> **메모리는 2^12 = 4096 개의 word 를 가지는 Memory.**

<br><br>

## Data 의 이동 (추가 노트 작성 필요)
> **Bus System 을 이용**해서 Register <-> Register , Register <-> Memory 간의 Data 이동이 수행된다.
> <br><br>
> **Selection Signal (S2, S1, S0) 에 의해 BUS 에 Load 될 Register 종류가 정해진다.**