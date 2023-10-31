---
Author: CarefreeLife98
Date: 2023-10-10T00:09:00
Agenda: 1. Register
tags:
  - Computer_Structure
---
# Registers (Sequential Circuit: Flip-Flop)
## Agenda
> **Binary 값을 저장하는 소자.**
> - **빠른 속도를 목적으로 존재.**
> - Memory 와는 다르게 CPU 내에서 **임시 저장소로 사용.**
> <br>
> **Flip-Flop 으로 구성됨**
> - **n Bit Register == n 개의 Flip-Flop 으로 구성된 Register.**

<br><br>

![[스크린샷 2023-10-10 오전 12.30.22.png]]
> 위의 Register 구성 요소 및 상태
> - **D Flip-Flop 4개로 구성.**
> - **상태**
> 	- A0 ~ A3
> - **D Input**
> 	- I0 ~ I3
> - **Clock Pulse 4개 구성.**
> 	- Positive Edge Mode
> 		- Clock Pulse 가 상승할 때에 상태가 바뀜.
> - **Clear Input 이 각각의 Flip-Flop 에 연결되어 있음. (Direct Input)**
> 	- **Direct Input** : Clock Pulse 나 Input 에 상관없이 Flip-Flop Output, 즉 상태를 바꾸는 것. (강제)
> 	- 현재 예시에서는 Clear 가 1 인경우 A0 ~ A3 까지 Clock Pulse, Input 상관없이 0으로 초기화시킴. (Reset)
- **Register 가 값을 기억하고 있는 원리**
	- **Clock Pulse 가 다시 상승하기 전까지는 이전의 상태를 유지함.**
	- **자신의 값 (Value) 을 Holding** 하고 있게 된다.
- 위 예시는 Clock Pulse 에 의하여 계속 값이 변함.
	- 다음 예시에서 그렇지 않은 경우를 보자.

<br><br>
## Register with Parallel Load
![[스크린샷 2023-10-10 오전 12.42.42.png]]

