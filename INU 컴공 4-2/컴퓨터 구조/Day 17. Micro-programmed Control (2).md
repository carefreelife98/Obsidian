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
### F Field 정의 (Function Field)
![[스크린샷 2023-11-21 오후 3.13.29.png]]

<br><br>

### CD Field 정의 (Condition field)
![[스크린샷 2023-11-21 오후 3.13.51.png]]
> **00 인 경우**
> - 조건 : always 1 (참)
> - **Unconditional Branch 수행**
> 	- BR 과 AD bit 를 참조하여 점프
> 
> <br>
> **01 인 경우**
> - 조건 : DR(15)
> 	- **Data Register 의 최상위 bit 를 의미.**
> 		- Instruction Register 가 따로 존재하지 않으므로 명령어를 DR 에 저장.
> 		- 따라서 명령어의 최상위 비트 I 를 참조하여 Direct / Indirect Address 를 구분.
> 	- 조건이 참 (DR(15) = 1) 이면 I = 1 이므로 해당 명령어는 Indirect Address.
> 	- 거짓이면 Direct Address.
> - **명령어의 최상위 비트 I 참조를 수행**
> 
> <br>
> **10 인 경우**
> - 조건 : AC(15)
> 	- **Accumulator 의 최상위 bit 를 참조.**
> 		- Accumulator 의 최상위 비트는 부호를 의미함. (= Sign bit)
> 			- 0: 양수
> 			- 1: 음수
> 
> <br>
> **11 인 경우**
> - 조건 : AC = 0
> 	- **Accumulator 자체의 값을 참조**
> 		- AC 값이 0 이면 참.
	

### BR Field 정의 (Branch Field)
![[스크린샷 2023-11-21 오후 3.14.03.png]]
> **BR 의 Function은 전부 CAR 값을 변경하는 것.**
> - CAR 에는 Control Memory 에 적재되어 있는 Micro-instruction 의 주소가 저장되어 있음.
> - 어떠한 Micro-instruction 을 수행할 것인지 결정하는 것.
> 
> <br>
> **00 인 경우 : Jump**
> - **조건 field 가 1이면 AD(Address field) 로 Jump.**
> 	- **CAR <- AD**
> 		- Jump 를 수행.
> 		- Return Address 를 고려하지 않음.
> - **조건이 0 인 경우 CAR <- CAR + 1 수행**
> 	- 다음 명령어를 수행.
> 
> <br>
> **01 인 경우 : Call**
> - **조건 field 가 1이면 Subroutine 으로 Jump.**
> 	- **CAR <- AD, SBR <- CAR + 1**
> 		- Return Address 를 SBR 에 저장해놓은 후 Jump.
> - **조건이 0 인 경우 CAR <- CAR + 1 수행**
> 	- 다음 명령어를 수행
> 
> <br>
> **10 인 경우 : Return**
> - **Return Address 로 Jump.**
> 	- **CAR <- SBR (return from subroutine)**
> 		- Subroutine Register 에 저장되어 있는 Return Address 를 CAR 에 저장.
> 
> <br>
> **11 인 경우 : Mapping**
> - **Micro Instruction 의 14 ~ 11 bit (Opcode) 를 CAR(7-bit) 에 넣기 위해서 Mapping 하는 과정.**
> - Opcode 의 앞에 1 bit(0), 뒤에 2 bit(00) 를 추가. (0 Opcode 00)
> 	- **CAR(2-5) <- DR(11 - 14), CAR(0, 1, 6) <- 0**

<br><br>

# Symbolic Micro-Program
## FETCH Routine
> **3개의 Micro-instruction 에 의해 Fetch 가 수행된다.**
> - Mapping 과정 수행<br>
> **\[FETCH]**
> 1. AR <- PC
> 2. DR <- M\[AR], PC <- PC + 1
> 3. AR <- DR(0-10), Mapping (CAR(2-5) <- DR(11-14), CAR(0, 1, 6) <- 0)


### FETCH 수행 과정의 실제 Binary 값
> **ORG 64** : FETCH 를 수행하는 **시작 번지를 나타냄.**<br>
> **NEXT : 다음 번지를 나타냄.**
> ![[스크린샷 2023-11-21 오후 3.52.24.png]]
> `Table` 

**ORG 64**

