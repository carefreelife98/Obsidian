---
Author: CarefreeLife98
Date: 2023-11-22T19:22:00
Agenda: 
tags:
  - Computer_Vision
---
# Convolutional Neural Networks
> MLP : 입력 Data 의 순서에 구애받지 않음.
> CNNs : 입력 Data 의 순서가 다르면 결과에 영향.
> <br>
> 

## CNN: Motivation
> **CNN 의 filter kernel 은 Locally 하게 존재.**
> - 전체 영역을 Cover 하지 않음.
> - 각 filter kernel 이 담당하고 있는 Local 영역을 **Receptive field** 라고 한다.

## CNN: Neurons
> CNN 의 각 neuron 은 작은 영역에 연결되어 있음.
> - CNN 은 layer 구조.
> - 각 layer 는 Volume transformer 의 역할을 함.

## Building a CNN
> **Layer 의 Type 4가지**
> 1. Convolution Layer
> 2. ReLU Layer
> 3. Pooling Layer
> 4. Fully connected Layer
> <br>
> ![[스크린샷 2023-11-22 오후 7.42.02.png]]
> **Parameter = 가중치 (weight)**
> - **학습이 되는 Parameter** 이다. (learnable parameter)
> <br>
> **Hyperparameters**
> - 학습이 되는 Parameter 가 아님.
> - **설정**이 필요함.
> 	- ex. hidden layer 에 node 를 몇개 생성 할 것인가?

## CNN - Layer: 1. Convolutional Layer
> **여러가지 목적에 따라 각 Filter 들은 서로 다르게 학습이 된다.**
> <br>
> **각 layer 의 구성요소**
> - MLP : Perceptron
> - **CONV Layer : Filter**
> <br>
> **Input 이 다음 Layer 의 Filter 에 적용되는 과정**
> ![[스크린샷 2023-11-22 오후 7.55.21.png]]
> ![[스크린샷 2023-11-22 오후 7.55.42.png]]
> - **Filter 가 Input image 에 대하여 Slide 하며 각 slide 마다 새로운 값을 만들어냄.**
> 
> <br>
> ![[스크린샷 2023-11-22 오후 7.58.27.png]]
> - **새로운 값이 activation map 을 구성**.
> 
> <br>
> ![[스크린샷 2023-11-22 오후 7.59.30.png]]
> - **Filter 의 개수만큼 생성된 activation map 들이 모여 새로운 차원의 image 를 만들어냄.**
> 
> <br>
> **제약 조건 (Hyper-parameters)**
> - **depth**
> 	- (32 x 32 x 3) image <-> (5 x 5 x 3) kernel 과 같이 depth 가 같아야 함.
> - **stride**
> 	- Filter 를 sliding 할 때 움직일 pixel 의 수.
> - **zero-padding**
> 	- 
> **위 제약 조건은 Conventional 한 기준이 존재한다.**
> <br>
> **Sptial size 를 유지하고 싶은 경우**
> - 

# QnA
> 입력 Data(image) 의 depth 와 다음 CONV layer 에 존재하는 filter 의 depth 가 같아야함.
> - 입력 data 가 이미지인 경우, 32 x 32 x 3 에서 depth 3 은 rgb ?
> - 그렇다면 첫번째 CONV 이후 6 depth