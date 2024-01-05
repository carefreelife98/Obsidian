---
Author: CarefreeLife98
Date: 2023-11-21T17:06:00
Agenda: 
tags:
  - Computer_Structure
---
# General Register Organization
> ![[스크린샷 2023-11-21 오후 5.25.24.png]]
> **범용 Register**
> - **어떤 연산의 Source 및 Destination 으로 사용 가능.**
> 
> <br>
> **범용 Register 의 Control Word**
> - **SELA, SELB**
> 	- **Source 를 선택**하기 위한 **2 개의 3-bit Register**
> - **SELD**
> 	- **Destination 을 선택**하기 위한 **1 개의 3-bit Register**
> - **OPR**
> 	- **Operation 을 결정**하기 위한 **5-bit Register**
> 
> <br>
> **범용 Register 의 편리성**
> - **Control Word**
> 	- **SELA, SELB 와 같은 Source 및 SELD 와 같은 Destination 이 존재.**
> 	- 기존처럼 피연산자를 DR 에 넣고 기존 AC 값과 DR 값을 더해 다시 AC 에 저장하는 등의 **복잡성이 낮아짐.**
> - 따라서 **각 명령어에 Source 및 Destination 이 명시되어야 하고, 이에 명령어의 길이가 길어짐.**
> 	- **대신 프로그램의 길이는 줄어듦.**

<br><br>

# Stack Organization
> **Memory 접근을 편리하게 하기 위한 방법 중 하나.**
> - Memory 의 일부를 사용하는 방법. (Memory Stack)
> - Register 로서 구현하는 방법.
> 
> <br>
> **Stack 의 특징**
> - **Last-In First-Out (LIFO)**
> 	- 하나씩 쌓아가며 Memory 에 접근
> 
> <br>
> **Memory Stack 구조 예시**
> ![[스크린샷 2023-11-23 오전 9.39.55.png]]
> - **PC**
> 	- **명령어의 주소를 저장.**
> 	- 명령어의 FETCH 단계에서 사용.
> - **AR**
> 	- **일반적으로 피연산자의 주소를 저장.**
> - **Stack Pointer (SP)**
> 	- **접근하고자 하는 Stack Memory 의 주소가 저장되어 있는 Register**
> 	- PUSH, POP 에 해당하는 메모리의 주소를 저장.
> - **FULL, EMTY**
> 	- **현재 Stack 의 포화/공백 상태를 가진 1-bit Register**
> 	- EMTY = 1 인 경우
> 		- SP = 0 (초기화)

## Memory Stack Operation: PUSH
> 	![[스크린샷 2023-11-23 오전 9.30.11.png]]
> 	`첫번째 PUSH`<br>
> 	![[스크린샷 2023-11-23 오전 9.31.12.png]]
> 	`마지막 PUSH`
> 	- **PUSH** : **Data 를 Memory 영역에 쓴 후 사용해야 함.**
> 		1. **SP <- SP + 1** // stack memory 하나 증가
> 		2. **M\[SP] <- DR** // 증가된 stack memory 에 write
> 		3. **IF (SP = 0) then (FULL <- 1)**
> 		4. **EMPTY <- 0**
> 	<br>

<br><br>

## Memory Stack Operation: POP
> ![[스크린샷 2023-11-23 오전 9.34.11.png]]
> `STACK: FULL 상태에서 첫번째 POP`<br>
> ![[스크린샷 2023-11-23 오전 9.34.56.png]]
> `마지막 POP`
> 	- **POP** : Data 를 하나 꺼내 읽어 생긴 **빈 공간을 표시**해주어야 함.
> 		1. **DR <- M\[SP]** // read
> 		2. **SP <- SP - 1** // stack pointer 감소
> 		3. **IF (SP = 0) then (EMPTY <- 1)**
> 		4. **FULL <- 0**

<br><br>

## Stack 구조 및 동작의 예: RPN (Reverse Polish Notation)
> ![[스크린샷 2023-11-23 오전 9.48.04.png]]
> **Stack 구조에 적합한 연산 형식을 RPN 형식를 통해 표현할 수 있다.**
> - 일반적인 연산 표현식: Infix notation (중위 표현식)
> 	- A + B
> - Prefix / Polish notation : 전위 표현식
> 	- + AB
> - **Postfix / Reverse Polish notation (RPN) : 후위 표현식**
> 	- **AB +**
> 	- **피연산자 뒤에 연산자가 위치.**
> 
> <br>
> **Stack 구조에서 RPN 형식을 사용**
> ![[스크린샷 2023-11-23 오전 9.57.34.png]]
> 1. **피연산자(Operand) 를 만나면 Stack 에 Push**
> 2. **연산자를 만나면 Stack 에 적재된 피연산자 두 개를 Pop 하여 연산 수행**
> 3. **연산 수행 결과를 다시 Stack 에 Push**

