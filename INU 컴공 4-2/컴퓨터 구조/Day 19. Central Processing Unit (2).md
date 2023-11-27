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
> **다양한 Addressing modes 를 사용함으로서 프로그램을 효율적으로 작성할 수 있게 된다.**
> 
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
> <br>
> - **Auto-increment / decrement mode**
> 	- Register mode 와 같으나 **값이 1씩 증/ 감 하는 mode.**
> <br>
> - **Direct / Indirect mode**
> 	- 피연산자의 Address field 값이 직접주소가 되거나 간접주소가 되는 경우.
> <br>
> - **Relative address mode, Indexed addressing mode, Base register address mode**
> 	- **Relative address mode** : 프로그램 카운터와 더하여 Address 를 지정하는 방식
> 	- **Indexed addressing mode** : index address 라는 특정 address 와 더하여 address 를 지정하는 방식

<br><br>