> **D Input 분석하기**
> - **AND Gate**
> 	- **Input**
> 		- A (0 ~ 3)
> 		- Load' (NOT)
> 	- **Output**
> 		- A * Load'
> - **OR Gate**
> 	- Input
> 		- AND 의 Output (A * Load'), (I * Load)
> 	- Output
> 		- (A * Load') + (I * Load)
> **따라서, D Input =**
> 	- **D_i = (A_i * Load') + (I_i * Load)**

<br><br>
### Register with Parallel Load - Load 가 0인 경우
![[스크린샷 2023-10-10 오전 1.03.08.png]]
> - 위의 경우에서, **만약 Load 가 0인 경우, D_i = A_i** 가 된다. 
> 	- 이때, **D_i 는 A_i 에 연결되어 있기 때문에 Clock Pulse 의 상태와 무관하게 D_i = A_i 를 유지**
> 	- 이는 Clock Pulse의 상태와 상관없이 **이전 상태를 유지한다는 의미.**
> 		- 즉, **Load = 0 인 경우 No Change.**

<br><br>

### Register with Parallel Load - Load 가 1인 경우
![[스크린샷 2023-10-10 오전 1.14.01.png]]
> - **만약 Load 가 1인 경우, D_i = I_i** 가 된다. 
> 	- 이 경우에는, 기존 Flip-Flop 동작 과정과 같이 **Clock Pulse 가 상승할 때에 I_0 ~ I_3 가 A_0 ~ A_3 로 전달된다.** 
> 		- (D Flip-Flop 이므로 이전 상태를 그대로 다음 상태에 전달함.)
> - **Parallel Load**
> 	- Load 라는 Control Signal 에 의해 병렬적으로 전달.
> 	- **I_0 ~ I_3 가 동시에 (한번에) A_0 ~ A_3 로 전달된다는 의미.**

<br><br>

### Register with Parallel Load 정리
> 가장 먼저 알아봤던 Basic Register 에서는 Clock Pulse 에 따라 값의 유지 및 변경이 끊임없이 발생함.
> <br>
> 하지만 **Load 를 사용한 Register 에서는 조금 더 구체화 됨.**
> 	- **Load 가 0 인 경우 값을 기억**
> 	- **Load 를 1로 변환하면 D Flip-Flop 이 동작하여 Clock Pulse 에 따라 값을 최신화** 해 줄 수 있게 된다.

<br><br>

## 4-Bit Shift Register
![[스크린샷 2023-10-10 오전 1.35.47.png]]
> 이전에 본 병렬 Register 가 아닌 **직렬 Register.**
> - 1, 1, 0, 1 이 Serial Input 으로 전달 **(직렬이므로 총 4번에 나누어)**
> - **Clock Pulse 가 상승 할 때마다 다음 Flip Flop 으로 값을 전달**해나감.

<br><br>

## Bidirectional Shift Register with Parallel Load
![[스크린샷 2023-10-10 오전 1.49.08.png]]
> 앞서 본 모든 Register 를 모아놓은 형태의 Register이다.
> <br>
> **구성 요소 및 형태 분석**
> - **D Flip-Flop 4개**
> 	- Input
> 		- 4 X 1 MUX
> - **4 X 1 MUX 4개**
> 	- Input 4개
> 	- **Selection Signal**
> 		- **D Flip-Flop 으로 전달될 Input 결정**

<br><br>

### Selection Signal 에 의한 동작 과정 (Operation)
1. **Selection Signal 이 0, 0일때**
	- Index 0 이 선택된다.
> ![[스크린샷 2023-10-10 오전 1.54.12.png]]
> - 앞에서 본 Parallel Load 가 0인 것과 같은 상황. (No Change)
> - **Input 과 Clock Pulse 에 관계없이 현재 D Flip-Flop의 상태를 그대로 A_0 ~ A_3 으로 전달. No Change.**

2. **Selection Signal 이 0, 1 일때**
	- Index 1 이 선택된다.
> ![[스크린샷 2023-10-10 오전 2.04.04.png]]
> - 앞에서 본 Shift Register (직렬 Register)
> - **Clock Pulse 가 상승 할 때 마다 아래 방향으로 한 Bit 씩 이동하는 Shift Right (Down) Register.**

3. **Selection Signal 이 1, 0 일때**
	- Index 2 가 선택된다.
>![[스크린샷 2023-10-10 오전 2.08.43.png]]
> - Step 2 에서 본 Shift Register (직렬 Register) 의 반대 방향으로 가는 Register.
> - **Clock Pulse 가 상승 할 때 마다 윗 방향으로 한 Bit 씩 이동하는 Shift Left (Up) Register.**

4. **Selection Signal 이 1, 1 일때**
	- Index 3 이 선택된다.
>
>- 앞에서 본 Parallel Load 가 1 인 것과 같은 상황. (Parallel Load)
> - **Clock Pulse 가 상승 할 때 Input 을 A_0 ~ A_3 으로 전달.**

<br><br>

## Binary Counter
> **Counter 역시 Register 이다.**
> <br>
> **Register vs Counter**
> **비슷한 점**
> - **Binary 값을 저장**한다는 의미에서는 같은 Register.
> <br>
> **차이점**
> - **정해진 Sequence, 상태를 반복하게 되는 Register**

<br><br>

### 4-Bit Synchronous Binary Counter (Analysis)
![[스크린샷 2023-10-10 오전 2.20.04.png]]
> **4 Bit**
> - 4 Bit 로 이루어짐 -> 4개의 Flip-Flop 으로 이루어짐.
> <br>
> **Synchronous**
> - Clock Pulse 에 의해서 동기화 되어 **모든 Flip-Flop 값이 Clock Pulse의 상태가 변할 때 (상승 / 하강) 함께 변한다.**
> <br>
> **구성 요소 및 상태 분석**
> - **Input x**
> 	- Count enable
> - **Output y**
> 	- Output carry
> - **4개의 JK Flip-Flop**
> 	- Input
> 		- J_0 ~ J_3
> 		- K_0 ~ K_3

<br><br>

1. **JK Flip-Flop Input 정의**
> ![[스크린샷 2023-10-10 오전 2.32.43.png]]
> - J_0 = K_0 = x
> - J_1 = K_1 = A_0 * x
> - J_2 = K_2 = A_1 * A_0 * x
> - J_3 = K_3 = A_2 * A_1 * A_0 * x
> - y = A_3 * A_2 * A_1 * A_0 * x
> <br>

2. **State Table 작성** 
> ![[스크린샷 2023-10-10 오전 2.43.29.png]]
> - **x 가 0이면 전부 0이 되므로 x = 1인 경우만 고려.**
> - **J 와 K 가 같으므로 JK Excitation Table 은 J 와 K 가 둘 다 0, 1 인 경우만 고려.**
> 	- 사실상 T Flip-Flop 과 같은 기능을 수행

3. State Diagram 작성
> ![[스크린샷 2023-10-10 오전 2.50.30.png]]
> - 위 Diagram을 보아, 위 Counter 는 **0 부터 1씩 15 까지 증가하는 4 Bit Increasing Counter.**
> 	- 16 이 될 시 Output Carry 를 1 반환하고 다시 0 으로 초기화됨.
> - x 가 1인 경우 하나씩 증가.
> 	- x 가 0인 경우 이전 상태 그대로 유지 (저장)

<br><br>

### Binary Counter with Parallel Load
![[스크린샷 2023-10-10 오후 2.42.47.png]]
> 조금 더 복잡한 Binary Counter 이다.
> <br>
> 각각의 JK Flip-Flop Input 에 대한 Boolean Function 먼저 찾아보자.
> - **I : Increment**
> - **I_0 ~ I_3 : Input**
> <br><br>
> ![[스크린샷 2023-10-10 오후 2.49.24.png]]
> - 결과적으로, 위 Counter 는 **Clear, Load, Increment 라는 Control Signal 에 의하여 서로 다른 Operation**을 수행.
> <br><br>
> 	- **Clear**
> 		![[스크린샷 2023-10-10 오후 2.57.25.png]]
> 		- **상태를 Clear**
> 		- **C 가 1인 경우, Load 및 Increment 와 상관없이 J = 0, K = 1** 이 된다.
> 			- **JK Flip-Flop 에서 0 이 들어가면 0으로 Reset** 하는 것.
> 			- 따라서 **C 가 1인 경우, L, I 와 상관없이 모든 Flip Flop 을 0으로 Clear.**
> 	<br><br>
> 	- **Load**
> 		![[스크린샷 2023-10-10 오후 3.08.22.png]]
> 		- **회로의 Input 을 전달**
> 		- **C 가 0이고, L이 1인 경우, I 와 상관없이 J_n = I_n, K_n = I_n' 이 된다.**
> 			- 이는 **JK FF 이 D FF 와 같은 기능**을 하게 되는 것.
> 				- (J_n, K_n) = (0, 1) -> 0 으로 Reset (A_n)
> 				- (J_n, K_n) = (1, 0) -> 1 (A_n)
> 					- **값의 전달이 발생. (Parallel Load)**
> 	<br><br>
> 	- **Increment**
> 		![[스크린샷 2023-10-10 오후 3.33.47.png]]
> 		- **증가 (Counter 기능)**
> 		- **C 가 0이고, L이 0, I 가 1 인 경우에는 Increasing Counter 로 동작.**
> <br><br>
> ![[스크린샷 2023-10-10 오후 3.40.01.png]]
> - **모든 동작 (Clear, Load, Increment) 은 JK FF 에 연결된 Clock Pulse 가 상승할 때에 동작한다.**
> - **C, L, I 가 모두 0인 경우**
> 	- **J_n = 0, K_n = 0 이 되어 No Change.**

<br><br>
# Memory units
## RAM (Random Access Memory)
![[스크린샷 2023-10-10 오후 3.48.28.png]]
> **Word**
> - **하나의 Address 로 접근할 수 있는 Bit 의 단위.**
> <br><br>
> **RAM**
> - **읽기와 쓰기**가 가능한 메모리
> - **Read / Write Selection Signal 을 받아 해당 동작 수행.**
> 	- **ReadSignal**
> 		- **k Address lines 를 Input** 으로 받아 해당 위치에 존재하는 값을 Output 으로 반환.
> 	- **Write Signal**
> 		- **k Address lines 과 Write 대상 값을 Input** 으로 받아 k Address lines 에 Write 실행.

<br><br>

## ROM (Read Only Memory)
![[스크린샷 2023-10-10 오후 3.50.34.png]]
> **읽기만 가능한 Memory 이므로 Address 만 Input 으로 받아 해당 위치의 값을 Output 으로 반환.**

<br><br>

# In-Class Assignment

## Quiz
```
그림 2-11 의 병렬 로드를 가진 이진 카운터를 이용하여 나머지 -12 (modular-12) 카운터를 만들어 보아라. 나머지-12 카운터는 0에서부터 11까지 카운트하고 다시 0으로 되돌아가는 카운터를 말한다.
```

![[KakaoTalk_Photo_2023-10-10-19-57-21.jpeg]]
> **X 가 0 인경우**
> - **No Change**
> <br>
> **X 가 1 인 경우**
> - Clock Pulse 가 **상승할 때 A_n = I_n**

<br><br>

## 나머지 - 12 Counter
![[KakaoTalk_Photo_2023-10-10-20-18-42.jpeg]]
Input
- Clear
- Load
	- AND Gate 연결.
	- 
- Increment

