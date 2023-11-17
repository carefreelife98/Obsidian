---
Author: CarefreeLife98
Date: 2023-11-17T14:36:00
Agenda: 
tags:
  - Computer_Vision
---
# Neuron
> 생물을 이루는 가장 작은 단위.
> - 이에 아이디어를 얻어 인공 신경망을 개발.

# Artificial neural networks (ANNs)
![[스크린샷 2023-11-17 오후 2.58.29.png]]
> **Multilayer perceptron : misnomer**
> - network of multiple logistic models (continous nonlinearity)
> - Perceptron은 불연속성이지만 misnomer 은 연속적인 특성을 가짐.
> 
> <br>
> 메인 아이디어
> - 추출된 input 에 대한 선형 조합에서 정보를 추출.
> - 그것에 대하여 Activation Function 을 취함 (?)

# Models of a Neuron
> 1. **Synapses**
> 	- weights 로서 구현
> 2. **Adder**
> 	- sumation.
> 	- input vector -> scalar
> 3. **Activation function (Possibly Nonlinear)**

D

## Activation function
![[스크린샷 2023-11-17 오후 3.26.14.png]]

### Sigmoid function

# ReLU (중요)
> **가장 많이 쓰이는 Linear Unit.**
> - **장점**
> 	- **back-propagation 수행 성능 좋음.**
> 	- 추후 **Vanishing gradient 문제 해결** 가능
> 		- Vanishing gradient: Layer 가 점점 깊어지면서 gradient 가 흐릿해짐.
> - **한계**
> 	- 프로그램이 죽을 수 있음.
> 		- Leaky ReLU 를 사용하여 극복 가능.

<br><br>
# Multilayer Perceptron
## Single-layer feedforward network
![[스크린샷 2023-11-17 오후 4.08.28.png]]
> 파란색 박스 : Input <br>
> 회색 원 : neuron (= perceptron)
> - **둘 사이에 연결이 존재한다 == 둘 사이에 weight 가 존재한다.**
> <br>
> **각 입력과 Perceptron 사이의 가중치(weight) 를 Matrix 형태로서 나타낼 수 있음.**
> - Input layer (입력 Layer)
> - Output layer (Perceptron Layer)
> <br>
> **Forwarded**
> - 모든 Signal 은입력에서 Output(Perceptron) 방향으로만 진행.
> - 반대 방향으로는 불가.

<br><br>
## Multilayer feedforward network (Multilayer Perceptron)
![[스크린샷 2023-11-17 오후 4.23.27.png]]
> **탄생 배경**
> - XNOR 과 같은 분포는 선형 모델로서 정확한 클래스 분류가 불가능 했음.
> - 이에 **Layer (Perceptron) 를 여러 개 쌓아 올림으로서 Multilayer 를 만들어 Nonlinear 문제를 해결.**

![[스크린샷 2023-11-17 오후 4.25.59.png]]
> **Input layer 와 Output layer 사이에 여러 개의 hidden layer 가 존재함.**
> - **각 Layer 의 Output 이 다음 Layer 의 Input 이 되어 Foward 방향으로 진행.**

<br><br>

## Types of signals in neural networks
![[스크린샷 2023-11-17 오후 4.34.20.png]]
### 1. Function Signal
> 이전에 배운것.
> - **Input 으로 Input signal 이 들어와 Output 으로 Output signal 이 나가는 것. (Forward)**

### 2. Error Signal
> **우리가 원하는 값과 실제 결과 값을 비교하여 Error signal 을 찾아낼 수 있음.**

<br><br>

## Softmax Function
![[스크린샷 2023-11-17 오후 4.35.07.png]]
> **10개 클래스가 도출되어 10차원의 Vector 로서 데이터가 생성된 경우, 각 데이터를 표현하기 위해 확률 분포로서 표현해주는 Function.**
> - **각 Data 에 시그마 함수를 씌워 확률 분포로서 표현.**
> 	- 이 경우 모든 Data 를 더하면 1이 된다. (확률 분포로서 표현된 데이터 이므로)

<br><br>
### Softmax function 예시
![[스크린샷 2023-11-17 오후 4.40.33.png]]
> **기존 Data 에 Softmax 를 씌우더라도 Data 간의 크기 순서는 변하지 않는다.**
> - 각 Data 의 값을 0 ~ 1 사이의 확률적 값으로 변환 시켜주는 것.
> <br>
> **Softmax Function 후 도출된 Data 에 대한 일반적인 해석**
> - 특정 Class 의 확률 값이 높다면, Input Data(이미지)는 해당 Class 일 확률이 높다.

<br><br>

## Function of Hidden Neurons
![[스크린샷 2023-11-17 오후 4.45.33.png]]
> **이렇게 추출한 Data 들은 그 자체로서 Data 를 표현하지는 못하지만 해당 Data 들은 실제 Data 들이 녹아들며 만들어진 것이므로 해당 Data 들의 군집을 통해 Image(Data) 의 특징을 뽑아 낼 수 있다.**