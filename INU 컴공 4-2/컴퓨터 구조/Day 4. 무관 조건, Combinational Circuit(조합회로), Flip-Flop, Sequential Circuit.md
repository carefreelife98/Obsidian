# 무관 조건 (Don't care condition)
- **Function 에서 무관조건에 해당하는 Index는 0, 1 중 어떤 값이더라도 결과에 영향을 주지 않는다.**
	- **어떤 값이어도 상관없는 항** (x로 표시)
	- 해당 항은 **K-map값이 1인 항과 같이 묶을 수 있는 항.**

<br><br>
## 문제 유형 1. SOP
![[스크린샷 2023-09-20 오전 2.22.53.png]]
- **항의 개수는 최대화 (2의 제곱값 기준, 무관조건 해당 항(x) 포함)**
	- 무관 조건이 없던 기존의 경우 1의 값을 가진 Cell 만 묶음.
		- F = A'C' + BC'
	- 무관 조건이 있는 경우 무관 조건인 Cell 까지 최대화 하여 묶음.
		- **F = A' + BC'**
- **그룹의 개수는 최소화 하여 K-map 작성.**

<br><br>

## 문제 유형 2. Equation
![[스크린샷 2023-09-20 오전 2.29.28.png]]
- **Equation 으로 문제가 주어졌을 시**
	1. **변수의 개수 파악.**
		- 3개 (A, B, C)
	2.  **그에 맞는 카르노 맵 작성.**
	3. **최대 항, 최소 그룹으로 묶어 간소화.**
		- F = C + A'B

<br><br>

## 문제 유형 3. 간소화의 여러 가지 경우의 수 발생
### 1. 간소화 결과 Literal(변수) 이 4개 발생한 경우
![[스크린샷 2023-09-20 오전 2.37.03.png]]
<br><br>
### 2. 간소화 결과 Literal(변수) 이 3개 발생한 경우
![[스크린샷 2023-09-20 오전 2.38.52.png]]
- 위 예시처럼 **여러 가지의 간소화 결과 발생 시 두 가지 답 모두 틀린 것은 아니다.**
- 하지만 **Literal 개수 관점에서 더 적은 Literal 을 가진 결과가 더 잘 간소화** 했다고 볼 수 있다.
	- 이 결과를 도출하기 위해 타 minterm 에서 **이전에 이미 묶여졌던 항을 위주로 묶는다면** (예시 2번처럼) **결과 도출 시 공유하고 있는 변수가 많을 확률**이 높아 다시 한번 간소화 가능.

<br><br>

# Combinational Circuits (조합 회로)
![[스크린샷 2023-09-20 오전 2.47.40.png]]
- **말 그대로 Gate 들이 조합되어 있는 회로.**
	- **쉽게 말해서 Flip-Flop 없이 Gateway 로만 이루어진 것.**
- **Output 이 Input 에 의해 즉각 결정됨.**
	- **n Input** Variables `-- Combinational Circuit -->` **m Output** Variables 
	- **`복수 Input, 복수 Output`** 을 가질 수 있다.


<br><br>

## Half Adder (반 가산기)
![[스크린샷 2023-09-20 오전 2.51.15.png]]
- **두 개의 Input** 을 가지는 Adder.
	- **두 개의 비트를 받아서 Sum 과 Carry 의 결과를 출력하는 회로.**
- **Carry 가 존재 (1 + 1 시에만)**
	- 1 + 1 은 **Sum 이 0 이 되고, 대신 Carry 에 1이 넘겨짐**
### Half Adder 의 진리표 (Truth Table), Function, Circuit
![[스크린샷 2023-09-20 오전 2.54.43.png]]
- **Half Adder Function**
	- **S 가 1인 경우 = x XOR y**
		- 0, 1
		- 1, 0
	- **C 가 1인 경우 = x AND y**
		- 1, 1
	- 따라서
		- S의 Function은 **x XOR y**
		- C 의 Function 은 **x AND y**

<br><br>

## Full Adder (가산기)
![[스크린샷 2023-09-20 오전 3.19.24.png]]
- n Bit 가산기
- 비트 계산 중 Carry 가 발생하여 기존 x, y 의 값에 Carry 값까지 더해줘야 하는 상황에 필요.
- **x 값 + y 값 + Carry 값** -> **`총 세개의 Input이 존재.`**
	- Output 은 Sum 과 Carry. (두 개)
- **반 가산기 두 개로 구현**이 가능하다.

<br><br>

![[스크린샷 2023-09-20 오전 3.34.23.png]]
1. **Full Adder 진리표 구하기**
	- x + y + z 의 결과(S) 와 Carry(C) 를 구한다.
2. 그 중 **S 의 진리표를 바탕으로 K-map 을 그리고, 간소화 하여 Equation**을 구한다.


![[스크린샷 2023-09-20 오전 4.02.00.png]]
1. 이전에 구한 S 를 가지고 C 의 K-map을 작성.
2. K-map을 바탕으로 간소화 하여 C의 Equation을 구한다.

![[스크린샷 2023-09-20 오전 4.17.45.png]]
- **위에서 구한 Full Adder 의 Circuit.**

### Half Adder 2개를 사용해서 Full Adder 만들기
![[스크린샷 2023-09-20 오전 4.21.15.png]]
- 이전에 그린 Full Adder 의 Circuit 을 자세히 보면 Half Adder 2개가 보인다.


# Sequential Circuits
- **기억 장치 존재.**
	- 이전의 **상태(State)** 존재.
	- Combination Circuit 과 다르게 **진리표가 아닌 State Table 사용**
		- **Q(t+1) -> 다음 상태** 개념을 사용
	- **출력은 이전 상태와 입력에 따라 결정**됨.
	- **순서, 시간의 개념** 추가됨.

