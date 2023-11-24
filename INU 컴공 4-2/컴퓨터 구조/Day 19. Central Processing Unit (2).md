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
