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
> **Memory Stack 예시**
> ![[스크린샷 2023-11-21 오후 5.48.10.png]]
> - **Stack Pointer (SP)**
> 	- **접근하고자 하는 Stack Memory 의 주소가 저장되어 있는 Register**
> - **FULL, EMTY**
> 	- **현재 Stack 의 포화/공백 상태를 가진 1-bit Register**
> 	- EMTY = 1 인 경우
> 		- SP = 0 (초기화)
> - **Operation**
> 	- **PUSH** : Data 를 Memory 영역에 쓴 후 사용해야 함.
> 		1. SP <- SP + 1
> 		2. 
> 	- **POP** : Data 를 하나 꺼내 읽어 생긴 빈 공간을 표시해주어야 함.