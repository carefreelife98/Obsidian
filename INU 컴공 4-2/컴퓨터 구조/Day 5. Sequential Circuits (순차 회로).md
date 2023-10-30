# Sequential Circuit's Excitation Table
![[스크린샷 2023-09-26 오후 4.39.29.png]]

<br><br>
# Sequential Circuit Analysis
![[스크린샷 2023-09-26 오후 4.44.18.png]]
- **Circuit 의 Output 이 Input 뿐만 아니라 이전 상태에 따라 결정된다.**
	- **순서의 개념 추가됨**
	- 동기화된. Clocked. Synchronized. 동기 순차 회로라고도 함.
- **분석 순서 (Circuit 의 동작 과정 설명)**
	1. **Boolean Function (Flip Flop 의 Input 과 Output) 찾기**
	2. **State Table** 찾기
	3. **State Diagram** 그리기

## Analysis of Sequential Circuit
### 1. Boolean Function 찾기
#### Flip-Flop 의 D input 에 대한 Boolean Function 찾기
![[스크린샷 2023-09-26 오후 4.49.02.png]]
- input : x
- State : (A, A'), (B, B') 
- Output : y

- **D input(Flip-Flop) Equation**
	- `D_A = Ax + Bx`
	- `D_B = A'x`
- **Combinational Circuit Output Equation**
	- `y = Ax' + Bx'`

### 2. State Table (상태표) 찾기
#### State Table 구성 요소
**Input**
- Present State (A, B)
- Input (x)

**Output?**
- Next State (A, B)
- Output (y)

**Flip-Flop Inputs**
- D_A, D_B

#### State Table 찾기
![[스크린샷 2023-09-26 오후 5.00.04.png]]
- **D F-F 는 현재 상태와 상관없이 다음 상태에 따라 Output 이 결정**된다.
	- 따라서 **D_A, D_B 를 그대로 Next States A, B 에 가져온다.**
	- D FF 에서는 FF-Inputs 를 찾을 필요 없이 Next States 까지만 찾아도 된다. (Next States == FF-Inputs 이므로)

### 3. State Diagram 그리기
> 각각의 상태가 Input에 의해 다음 상태가 어떻게 되는지 확인하는 것.
> State Table 을 기반으로 그릴 수 있다.

![[스크린샷 2023-09-26 오후 5.09.09.png]]

```
(입력)/(출력) : " / " 앞 부분은 입력, 뒷 부분은 출력을 나타냄.

(현재 상태) -> (다음 상태) : 화살표의 출발점은 현재 상태, 도착점은 다음 상태를 나타냄.
```
## 예시 1
### Boolean Function 찾기
#### Sequential Circuit
> Input: x
> State : A, B
> FF : JK
> JK Input: J_A, K_A, J_B, K_B

#### Boolean Function
> J_A = B, K_A = Bx'
> J_B = x', K_B = A XOR x

![[스크린샷 2023-09-26 오후 5.15.33.png]]

### State Table 찾기
![[스크린샷 2023-09-26 오후 5.28.58.png]]


| Pr.St | Input | Nx.St | FF-Input        |
| ----- | ----- | ----- | --------------- |
| A, B  | x     | A, B  | J_A,K_A,J_B,K_B |
| ----- | ----- | ----- | --------------- |
| 0 0   | 0     | 0 1   | 0 0 1 0         |
| 0 0   | 1     | 0 0   | 0 0 0 1         |
| 0 1   | 0     | 1 1   | 1 1 1 0         |
| 0 1   | 1     | 1 0   | 1 0 0 1         |
| ----- | ----- | ----- | --------------- |
| 1 0   | 0     | 1 1   | 0 0 1 1         |
| 1 0   | 1     | 1 0   | 0 0 0 0         |
| 1 1   | 0     | 0 0   | 1 1 1 1         |
| 1 1   | 1     | 1 1   | 1 0 0 0         |


### State Diagram 그리기
![[스크린샷 2023-09-26 오후 5.30.53.png]]


# Sequential Circuit Design (설계)
> 회로의 동작, 기능 등이 주어지면 해당 Circuit을 그리는 것.
> 	- Analysis 와는 반대의 과정.

## Binary Counter
> input 이 1 일 때, `00 -> 01 -> 10 -> 11` 순으로 상태가 변함. (Counter - 반복)
> input 이 0 일 때, 상태는 변하지 않음.

### 1. State Diagram 그리기
![[스크린샷 2023-09-26 오후 5.36.24.png]]


### 2. State Table 찾기
> FF : JK Flip-Flop
> Input : x
> Excitation Table (기저 테이블, 여기표) 사용.

![[스크린샷 2023-09-26 오후 5.46.17.png]]

| Present States / Input | Next States (P.S와 Input에 의해 결정됨) | FF - Imputs |
| ---------------------- | --------------------------------------- | ----------- |
| A B             x      | A B                                     | JA,KA,JB,KB |
| 0 0 0                    | 0 0                                    | 0 x 0 x           |
| 0 0 1                    | 0 1                                     |    0 x 1 x        |
| 0 1 0                    | 0 1                                  |       0 x x 0     |
| 0 1 1                    | 1 0                                |1 x    x 1         |
| -----------------      | ---------------------------------------                             | ----------- |
| 1 0 0                    | 1 0                                   |   x 0 0 x         |
| 1 0 1                    | 1 1                                  |      x 0  1 x     |
| 1 1 0                    | 1 1                                   |x 0    x 0         |
| 1 1 1                    | 0 0                           |   x 1       x 1   |


###  JK Flip-Flop 에 대한 Boolean Function 찾기 (K-map)
![[스크린샷 2023-09-27 오전 12.57.22.png]]

### Sequential Circuit (순차 회로) 그리기
![[스크린샷 2023-09-27 오전 1.00.07.png]]

## 3-Bit Binary Counter
> 각 비트의 개수 만큼 Flip-Flop 이 필요함. (본 예제에서는 T F-F 사용)
> 0 ~ 7 범위를 순환하는 Counter.

### 1. State Table 찾기
![[스크린샷 2023-09-27 오전 1.06.50.png]]

### 2. T Flip-Flop Input 에 대한 Boolean Fumction 찾기
![[스크린샷 2023-09-27 오전 1.09.15.png]]

### 3. 3-Bit Counter Circuit 그리기
![[스크린샷 2023-09-27 오전 1.12.27.png]]

### 4. 3-Bit Counter Circuit 분석
![[스크린샷 2023-09-27 오전 1.18.46.png]]
- A0
	- Always Complement
- A1
	- A0 가 1일때 Complement
		- Clock pulse 가 상승하는 경우에만 Toggle.
		- Clock Pulse 가 상승 할 때 값이 1 or 0 으로 전환된다.
- A2
	- A0 와 A1 이 모두 1인 경우에만 1.
	- AND Gate 가 존재 하므로.

# In-class Ex


## Q1. Analysis


> **현재 상태 : 아래에서 올라온 Carry**

1. Full Adder Logic 찾기
2. Full Adder Logic 을 바탕으로 State Table 작성
3. State Diagram 작성

결과


## Q2. Design

1. State Diagram 작성
2. State Table 작성
3. JK Flip-Flop Excitation Table 을 바탕으로 Input Equation 찾기
4. Circuit 그리기
