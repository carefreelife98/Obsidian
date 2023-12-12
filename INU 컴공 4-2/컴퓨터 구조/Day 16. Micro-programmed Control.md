---
Author: CarefreeLife98
Date: 2023-11-16T09:46:00
Agenda:
  - 1. Control Memory
  - 2. Address Sequencing
  - 3. Micro-program Example
  - 4. Design of Control Unit
tags:
  - Computer_Structure
---
# Control Memory
![[스크린샷 2023-11-16 오전 10.09.50.png]]
`Micro-program 의 전반적인 모습 및 구성요소`

<br><br>

## Hard-wired Control vs Micro-programmed Control
> **Hard-wired Control : 회로로서 Control 이 설계되어 있음.**
> ![[스크린샷 2023-11-16 오전 9.54.17.png]]
> - ADD 라는 Control 을 위해서 각 Timing signal (T_0 ~ T_5) 에 적절한 Micro-operation 을 수행하도록 회로를 설계.
> - **모든 Control 이 Hardware 적으로 설계되어 있음.**
> 	
> 	<br><br>
> **Micro-programmed Control : Micro-program 에 의해 Control 이 설계됨.**
> ![[스크린샷 2023-11-16 오전 10.08.44.png]]
> - **Control 을 위한 Memory (Control Memory) 가 따로 존재.**
> 	- **Control Memory : Micro-program 들이 적재되는 Memory.**
> 		- **Micro-program 을 구성하는 각각의 명령어를 Micro-instruction** 이라고 한다.
> 	- Control Memory 내부에 존재하는 **Micro-program 에 의해 각각의 명령어가 수행**되도록 함.
> - **Control Memory 는 Control word 를 출력.**
> 	- **Control word** : 해당 Clock 에서 어떠한 Micro-operation 을 수행할지 선택하는 Control Signal 의 조합
> 		- **Control variable** : Micro-operation 을 구체화하여 분류.

<br><br>
## General Configuration of a Micro-programmed Control Unit
![[스크린샷 2023-11-16 오전 10.23.03.png]]
> 1. **Control Memory**
> 	- Control 을 위한 Micro-program 들이 적재되어 있는 Memory
> 2. **Control Address Register (CAR)**
> 	- **Control Memory 내부에 적재되어 있는 각 Micro-instruction 이 존재하는 주소를 가지고 있는 Register.**
> 	- 이전에 배운 **AR 과 비슷한 기능**을 함.
> 	- **동작 과정**
> 		1. **Address Sequencer 에 의해 다음 실행할 Micro-instruction 의 주소가 결정.**
> 		2. **해당 주소가 Control Address Register 에 저장됨.**
> 		3. **Control Memory 가 Control Address Register 에 저장되어 있는 주소를 참조하여 해당 주소에 저장되어 있는 Micro-instruction 을 Control Data Register 에 저장.**
> 		4. **Control Data Register 에 저장된 것이 곧 Control Word 로서 저장.**
> 			- 즉, Control word == Micro-operation

<br><br>
# Address Sequencing

> **Micro-instruction 이 모여 Micro-program 이 되고, Micro-program 이 모여 Control Memory 를 구성한다.**
> - **Address Sequencing** :
> 	- 이때 어떠한 **Micro-instruction 을 수행하기 위해 해당 주소를 어떻게 지정할 것인지**에 대한 전략.

<br><br>
## Micro-program Routine

> **각각의 Micro-instruction 은 Micro-program Routine 을 가지고 있다.**
> ![[스크린샷 2023-11-16 오전 10.36.30.png]]
> - ADD 의 경우에는 **NOP -> READ -> ADD 라는 Routine** 을 가짐.

<br><br>

## 각 Micro-instruction 수행 과정
![[스크린샷 2023-11-16 오전 10.40.57.png]]
> 1. **FETCH**
> 2. **해당 명령어에 필요한 유효 주소 결정**
> 3. **명령어 실행 후 FETCH 로 돌아감**

<br><br>

## Address Sequencing : 4가지 방법
### 1. Increment of CAR
![[스크린샷 2023-11-16 오전 10.51.03.png]]
> Program Counter 를 하나 증가시키는 것과 비슷한 원리.
> - **Control Memory 에 있는 Micro-program 을 순차적으로 수행.**

<br><br>

### 2. Conditional Branching
![[스크린샷 2023-11-16 오전 10.54.00.png]]
> **특정 명령어 수행 중 조건 충족 시 지정된 특정 명령어로 점프하는 것.**
> - Control Memory 에서 Branch address Line 을 통해 해당 주소를 CAR 에 저장.

<br><br>

### 3. Mapping from Instruction
![[스크린샷 2023-11-16 오전 10.58.03.png]]
> **해당 명령어의 실행 첫 부분에 Control Memory 의 번지 수를 지정**
> - **Micro-instruction 의 Code 일부분을 토대로 해당 Instruction 의 Address 로 변환**하는 방법.

<br><br>
> ![[스크린샷 2023-11-16 오전 11.12.17.png]]
> **Control Memory, CAR 의 범위는 7 Bit 로 설정.**
> - **Micro-instruction 의 Opcode (4 bit) 를 7 bit 로 변환**해야 한다.
> 	- **기존 Opcode 에 최상위 비트 0 1개, 하위 비트 0 2개를 추가.**
> 		![[스크린샷 2023-11-16 오전 11.10.40.png]]
> 		- 이 경우 위 사진과 같이 하위 비트 경우의 수 만큼 해당 명령어의 주소 범위가 생성됨.
> 	- 이렇게 생성된 **Address 를 해당 명령어가 포함된 Routine 의 첫번째 주소로 지정.**

<br><br>

### 4. Subroutine Call and Return
![[스크린샷 2023-11-16 오전 11.01.06.png]]
> **Conditional Branching 등에 의해 특정 명령어로 점프했을 경우 돌아올 주소를 Subroutine Register 에 저장.**
> - **Subroutine Register 에 저장되어 있는 돌아올 주소를 꺼내 CAR 에 넣는 것.**





<br><br>

