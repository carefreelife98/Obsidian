---
Author: CarefreeLife98
Date: 2023-11-24T11:34:00
Agenda: 
tags:
  - Computer_Structure
---
# Instruction Format
> **범용 Register 구조 및 Stack 구조에서는 명령어의 Format 이 어떻게 달라지는가?**
> <br>
> **Single Accumulator Organization**
> - ADD X : AC <- AC + X
> <br>
> **General Register Organization**
> - **범용 Register 구조에서는 Source 와 Destination 을 전부 정의 해주어야 한다.**
> 	- ADD R1, R2, R3; R1 <- R2 + R3
> 	- ADD R1, R2; R1 <- R1 + R2
> <br>
> **Instruction Format for Stack Organization**
> - **Stack 이라는 구조의 특성이 반영**되어 있으므로 ADD 와 같은 명령어는 피연산자가 정의되어 있지 않아도 된다.
> 	- **PUSH X**
> 	- **ADD**
> <br>
> **즉, 컴퓨터의 구조에 따라서 명령어 Format 의 요소가 정의되는 방식이 다를 수 있다.**
> - **Three address instruction**
> - **Two address instruction**
> - **One address instruction**
> - **Zero address instruction**

<br><br>
## Three address instructions 예시
> **X = (A + B) * (C + D)**
> - X = (A 와 B 의 주소를 더한 값) * (C 와 D 의 주소를 더한 값)
> 
> <br>
> **Three address instructions**
> 1. **ADD R1, A, B ;**
> 	- R1 <- M\[A] + M\[B]
> 2. **ADD R2, C, D ;**
> 	- R2 <- M\[C] + M\[D]
> 3. **MUL X, R1, R2 ;**
> 	- M\[X] <- R1 * R2
> <br>
> **결과적으로 프로그램의 길이는 짧아지고, 프로그램을 구성하고 있는 각 명령어의 길이는 길어진다.**

<br><br>

## Two address instructions 예시
> **가장 일반적인 형태.**<br><br>
> **X = (A + B) * (C + D)**
> - X = (A 와 B 의 주소를 더한 값) * (C 와 D 의 주소를 더한 값)
> 
> <br>
> 1. **MOV R1, A**
> 	- R1 <- M\[A]
> 2. **ADD R1, B**
> 	- R1 <- R1 + M\[B]
> 3. **MOV R2, C**
> 	- R2 <- M\[C]
> 4. **ADD R2, D**
> 	- R2 <- R2 + M\[D]
> 5. **MUL R1, R2**
> 	- R1 <- R1 * R2
> 6. **MOV X, R1**
> 	- M\[X] <- R1
> <br>
> **결과적으로, 각 명령어들의 길이는 짧아지지만 프로그램의 길이는 길어진다.**

<br><br>

## One address instructions
> **Implied accumulator register**
> - 명령어를 통해 명시적으로 표현하지 않지만 **내재적으로 AC 에서 연산이 이루어지는 것을 가정하는 구조.**
> - **명령어 형식은 짧아지고, 프로그램의 길이는 길어진다.** 
> <br>
> 1. **LOAD A**
> 	- AC <- M\[A]
> 2. **ADD B**
> 	- AC <- AC + M\[B]
> 3. **STORE T**
> 	- M\[T] <- AC
> 4. **LOAD C**
> 	- AC <- M\[C]
> 5. **ADD D**
> 	- AC <- AC + M\[D]
> 6. **MUL T** 
> 	- AC <- AC * M\[T]
> 7. **STORE X**
> 	- M\[X] <- AC

<br><br>

