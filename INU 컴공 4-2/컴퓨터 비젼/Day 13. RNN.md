---
Author: CarefreeLife98
Date: 2023-12-06T18:11:00
Agenda: 
tags:
  - Computer_Vision
---
# Vanilla Neural Net
> **Problem**
> - Sequence Data type 생겨남 (String?)
> - input 의 길이에 따라 영향 (?)
> - Parameter 를 공유하지 않는다.
> <br>
> **위와 같은 문제가 있어 Vanilla 대신 RNN 을 사용한다.**

# RNN
> **Parameter Sharing**

## Dynamic System
> **s: Status**
> - 상태를 나타냄
> <br>
> **f:Function**
> - 이전의 상태를 현재 상태로 Mapping
> - Recurrence (되풀이)
> <br>
> **수식 잘 보기. 일반적인 RNN Modeling**

## RNN 표현 방식
> 1. Loop (Recurrent Graph) : Left
> 2. Unfolded Graph : Right

## Bidirectional RNN
> **미래로부터 지금을 예측하는 느낌**
> - 뒤의 단어를 보고 앞의 단어를 예측하는 느낌?
> - g 라는 Feature 를 만들어서 수행.
> - 역방향으로 수행
> <br>
> 
> - h: 과거에 대한 Summary
> - g: 미래에 대한 Summary

# RNN - 
![[스크린샷 2023-12-06 오후 6.37.49.png]]
> loss (Error) 정의
> - 매 Time Step 마다 계산.

## BPTT Algorithm
> Forward : Loss 를 계산.
> Backpropagation: 거꾸로 돌아가며 Parameter 들을 Update.
> 수식이 복잡하니 필요하면 찾아볼 것




# RNN Part 2
> **RNN 은 Dependency 를 학습 할 수 있다.**
> <br>
> **example(pdf)**
> - 빈 칸에 도달하기 전까지의 Sequence 가 짧으면 예측이 쉽지만, 길다면 필요없는 정보가 더 많이 섞여있을 확률이 크므로 예측이 어려워진다.
> <br>
> **Gradient Vanishing 문제가 주로 발생.**
> - 문제 찾기가 어려움.
> 	- Vanishing 하는 정도의 기준 설정이 어려움.
> 	- 어느 정도 Vanishing 되어야 vanishing 되었다고 판단할 것 인가?
> - 따라서 Heuristic 하게 해결해야함.
> 	- 이 문제를 해결하기 위해 RNN 을 뜯어고침 (Gated RNNs).

## Gated RNNs
> 기존 RNN 은 매 타입 스텝마다 사용하는 Parameter 가 동일.
> - **Gated RNNs 에서는 매 타임스텝마다 다른 Parameter 를 사용.**

## LSTM
> ![[스크린샷 2023-12-06 오후 6.51.47.png]]

## LSTM networks
> **오랫동안 정보를 잘 담아둘수 있도록 하는 경로가 존재.**
> <br>
> **기존 RNN 은 동일한 Module 의 반복.**
> ![[스크린샷 2023-12-06 오후 6.53.19.png]]
> - Single Tanh layer
> - 단순한 구조.
> <br>
> **LSTM : 4개의 Interactive 한 mudule 로서 구성됨.**
> ![[스크린샷 2023-12-06 오후 6.54.30.png]]
> - RNN 보다 조금 더 복잡한 구조.
> - LSTM Cell 로 구성됨.

## LSTM Cell
> **각 Cell 내부에서 발생하는 Recurrence 존재.**
> <br>
> **RNN 과 동일한 I/O 를 가지고 있지만 더 많은 Parameter 와 Gate 를 가지고 있음.**
> - Gate 매커니즘을 통해 정보의 양을 조절 할 수 있음. (문 처럼)
> <br>
> Cell State c 존재. (Vector)
> - 컨베이어 벨트위의 제품같은 느낌.
> <br>
> **3개의 Gate 로 이루어짐.**

## Notation and Conventions
> **element-wise operation**
> - 각 열끼리 의 원소 간 연산
> - a = \[a1, a2]
> - b = \[b1, b2]
> - a (wise)* b = \[a1 * b1, a2 * b2]
> <br>
> **Simplified Notation**
> - **hidden-hidden 과 input-hidden 을 합친다.**
> -  ...
> <br>
> **Cell state 는 매 Time step 마다 수정, 변경된다.**
> - 무엇에 의해 조절되는가?
> 	- gate 에 의해서.
> <br>
> Gate 의 역할
> - 정보의 양을 조절하는 역할
> - 구성
> 	- Sigmoid Neural net
> 	- element wise multiplication
> - sigmoid 값의 의미 (0,1)
> 	- 0: 모든 문을 닫음.
> 	- 1: 모든 문을 Open
> <br>
> **Gate 종류 및 순서**
> 1. forget
> 2. input
> 3. output

## Forget Gate
> 얼만큼 정보를 버려야하는지 계산.
> - 이전의 cell state 를 얼마나 잊어야 하는지
> 	- 0인 경우 삭제
> 	- 1인 경우 유지

## Input gate
> - Candidate Value: c 틸다
> - tanh 로 Activate.
> - 최종적으로 cell state 를 새로운 값으로 Update.
> <br>
> 선택적 기억, 선택적 입력

## Output Gate
> **O(t) 에다 tanh 를 적용 시킨 후 wise 연산.**

## 비교 (vs RNN)
![[스크린샷 2023-12-06 오후 7.19.04.png]]
> **결론: LSTM 은 RNN 에 비해 Gradient Vanishing 문제가 줄어든다.**

## GRU
> LSTM 에 비해 적은 Paramter.
> LSTM 의 Gate 이름을 바꿈 (update, reset ...)