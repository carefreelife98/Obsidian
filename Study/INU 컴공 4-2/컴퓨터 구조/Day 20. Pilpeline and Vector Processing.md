---
Author: CarefreeLife98
Date: 2023-12-05T15:04:00
Agenda: 
tags:
  - Computer_Structure
---
# Parallel Processing
> **하나의 커다란 Process 를 여러 개로 쪼개어 처리하는 것.**

<br><br>

# Pipelining
![[스크린샷 2023-12-05 오후 3.19.23.png]]

## Pipelining 시 전체 Task 의 수행 시간
![[스크린샷 2023-12-05 오후 3.32.46.png]]
> **6개의 Task 를 처리하는데에 소요되는 시간: (k + n - 1)t_p**
> - **첫번째 Task(T1):** 
> 	- **k(segment 의 개수) * t_p(Clock cycle time)**
> - **두번째 ~ 마지막 Task(T2 ~ Tn):**
> 	- **(n-1) * t_p**

<br><br>

## Pipelining - Speedup
![[스크린샷 2023-12-05 오후 3.52.18.png]]
> **Speedup: S = n * t_n / (k + n - 1) * t_p**
> - S 는 속도 향상률.
> - **t_n 은 none pipelining** 을 나타냄. (pipelining 을 하지 않았을 때 하나의 Task 를 처리하는데 걸리는 시간)
> - **n(Task 의 개수)이 무한정 증가할 시, S = t_n / t_p 가 된다.**
> - **하지만 Pipelining 을 하게 되면,**
> 	- **분자가 t_n 이 아닌 k * t_p 가 되어 S = k 가 된다.**
> - **이론상 가장 빠른 속도를 가짐. (최대 처리 속도 향상률)**
> 	- 최대 세그먼트의 개수(k)만큼 처리 속도 향상.

<br><br>

# Instruction Pipeline
> **4 - Segment Instruction Pipeline**
> - **4가지의 Segment(동작) 로 나누어 하나의 명령어를 수행.**
> - 4가지 Segment
> 	1. **FI (Fetch Instruction)**: Memory 로부터 instruction 을 가져오는 FETCH Segment
> 	2. **DA (Decode Address)**: 명령어를 해독하고, 피연산자의 유효주소를 계산하는 Segment.
> 	3. **FO (Fetch Operand)**: Memory 로부터 Operand 를 가져오는 Segment.
> 	4. **EX (Execution)**: 동작을 실행하는 Segment

<br><br>

# Pipelining 시 주요한 위험 요소
![[스크린샷 2023-12-05 오후 4.30.01.png]]
> 1. **Structural Hazard (resource conflict)**
> 	- 구조적 위험 (자원 충돌)
> 	- **여러 Segment 가 동시에 하나의 Memory 에 접근하려 하는 경우.**
> 		- Memory access port 가 하나이면 충돌 발생
> 2. **Data Hazard (data dependencty conflict)**
> 	- 데이터 위험 (데이터 의존성 문제)
> 	- **Segment 가 일을 수행하고자 할 때 미리 Data가 준비되어야 하는데 (Register 에 Data 가 저장되어 있어야 하는데) 이전의 다른 Segment 에서 아직 해당 Register에 Data 가 준비되지 않은 경우.**
> 3. **Control Hazard (branch address dependency conflict)**
> 	- 제어 위험
> 	- **Branch 등과 같은 PC 값을 변경하는 명령어는 해당 segment 의 수행 마지막 동작에서 PC 값을 변경하므로 해당 segment 가 종료될 때 까지 타 segment 들은 수행되지 못하고 지연된다.**

<br><br>

## Structural Hazard (Resource Conflict)
> **구조적 위험, 자원 충돌의 문제.**
> ![[스크린샷 2023-12-05 오후 4.32.54.png]]
> **문제 해결 방법 1. 지연 실행**
> - **하나의 memory port 를 가진 경우, DA(Data) 와 FI(Fetch instruction) 은 동시에 수행되지 못함.**
> 	- 따라서 **memory 를 동시에 사용하지 않는 조건을 충족 할 때까지 충돌 위험 segment 는 지연 실행** 시킨다.
> 
> <br>
> **문제 해결 방법 2. 메모리 포트 구분**
> - **메모리를 사용하는 각 프로그램 영역에 대하여 서로 다른 메모리 접근 포트를 지정.**

