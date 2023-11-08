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