|LABEL|F SYMBOL|CD|BR|Address|
|:---:|:---:|:---:|:---:|:---:|
|FETCH|PCTAR|U|JMP|NEXT|
||READ, INCPC|U|JMP|NEXT|
||DRTAR|U|MAP||

> **Table 과 위 표를 참조하여 아래와 같이 FETCH 수행 과정의 실제 Binary 코드가 어떻게 존재하는지 알 수 있다.**

|LABEL|F SYMBOL|Control Memory Address|F1|F2|F3|CD|BR|Address Field|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|FETCH|PCTAR|1000000|110|000|000|00|00|1000001|
||READ, INCPC|1000001|000|100|101|00|00|1000010|
||DRTAR|1000010|101|000|000|00|11|0000000|

> **다음과 같이 Control Memory 의 각 주소에는 각 F1 ~ Address Field 의 총 20 bit 가 저장된다.**
> - Control Memory(1000000) : 110 000 000 00 00 1000001
> - Control Memory(1000001) : 000 100 101 00 00 1000010
> - Control Memory(1000010) : 101 000 000 00 11 0000000

<br><br>

## ADD Routine
![[스크린샷 2023-11-21 오후 4.07.53.png]]
`Table`

> **ORG 0, INDRCT 67**

|LABEL|F SYMBOL|CD|BR|Address|
|:---:|:---:|:---:|:---:|:---:|
|ADD|NOP|I|CALL|INDRCT|
||READ|U|JMP|NEXT|
||ADD|U|JMP|FETCH|

> **FETCH 와 마찬가지로 Table 과 위 표를 참조하여 다음과 같이 실제 Binary 값을 알 수 있다.**

|LABEL|F SYMBOL|Control Memory Address|F1|F2|F3|CD|BR|Address Field|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|ADD|NOP|0000000|000|000|000|01|01|1000011|
||READ|0000001|000|100|000|00|00|0000010|
||ADD|0000010|001|000|000|00|00|1000000|
> **NOP: Indirect Address 의 최상위 bit 가 1 이므로 CD = 1 이 되어 CALL(1) : CAR <- AD, SBR <- CAR + 1 을 수행.**
> - 다음 명령어를 수행하게 된다. (0000001)

### ADD ERROR 예방
![[스크린샷 2023-11-21 오후 4.16.52.png]]
> Control Memory Address 는 0 0000 00 부터 0 0000 11 까지 **총 4개의 Microoperation 을 수행할 수 있으나, 위 ADD 연산에서는 3가지의 Microoperation 만 정의되어 있음.**
> - Error 가 발생하여 0 0000 11 이 호출되는 경우를 대비하여 **일반적으로 남는 Microoperation 단계는 아래와 같이 FETCH 단계로 무조건 점프하는 Microoperation 을 정의해두는 것이 좋다.**

|LABEL|F SYMBOL|Control Memory Address|F1|F2|F3|CD|BR|Address Field|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|ADD|NOP|0000000|000|000|000|01|01|1000011|
||READ|0000001|000|100|000|00|00|0000010|
||ADD|0000010|001|000|000|00|00|1000000|
||ERROR|0000011|000|000|000|00|00|1000000|

> **0000011 이 호출된 경우**
> - F(1~3) : **No Operation**
> - CD : **Unconditionally**
> - BR : **Jump**
> - AD : **FETCH**

<br><br>

# Design of Control Unit
> 각 Control Unit 에 대한 하드웨어 설계 예시를 알아보자.
## Function (F1 ~ F3)
![[스크린샷 2023-11-21 오후 4.28.14.png]]
> **Decoding of F fields**
> - F1 ~ F3 를 3 x 8 Decoder 를 통해 Decode 하는 Logic 설계 필요.
> - Arithmetic Logic Shift Unit 설계 필요.

<br><br>

# QnA
> Memory 에 적재된 명령어가 Micro-program 에서 동작하기 위해서 ->
> Instruction 의 Opcode 4-bit -> Micro-instruction 의 AD 에 7-bit 로 변환 저장.
> - AD 의 7-bit 중 최상위 비트가 D / Indirect 지정?
> - 그럼 Instruction 의 I bit 는 No function?



# Quiz

![[KakaoTalk_Photo_2023-11-21-20-34-24.jpeg]]![[스크린샷 2023-11-21 오후 8.38.12.png]]