## Zero address instruction
> **Stack organized computer**
> - Stack 구조를 가진 Computer 인 경우.
> 
> <br>
> 1. **PUSH A**
> 	- TOS <- A
> 2. **PUSH B**
> 	- TOS <- B
> 3. **ADD**
> 	- TOS <- (A + B)
> 4. **PUSH C**
> 	- TOS <- C
> 5. **PUSH D**
> 	- TOS <- D
> 6. **ADD**
> 	- TOS <- (C + D)
> 7. **MUL**
> 	- TOS <- (C + D) * (A + B)
> 8. **POP X**
> 	- M\[X] <- TOS
> 	- **X 주소에 최종 결과를 write**

<br><br>

# Addressing modes
![[스크린샷 2023-11-28 오후 4.21.10.png]]
> **다양한 Addressing modes 를 사용함으로서 프로그램을 효율적으로 작성할 수 있게 된다.**
> ![[스크린샷 2023-11-28 오후 4.21.47.png]]
> <br>
> - **Implied mode**
> 	- **실제 주소 지정없이 사용되는 mode.**
> 	- 예: ADD X
> 		- AC <- AC + Y
> 		- **피연산자 하나는 Accumulator 이고, Destination 역시 Accumulator 임을 내제하고 있음.**
> <br>
> - **Immediate mode**
> 	- **Address field 자체가 연산의 대상**이 되는 경우.
> <br>
> - **Register mode, Register indirect mode**
> 	- **Register 에 있는 값이 유효 주소**가 될 때.
> 	- Register 에 있는 값이 메모리 주소가 되어 해당 **메모리 주소에 있는 값이 유효 주소**가 될 때. (Indirect)
> 		- **register Indirect** : 해당 Register 의 값이 EA(유효주소) 가 되어 M\[해당 Register 값] 에 저장된 값이 AC 에 저장된다.
> <br>
> - **Auto-increment / decrement mode**
> 	- Register mode 와 같으나 **값이 1씩 증/ 감 하는 mode.**
> 	- **Auto-increment**
> 		- **Register mode 의 명령어가 수행 후** Register 에 저장된 유효주소 값이 1 증가한다.
> 	- **Auto-decrement**
> 		- **Register mode 의 명령어 수행 전에**Register 에 저장된 값이 1 감소하고, 해당 값이 EA(유효주소)가 된다.
> <br>
> - **Direct / Indirect mode**
> 	- 피연산자의 Address field 값이 직접주소가 되거나 간접주소가 되는 경우.
> <br>
> - **Relative address mode, Indexed addressing mode, Base register address mode**
> 	- **Relative address mode** : 프로그램 카운터와 더하여 Address 를 지정하는 방식
> 	- **Indexed addressing mode** : index address 라는 특정 address 와 더하여 address 를 지정하는 방식

<br><br>

# Program Control Instructions
![[스크린샷 2023-11-28 오후 4.34.46.png]]
> **Program Control Instructions**
> - **PC(Program Counter) 값을 변화시키는 명령어.**
> 	- Branch, Skip, Call, Return ...
> - **참조되는 조건 bit** 를 이용하여 Program control 이 이루어짐.
> 	- C, S, Z, V

<br><br>

## 조건 Bit
![[스크린샷 2023-11-28 오후 4.34.33.png]]
> - **C : Carry**
> 	- End carry 가 1인 경우 C는 1로 Set.
> - **S : Sign**
> 	- 산술 논리 연산의 대상이 되는 Register 의 최상위 Bit를 나타냄.
> 		- 1이면 S는 1로 Set.
> - **Z : Zero**
> 	- 산술 논리 연산의 결과가 0인 경우
> 		- 예(AC) : AC 의 모든 Bit 가 0인 경우 Z는 1로 Set.
> - **V : Overflow**
> 	- 최상위 2개 bit 가 서로 다르면 (Exclusive-OR) overflow 발생 가능.
> 	- 최상위 2개 Bit XOR 시 1인 경우 V는 1로 Set.

<br><br>
**위 조건식을 회로 구성하여 구현하면 다음과 같다. (Status Register)**
> ![[스크린샷 2023-11-28 오후 4.41.49.png]]

<br><br>

