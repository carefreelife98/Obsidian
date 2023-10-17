# Register Transfer Language
> **CPU 는 Register 간의 전송(수학적 처리) 을 통해 Data 가 처리된다.**
> <br>
> **Micro Operations (마이크로 연산)**
> - **Register 에 저장된 Data 들의 조작.**
> 	- **한 Clock** 안에 Register 의 전송 동작.
> - Ex) shift, count, clear, load
> <br>
> **Register Transfer Language**
> - **Micro 연산을 간편하게 표현하는 언어**
> - **Register 는 (직)사각형 으로 표현**
> 	![[스크린샷 2023-10-17 오전 8.00.26.png]]
> 	- Register 의 **이름**으로 표현
> 	- Register 를 **구성하는 각 Bit** 로 표현
> 	- Register 의 **최상위, 최하위 Bit** 로 표현
> 	- Register 의 **상, 하위 Bit (H, L) 를 각각** 표현
> <br>
> **일반적인 표현 방식**
> ![[스크린샷 2023-10-17 오전 8.22.54.png]]
> ![[스크린샷 2023-10-17 오전 8.24.42.png]]
> - **P**
> 	- Control Circuit 에서 발생하는 신호
> - **R1, R2**
> 	- Register
> - **P 가 1일 때 Load 되어 R1 에 있는 값이 R2로 전송된다.**
> 	- 병렬 Load 를 가지는 Register
> 		- Load 가 0 일때 외부 입력 R1 이 R2 로 전송되는 방식으로 구현 가능. (파란색 Box 부분)
> - **Exchange, 즉 두 Register 의 값의 교환은 다음과 같이 표현 할 수 있다.**
> 	- **T: R2 <- R1, R1 <- R2**
> 	- " , (쉼표) " 는 동시에 발생하는 연산을 나타낼 때에 사용

<br><br>
## Register Transfer Language 의 구성요소
![[스크린샷 2023-10-17 오전 8.29.40.png]]
> **Register**
> - Alphabet 이나 숫자를 붙혀 표현
> - 최상/하위 Bit, 상/하위 Bit 등으로 표현
> <br>
> **Arrow (<-)** 
> - Data 의 전송을 표현
> - 방향은 (Destination <- Source) 으로 고정하여 사용.
> <br>
> **Comma ( , )**
> - 동시에 발생하는 Micro Operation 을 표현

<br><br>
# Bus and Memory Transfer
## Bus
![[스크린샷 2023-10-17 오전 8.34.28.png]]
> **Bus**
> - 공통 Line 의 Set
> - 복수 개의 Register 간의 Data Transfer 을 효율적으로 하기 위해 사용하는 System.
> - 실제로는 Bus 를 통해 Data Transfer 이 이루어지는 것이 맞지만, 편의상 RTL(Register Transfer Language) 에서는 Bus 표기를 생략하여 사용가능.
> <br>
> **Bus 예시**
> ![[스크린샷 2023-10-17 오전 8.44.26.png]]
> - 각각의 Register 는 4 X 1 MUX 에 연결되어 있고, **Selection Signal 에 따라 Register A, B, C, D 가 선택**된다. 이러한 방식으로 공통 Bus 가 구현됨.

<br><br>
## 3-State Bus Buffer
![[스크린샷 2023-10-17 오전 8.51.23.png]]
> **기존 Buffer 에 Control input C 가 존재.**
> - **C = 1**
> 	- Buffer 로서 동작.
> 	- State : 0, 1
> - **C = 0**
> 	- **강한 저항이 발생하여 Buffer output 의 회로가 끊어진 것처럼 동작.**
> 	- State : **High impedance**
> <br>
> **3-State Bus Buffer 를 이용한 MUX 의 구현**
> ![[스크린샷 2023-10-17 오전 8.57.29.png]]
> - Selection Signal (S1, S2) 가 0, 0 인 경우
> 	- **D_0 를 제외한 나머지 (D_1, D_2, D_3)는 Control Input 으로서 0이 전달됨.**
> 	- 이는 마치 **해당 회로가 끊긴 것처럼 동작**하게 된다. (High-Impedance) 
> 	- 따라서, **A_0 만 Buffer 로서 동작**하게 된다.

<br><br>
## Memory Transfer
> Register 간의 전송 뿐만 아니라, **Memory 와 Register 간의 전송도 존재.**
> - **Memory 로부터 Read / Write 수행.**
> <br>
> **M[ AR ]**
> - Memory 를 표현.
> - \[ ] 안에는 Memory 의 Address 가 들어가게 되는데, 이는 Address 가 저장된 Register (AR) 에 존재. 
> <br>
> **Read: DR <- M\[AR]**
> ![[스크린샷 2023-10-17 오전 10.37.32.png]]
> <br>
> **Write: M\[AR] <- R1**
> ![[스크린샷 2023-10-17 오전 10.38.57.png]]

<br><br>
# Arithmetic Micro-Operations
## 4-Bit Binary Adder
![[스크린샷 2023-10-17 오전 10.51.47.png]]
> Full Adder 4개를 Serial 로 연결.
> 각 하위 Adder 의 Carry 를 상위 Adder 의 Carry 로 연결.

<br><br>
## Binary Adder-Subtractor
> **M 이라는 값에 의해 Adder or Subtractor 로서 동작**
> <br>
> **M = 0 인 경우**
> ![[스크린샷 2023-10-17 오전 10.54.19.png]]
> - **B_i XOR 0 = B_i**
> - C_0 = 0
> - 4-Bit Adder 로서 동작. (Addition)
> <br>
> **M = 1 인 경우**
> ![[스크린샷 2023-10-17 오전 11.00.41.png]]
> - **B_i XOR 1 = (B_i)'**
> - C_0 = 1
> - **A + 2's Complement of B -> A - B (Subtraction)**

<br><br>
## Binary Incrementer
![[스크린샷 2023-10-17 오전 11.03.42.png]]
> Adder 와 다르게 **단순히 1씩 증가하는 Logic 을 가졌으므로 Half Adder** 만으로도 구현이 가능하다.

<br><br>
## Arithmetic Circuit

> **위에서 알아본 Adder-Subtractor, Increment, Decrement 을 통해 Arithmetic Circuit 을 구현할 수 있다.**
> <br>
> **Arithmetic Circuit 분석**
> 1. Full Adder 4개
> 	- input 3개
> 		- A_i = X_i
> 		- 4 by 1 MUX = Y_i
> 			- Selection Signal (S_0, S_1) 에 의해 4 by 1 MUX 의 Output (0, 1, 2, 3) 이 결정됨.
> 		- C_input, C_i
> 	- 