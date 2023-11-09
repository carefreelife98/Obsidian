---
Author: CarefreeLife98
Date: 2023-11-08T13:02:00
Agenda: 
tags:
  - Computer_Structure
---
# Timing and Control

## Control Unit of Basic Computer
![[스크린샷 2023-11-08 오후 1.21.31.png]]
> **최상위 Bit 는 I 라는 Register 에 연결**되어 있음.
> <br><br>
> **Opcode 는 3 x 8 Decoder 의 Input으로 입력되어 Decoder Output 을 반환.**
> - **Opcode 는 000 ~ 110  까지는 Memory 참조 명령어를 나타냄.**
> 	- **000 ~ 110** 이 Decoder 에 Input 으로 입력될 시, **D0 ~ D6 까지 1**이 반환됨.
> 		- 이 경우 **Memory 참조 명령어 임을 알 수 있음.**
> - **Opcode 가 111 인 경우 Register 참조 명령어 혹은 I/O 명령어를 나타냄.**
> 	- **111** 이 Decoder 에 Input 으로 입력 될 시, **D7 만 1**이 반환됨.
> 		- **I 의 값에 따라 Register 참조 명령어인지, I/O 명령어 인지 구분 가능**해짐.
> 		- **나머지 Bit (12 Bit) 에 의해 명령어의 종류가 결정**됨.
> <br><br>
> 	**따라서 Control Logic Gates 에서는 I 값과 3 x 8 Decoder Output, 그리고 나머지 12 Bit 를 입력으로 받아 어떤 명령어인지 판독 할 수 있게 된다.**

<br><br>![[스크린샷 2023-11-08 오후 1.28.55.png]]
> **하단의 회로는 순차적으로 증가하는 순차 회로 (SC) 이다.**
> - **CLR 의 Input 이 1인 경우**
> 	- 0000을 4 x 16 Decoder 에 전송.
> - **CLR 이 Input 이 0 이고, Increment 가 1인 경우**
> 	- Clock 이 1 이 될 때마다 D Input 이 1씩 증가. (0000 ~ 1111)
> <br><br>
> **결국 Timing Signal 을 생성하는 회로임.**
	![[스크린샷 2023-11-08 오후 1.41.57.png]]
> - 해당 **Timing Signal 에 따라서 Control Logi Gates 에 존재하는 명령어들이 순차적으로 수행됨**.

<br><br>

## Instruction Cycle
> **하나의 명령어는 하나의 Cycle 로 이루어짐.**
> - **하나의 Cycle 을 지나면서 해당 명령어가 수행**됨.
> <br><br>
> **Instruction Cycle 의 구성**
> **우리가 배운 모든 (대부분의) 명령어는 아래와 같은 구성을 가짐.**
> 1. **Fetch**
> 	- 명령어를 가져오는 동작.
> 2. **Decode**
> 	- 어떤 명령어인지 해석하는 과정
> 3. **Read Effective Address**
> 	- 만약 Indirect Address 인 경우 수행.
> 	- 유효 주소를 다시 찾아 읽어오는 과정.
> 4. **Execute**
> 	- 명령어 수행과정

<br><br>
## Instruction Cycle - Fetch & Decode
![[스크린샷 2023-11-08 오후 2.18.26.png]]
### 1. Fetch 의 동작 과정 - 명령어를 가져온 후 PC 증가
![[스크린샷 2023-11-08 오후 2.19.02.png]]
> 1. **[FETCH] T_0 : AR <- PC**
> 	![[스크린샷 2023-11-08 오후 2.26.33.png]]
> 	- **T_0 에서 PC 의 값을 AR 로 가져옴.**
> 		- PC 는 명령어의 주소가 존재하는 Register.
> 	- 명령어를 읽어오기 위해 해당 명령어의 주소를 AR 에 넣어야 함.
> 	<br><br>
> 2. **[FETCH] T_1 : IR <- M\[AR], PC <- PC + 1**
> 	![[스크린샷 2023-11-08 오후 2.28.48.png]]
> 	- 이전에 저장한 **명령어의 주소를 가지고 있는 AR 값을 이용해 Memory에서 해당 위치에 존재하는 값을 Instruction Register 에 저장.**
> 	- 그와 동시에 Program Counter 의 값을 하나 증가시킴.
> 		- **프로그램에서 수행할 각 명령어들이 순차적으로 누적되어 있기 때문에 다음 수행할 명령어를 PC 값을 하나 증가시켜 알려주어야 함.**
> <br>
> \[Fetch 끝]