## Conditional Branch Instructions
> ![[스크린샷 2023-11-28 오후 4.39.32.png]]

<br><br>

## Subroutine Call and Return
> **Subroutine Call and Return**
> - **자주 사용하는 명령어에 대해서 해당 명령어로 Jump 하여 명령 수행 후 앞서 진행했던 명령어의 다음 명령어로 돌아가는 것.**
> - Main Program 에서 자주 사용.
> 
> <br>
> - **Category**
> 	- Call Subroutine
> 	- Jump to Subroutine
> 	- Branch to Subroutine
> 	- Branch and Save Address
> 
> <br>
> - **Logic**
> 	- 한 명령어가 끝난 후 PC 값을 Jump할 Subroutine 의 시작 주소로 바꾼다.
> 	- PC 값은 1이 증가하여 다음 명령어(Subroutine 수행 후 돌아갈 주소) 를 가지고 있으므로 해당 값(Return address)을 임시로 다른 곳에 저장해두어야 함.
> 	- Subroutine 이 끝난 후 임시 저장소에 저장되어 있는 Return address 를 PC 에 전송하여 Jump 했던 지점으로 돌아간다.

<br><br>

### \[STACK] Subroutine Call and Return

> Subroutine 을 Call 하는 방법 중 **가장 효율적인 방법은 Memory Stack 에 Return Address 를 저장하는 방법**이다.
> - **Memory Stack 은 순환적인 Subroutine 에 효율적.**
> <br>
> 1. **[PUSH] Subroutine Call**
> 	- SP <- SP + 1
> 	- M\[SP] <- PC
> 	- PC <- EA
> 	<br>
> 2. **[POP] Return from subroutine**
> 	- PC <- M\[SP]
> 	- SP <- SP - 1
> 
> <br>
> **예시**
> - **%esp : Stack Pointer (SP)**
> - **%eip : Program Counter (PC)**
> <br>
> 1. **\[Subroutine CALL]**
> 	![[스크린샷 2023-11-29 오후 3.33.54.png]]
> 	- **804854e 번지의 명령어는 e8 3d 06 00 00**
> 		- 해당 명령어는 8048b90 번지(Subroutine)를 call 하는 명령어.
> 		- 현재 PC 값은 804854e (현재 실행 할 명령어의 주소값)
> 	- **다음 명령어는 8048553 번지이고, 이는 Return Address 가 된다.**
> 		- 따라서 **Stack 에 Return address 인 8048553 번지를 Push.**
> 		- Stack Pointer 는 하나 증가 (현 예에서는 감소)
> 		- **PC 에 Subroutine 시작 주소인 8048b90 값을 넣어 Call.**
> 
> 	<br>
> 2. **[Subroutine 수행 후 Return]**
> 	![[스크린샷 2023-11-29 오후 3.35.27.png]]
> 	- **Subroutine 수행 종료 후 Return 명령 실행.**
> 		- 현재 PC 값은 Return 명령어의 주소인 8048591.
> 		- Stack Pointer (%esp) 와 Stack 의 구조는 Subroutine 시작 시점과 동일.
> 	- **Stack Pointer 가 가리키고 있는 주소가 Return Address 이므로 POP.**
> 		- **SP 가 가리키고 있는 Stack 의 104 번지 값인 8048553 이 POP 되어 PC(%eip) 에 저장되고 SP(%esp) 는 하나 증가. (현 예에서는 감소 = 108)**

<br><br>

## Program Interrupt
> **Program Interrupt 와 Subroutine Call 의 차이점**
> - Subroutine Call 은 Program 수행 중 Program 내부에서 특정 Subroutine 으로 점프하여 수행 후 Return.
> 	- **Interrupt 는 Subroutine Call 이 Program 의 외부에서 Trigger 됨.**
> 	- **I/O Device 에 의해 Program 외부에서 Subroutine Call 이 발생함.**
> 	- 