<br><br>

## Data Hazard
> **데이터 위험, 데이터 의존성 충돌 문제.**
> - 한 Instruction 의 실행이 이전 instruction 의 수행 결과 (data) 에 의존되는 경우 발생.
> 
> <br>
> ![[스크린샷 2023-12-05 오후 4.43.33.png]]
> - **첫번째 segment 에서 + 한 결과를 R1에 저장하는 동작과 두번째 segment 에서 R1의 값을 참조하는 동작을 동시에 수행하게 되면 Conflict 발생.**
> 	- 두번째 segment 의 R1 참조 동작을 지연 실행하여 해결.

<br><br>

## Control Hazard
> **Branch Address 의 의존성 문제.**
> - Branch 의 경우 특정 명령어로 점프하는 동작이 마지막에 수행되는데, 다음 Segment 에서 곧바로 Fetch 동작을 수행하면 점프한 명령어를 가져오지 못함.
> 
> <br>
> ![[스크린샷 2023-12-05 오후 4.50.50.png]]
> - **Delayed Branch 로서 해결가능.**

<br><br>

# RISC Pipeline
> **Three - Segment Instruction Pipeline**
> 1. **Data Manipulation Instructions**
> 	-  I(S1): Instruction Fetch
> 	- A(S2): Decode, Read Registers, ALU Operations
> 	- E(S3): Write a Register
> 2. **Data Transfer Instructions (Load/Store)**
> 	- I(S1): Instruction Fetch
> 	- A(S2): Decode, Evaluate Effective Address
> 	- E(S3): Register-to-memory or Memory-to-register
> 3. **Program Control Instructions**
> 	- I(S1): Instruction Fetch
> 	- A(S2): Decode, Evaluate Branch Address
> 	- E(S3): Write Register (PC)

<br><br>

## Data Conflict -> Delayed Load 예시
![[스크린샷 2023-12-05 오후 5.24.29.png]]
> Clock 3 의 A 동작에서 Clock 1,2 의 수행 결과인 **피연산자(R1, R2)를 가져오고 연산 수행까지 해야 하는데, Clock 2 가 수행 완료되지 않고Execute 중인 상태. (Data conflict 발생)**
> - **Clock 3 에 No-opeartion 을 넣어 ADD 연산을 Data 가 전부 준비 될 때까지(Clock1, Clock2) Delay 시킨다.**

<br><br>

## Pipeline timing with Branch Address Conflict 예시
> **Clock 5 에서 X 위치로 Jump 해야 (PC 값이 해당 명령어로 변환되어야) 다음 수행 동작 (Clock 6) 이 해당 PC 값을 바탕으로 Fetch 하며 수행 될 수 있다.**
> - Branch 명령어가 수행되어 **PC 값이 변환되기 전에 Clock 6 가 수행되어 Fetch Instruction X 를 수행하므로 충돌 발생.**
> 
> <br>
> **[해결 방법]**
> 1. **Delayed Branch (Branch 지연 수행)**
> 	![[스크린샷 2023-12-05 오후 5.33.27.png]]
> 	- **Clock 6, 7 에 No-operation 을 넣어 Clock 5 가 수행 종료되어 PC 값이 변환된 후 Clock 8 을 수행하도록 Delay.**
> 	
> 	<br>
> 2. **Rearranging (명령어 재배치)**
> 	![[스크린샷 2023-12-05 오후 5.34.05.png]]
> 	- **Branch 명령 수행 시점을 앞당겨 수행.**
> 	- **ADD, Subtract 명령은 앞당겨진 Branch 명령어에 의한 영향을 받지 않음**
> 		- ADD, Subtract 명령어의 Fetch 동작이 수행되기 전까지 Branch 명령이 종료되지 않으므로.(PC 값이 유지됨)
> 	- **Clock 3 로 앞당겨진 Branch 명령어가 종료된 직후 Clock 6 의 Fetch 동작이 수행되므로 올바르게 동작.**