<br><br>
### 2. Decode 의 동작 과정
![[스크린샷 2023-11-08 오후 2.19.37.png]]
> **[DECODE] T_2 : D_0, ... D_7 <- Decode IR(12~14), AR <- IR(0~11), I <- IR(15)**
> - **Opcode (3 Bit) 가 3 x 8 Decoder 에 삽입**됨.
> - **동시에, Opcode (4 bit) 를 제외한 나머지 Address Bit (12 bit) 를 AR 에 삽입.**
> 	- Instruction Register 의 하위 12 Bit 는 피연산자의 주소값이 되므로 해당 주소를 나타내는 하위 12 bit 를 AR 에 저장.
> - **그와 동시에, I Register 에 최상위 1 Bit 를 저장.**
> <br>
> \[Decode 끝]

### Fetch & Decode 정리
![[스크린샷 2023-11-08 오후 2.22.10.png]]

<br><br>

## Instruction Cycle - Determine the type of instruction
![[스크린샷 2023-11-08 오후 2.37.28.png]]
> 1. **순차 카운터 (SC) 를 0으로 함으로서 시작.**
> 2. **Fetch & Decode (T_0 ~ T_2) 수행**
> 3. **Execute (수행 단계)**
> 	![[스크린샷 2023-11-08 오후 2.41.22.png]]
> 	- **D_7 값과 I 값을 사용해서 어떤 명령어인지 판독하는 단계.**
> 		- D_7 값과 I 값은 T_2 단계에서 이미 정해졌음
> 		- **D_7 값은 Opcode 가 000~110 범위이면 0, 111 이면 1로서 존재.**
> 	- **D_7 = 0, D_0 ~ D_6 != 111**
> 		- **Memory 참조 명령어** 임을 나타냄.
> 	- **D_7 = 1, I = 0**
> 		- **Register 참조 명령어** 임을  나타냄.
> 	- **D_7 = 1, I = 1**
> 		- **I/O 명령어** 임을 나타냄.

<br><br>
## Instruction Cycle - 전체 Flow Chart
![[스크린샷 2023-11-08 오후 3.03.39.png]]
> **Memory 참조 명령어인 경우 (D_7 = 0)**
> - **D_7' I T3 : AR <- M[AR] (D_7 = 0, I = 1)**
> 	- **D_7 = 0, I = 1 이므로 간접 주소를 사용한 Memory 참조 명령어 임을 알 수 있음.**
> 	- 또한 T_3 = 1인 동안에 유효 주소를 읽어오게 됨.
> 		- **T_2 단계에서 한번 읽어온 주소를 다시 참조하여 AR이 유효 주소를 저장하도록 갱신.**
><br><br>
>- **D_7' I' T_3 : Nothing (D_7 = 0, I = 0)**
>	- **D_7 = 0, I = 0 이므로 직접 주소를 사용한 Memory 참조 명령어 임을 알 수 있음.**
>	- **따라서 T_3 = 1인 동안 아무 동작도 수행하지 않음.**
><br><br>
> **Register 참조 명령어인 경우 (D_7 = 1, I = 0)**
> - Register 참조 명령어를 수행.
> <br><br>
> **I/O 명령어인 경우 (D_7 = 1, I = 1)**
> - I/O 명령어 수행.

<br><br>

## Instruction Cycle - Register 참조 명령어
![[스크린샷 2023-11-08 오후 3.26.15.png]]
> **Register 참조 명령어인 경우 = 0111 ~**
> - **최상위 Bit I = 0**
> - **Opcode = 111**
> 	- 나머지 12 bit 는 Register 참조 명령어의 종류를 나타냄.
> <br><br>
> **표기 예시 : r B_11 (= CLA)**
> - **r = D_7 I' T_3**
> 	- 위에서 본 것과 같다. D_7 이 1이고 I 가 0이면 Register 참조 명령어임.
> 	- **따라서 Register 참조 명령어이고 T_3 Clock 이 1 인 경우 AC <- 0 과 같은 Clear 동작을 수행하라는 것.**

<br><br>