# Flip-Flop (암기 필수)
> **한 비트에 데이터를 저장하는 장치.**
> **Sequencial Circuit 을 구현하는 데에 있어 가장 중요한 장치.**
> 
> > **Clock Pulse 가 존재.
> > Clock Pulse 가 1이 되는 순간 Output의 상태가 변한다.
> >	- Clock Pulse 에 NOT 이 붙으면 0이 되는 순간 Output의 상태가 바뀜.**
- Characteristic Table (특성표, 동작표)
- Excitation Table (여기표)
	- 현재 상태와 다음 상태를 결정짓게하는 Flip-Flop의 Input
	- D, JK, T
	- x 가 의미하는 바는 No - care. 
		- 0,1 아무거나 와도 상관없다.
## SR Flip-Flop
![[스크린샷 2023-09-21 오전 3.53.41.png]]
- Input
	- S : Set (1로 만드는 것)
	- R : Reset (0으로 만드는 것)
- Output (상태)
	- Q
	- Q' (Complement)
### SR Flip-Flop 특성표 (Characteristic Table)
![[스크린샷 2023-09-21 오전 3.53.51.png]]

## D Flip-Flop
![[스크린샷 2023-09-21 오전 3.58.54.png]]
> **하나의 Input**.
   간단함.
- Input
	- D (Direct, Data - Input 의 값이 바로 Output 이 됨.)
		- **Input (d) 이 1이면 Q(t+1) = 1**
		- **Input (d) 이 0이면 Q(t+1) = 0**
- Output (상태)
	- Q
	- Q' (Complement)

## JK Flip-Flop
![[스크린샷 2023-09-21 오전 4.03.36.png]]
>SR Flip-Flop 과 비슷.
> - 다른 점은 **Input 이 1, 1 일때 Output 이 Complement** 인 점.
- Input
	- J
	- K
- Output (상태)
	- Q
	- Q' (Complement)

## T Flip-Flop
![[스크린샷 2023-09-21 오전 4.07.29.png]]
> 하나의 Input.
> Toggle Switch
- Input
	- T (Transition, Toggle)
		- Input 이 0 이면 그대로 반환.
		- **Input이 1 이면 해당 상태를 역전(Toggle) 하여 반환. (Complement)**
- Output (상태)
	- Q
	- Q' (Complement)

## Edge-triggered Flip-Flop (매우 중요)
![[스크린샷 2023-09-21 오전 4.18.58.png]]
- 상단 그래프 : Positive Edge
- 하단 그래프 : Negative Edge
- **Clock Pulse 에 의해 동작**
	- **Clock Pulse 가 상승/하강 할 때에 D Input (J, K, ..) 에 의해 동작하여 Output 반환**


### Positive Edge (Rising, 상승 엣지)
> - **Clock 의 값이 1이 되어 상승하는 구간에서만 Output이 변화**한다. (Positive Clock, Edge)
> - **그 외의 경우 (Clock의 값이 변하지 않거나 하강하는 구간) 에는 Output이 변화하지 않는다.** -> No Change
#### Example of Positive Edge-Triggered Flip-Flop
![[스크린샷 2023-09-21 오전 4.26.17.png]]
- **Positive Edge Triggered JK Flip-Flop**
- Q는 1로 초기화 후 실행.

### Negative Edge (Falling, 하강 엣지)
> - **Clock 의 값이 0이 되어 하강하는 구간에서만 Output이 변화**한다. (Positive Clock, Edge)
> - **그 외의 경우 (Clock의 값이 변하지 않거나 하강하는 구간) 에는 Output이 변화하지 않는다.** -> No Change
#### Example of Negative Edge-Triggered Flip-Flop
![[스크린샷 2023-09-21 오전 4.28.23.png]]
- **Negative Edge Triggered JK Flip-Flop**
- Q는 1로 초기화 후 실행.

## Excitation Table (기저표)
![[스크린샷 2023-09-21 오전 4.57.42.png]]
> 1. 현 상태 및 다음 상태를 진리표와 같이 가정.
> 2. 해당 상태를 만족하는 Input D 를 작성.
### Excitation Table of D Flip-Flop 
> **D Flip Flop 은 현 상태와 상관없음.**
> -> D input 이 1 이면 1, 0 이면 0이 다음 상태가 된다.
![[스크린샷 2023-09-21 오전 4.36.48.png]]

### Excitation Table of JK Flip-Flop 
![[스크린샷 2023-09-21 오전 4.55.30.png]]
- **현 상태가 0 일때 다음 상태( Q(t+1) )도 0으로 만들어주는 JK Input**
	1. No Change (0, 0)
	2. Reset (0, 1)
	3. 위 두 경우는 **결국 (0, x)** 이다.
		- x = 무관 조건
- **현 상태가 0 일때 다음 상태( Q(t+1) )를 1으로 만들어주는 JK Input**
	1. Set (1, 0)
	2. Complement (1, 1)
	3. 위 두 경우는 **결국 (1, x)** 이다.
		- x = 무관 조건
- **현 상태가 1 일때 다음 상태( Q(t+1) )를 0으로 만들어주는 JK Input**
	1. Reset (0, 1)
	2. Complement (1, 1)
	3. 위 두 경우는 **결국 (x, 1)** 이다.
		- x = 무관 조건
- **현 상태가 1 일때 다음 상태( Q(t+1) )도 1으로 만들어주는 JK Input**
	1. No Change (0, 0)
	2. Set (1, 0)
	3. 위 두 경우는 **결국 (x, 0)** 이다.
		- x = 무관 조건

### Excitation Table of T Flip-Flop 
![[스크린샷 2023-09-21 오전 4.57.04.png]]