## Instruction Cycle - Memory 참조 명령어
![[스크린샷 2023-11-09 오후 2.36.33.png]]
> **Opcode 가 000 ~ 110 범위 인 경우.**<br>
> 앞서 진행했던 단계 (T_0, T_1 : FETCH, T_2 : DECODE, T_3 : EXECUTE) 이후에 Memory 참조 명령어가 실행됨.
> - **따라서 모든 Memory 참조 명령어는 T_4 단계부터 진행.**
> <br><br>
> 1. **AND : AND to AC**
> 	![[스크린샷 2023-11-09 오후 2.07.27.png]]
> 	- **AND memory word to AC**
> 		- **memory word 란 유효 주소가 가리키고 있는 메모리의 값.**
> 			- 즉, Address Register 에 들어있는 번지 수가 가리키고 있는 메모리 공간의 값.
> 		- **memory word 와 AND 연산을 한 결과를 AC 에 저장하는 연산.**
> 	- **두 단계에 걸쳐 이루어짐 (T_4 -> T_5)**
> 		1. **D_0 T_4 : DR <- M\[AR]**
> 			- D_0 T_4 (D_0 는 Opcode 가 000 임을 나타냄.)
> 				- **Opcode 가 000이고 T_4 이면 실행**
> 			- **Address Register 에 저장되어 있는 유효 주소를 메모리에서 참조해 해당 주소에 존재하는 값을 DR 에 저장.**
> 			- 메모리에 존재하는 값을 직접 연산할 수 없기에 우선 해당 값을 DR 에 가져와야함.
> 		2. **D_0 D_5 : AC <- AC ^ DR, SC <- 0**
> 			- **AC 와 이전에 DR 로 가져온 값을 AND 연산 후 다시 AC 에 저장.**
> 			- **그와 동시에 SC (순차 카운터) 를 0으로 초기화**
> 				- SC 를 0으로 초기화 할 시 T_0 로 돌아가게 됨.<br><br>
> 2. **ADD : add to AC**
> 	![[스크린샷 2023-11-09 오후 2.21.51.png]]
> 	- **가산 연산**
> 	- **두 단계에 걸쳐 이루어짐 (T_4 -> T_5)**
> 		1. **D_1 T_4 : DR <- M\[AR]**
> 			- D_1 T_4 (D_1 는 Opcode 가 001 임을 나타냄.)
> 				- **Opcode 가 001이고 T_4 이면 실행**
> 			- **Address Register 에 저장되어 있는 유효 주소를 메모리에서 참조해 해당 주소에 존재하는 값을 DR 에 저장.**
> 			- 메모리에 존재하는 값을 직접 연산할 수 없기에 우선 해당 값을 DR 에 가져와야함.
> 		2. **D_1 T_5 : AC <- AC + DR, E <- C_out, SC <- 0**
> 			- **가산 연산 (add) 실행과 동시에 E (Extended Accumulator) 에 end carry 저장.**
> 			- 그와 동시에 **SC 를 0으로 초기화 하여 T_0 로 상태 설정.**
> 			<br><br>
> 3. **LDA : load to AC**
> 	![[스크린샷 2023-11-09 오후 2.21.26.png]]
> 	- Read 동작이라고 보면 됨.
> 	- **DR 에 존재하는 값을 AC 에 값을 가져오는 기능.**
> 	- **두 단계에 걸쳐 이루어짐 (T_4 -> T_5)**
> 		1. **D_2 T_4 : DR <- M\[AR]**
> 			- D_2 T_4 (D_2 는 Opcode 가 010 임을 나타냄.)
> 				- **Opcode 가 010이고 T_4 이면 실행**
> 			- **Address Register 에 저장되어 있는 유효 주소를 메모리에서 참조해 해당 주소에 존재하는 값을 DR 에 저장.**
> 			- 메모리에 존재하는 값을 직접 연산할 수 없기에 우선 해당 값을 DR 에 가져와야함. 
> 		2. **D_2 T_5 : AC <- DR , SC <- 0**
> 			- **DR 의 값을 그대로 AC 에 전송.**
> 			- 항상 마무리는 SC <- 0 을 통해 SC 를 초기화 하여 다음 명령어가 실행될 수 있도록 T_0 로 상태를 설정 해주어야 한다.
> 		<br><br>
> 4. **STA : store AC**
> 	![[스크린샷 2023-11-09 오후 2.36.18.png]]
> 	- **AC 의 값을 유효주소에 저장.**
> 	- 한 단계로 진행 (T_4)
> 		- **D_3 T_4 : M\[AR] <- AC, SC <- 0**
> 			- D_2 T_4 (D_3 는 Opcode 가 011 임을 나타냄.)
> 				- **Opcode 가 011이고 T_4 이면 실행**
> 			- **이전 단계 (T_0 ~ T_3) 에서 읽어온 유효 주소에 AC 값을 Write**
> 			- 마찬가지로 SC <- 0 으로 초기화.

<br><br>
## Instruction Cycle - Memory 참조 명령어 (Branch)
> **Direct Address 인 경우 : 01xx (4, 5, 6)**<br>
> **Indirect Address 인 경우 : 11xx (C, D, E)**
> <br>
> **Branch 란?**
> - **기존 방식 (순차적인 명령어 실행) 과 다르게 특정 주소의 명령어로 점프하여 바로 실행하는 것.**
> <br><br>
> 1. **BUN : Branch unconditionally (무조건 점프)**
> 	![[스크린샷 2023-11-09 오후 2.47.09.png]]
> 	![[스크린샷 2023-11-09 오후 2.49.08.png]]
> 	- **D_4 T_4 : PC <- AR , SC <- 0**
> 		- **Address Register 의 값을 Program Counter 에 저장 후 종료.**
> 	- **Program counter 에 특정 명령어의 유효 주소가 저장되어 다음 실행 시 해당 명령어로 점프하여 바로 실행됨.**
