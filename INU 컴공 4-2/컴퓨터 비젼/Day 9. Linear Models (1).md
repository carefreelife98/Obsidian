---
Author: CarefreeLife98
Date: 2023-11-03T00:46:00
Agenda:
  - 1. Learning Problem
  - 2. Linear Classification
  - 3. Linear Regression
tags:
  - Computer_Vision
---
# Learning Problem
## Learning from Data
> **인간의 학습**<br>
> 나무를 예로 들자면, 인간은 나무를 특별히 학습하지 않더라도 살아가는 과정에서 나무를 보고 접하며 학습 하게 될 수 있다.<br>
> **즉, Data 를 통해 학습 할 수 있다.**<br>
> 이처럼, 특정 학습을 위해 어떠한 **수학적인 Function을 구축하는 것은 쉽지 않다.**<br>
> **대신 이 과정을 여러 Historical Data 를 통해 학습 시킬 수 있다.**

<br><br>

## Problem Setup
> Learning Problem 을어떻게 Setup 하는가?
### Credit Approval
> 각 Data 의 확실한 특징을 설정.

|**Component**|**Symbol**|**Credit Approval Metaphor**|
|---|---|---|
|**Input**|**x(Vector)**|customer application|
|**Output**|y|approve or deny|
|**Target Function**|f:**x** -> y|ideal credit approval formula|
|**Data**|(**x**1, y1) , ... , (**x**n, yn)|historical records|
|**Hypothesis**|g: **x** -> y|formula to be used|

> **Input**
> - **x** : Vector
> <br>
> **Output**
> - y : Binary 값.
> - 승인 여부
> <br>
> **Target Function**
> - 우리가 정답이라고 생각하는 것. (Oracle)
> - 실제 정답에 해당되는 것들의 분포.
> <br>
> **Data**
> - 각각의 Data
> <br>
> **Hypothesis**
> - g
> - 우리가 맞추어야 하는 F (Target Function) 를 근사, 모사하도록 학습이된다.
- **우리는 Target Function 에 해당되는 정답이란 것을 수학적으로 정의하기는 어려움.**
- 해당 **Target Function 을 대신 할 수 있는 Hypothesis (g) 를 생성.**
	- g 에 대한 Input(**x**) 을 통해 얻은 Output(y) 의 **정확도를 높이기 위한 Parameter 를 설정**하게 되는 것.

<br><br>
### Credit Approval 예시
![[스크린샷 2023-11-03 오전 2.06.33.png]]
- 이성적인 판단 기준 (정답) 은 F 이나, 이를 **수학적으로 정의하기 어려움.**
- 따라서 **F에 근접한 Hypothesis (g) 를 가장 기본적인 구조인 Linear (Model) 로서 표현.**

<br><br>
### Learning Algorithm (Model)
![[스크린샷 2023-11-03 오전 2.10.59.png]]
> **Learning Algorithm (Model) 생성이란?**
> - **Hypothesis (g)를 생성하는 것.**
> - Data 를 이용하여 f 에 근접한 g 라는 Folmula 를 생성.
> <br>
> **여러 Hypothesis 가 모여있는 그룹을 Hypothesis Set (**H**) 라고 함.**
> - **H** 에서 Best **Hypothesis (g) 를 선정.**

<br><br>
### Basic Setup
![[스크린샷 2023-11-03 오전 2.14.22.png]]
- 이전까지 설명한 내용을 표로서 위와 같이 표현 할 수 있다.

<br><br>

## Learning Model
> **Learning Problem 의 세 가지를 정의.**
> - **Target Function (f)**
> - **Training Examples (Data)**
> - **Learning Algorithm(A), Hypothesis Set (H)**
> 	- 사용자가 정하는 것. 
> 	- 어떤 Model 을 사용 할 것인지.

<br><br>
## Hypothesis Set
![[스크린샷 2023-11-03 오전 2.48.37.png]]
> **Hypothesis Set H 는 h(x) 로서 구체화됨.**
> - 또한 h(x) 는 항상 H 에 포함됨.
> <br>
> **h(x)**
> - **x 로부터 각각의 다른 weights(= Parameter) 를 받게 됨.**
> 	- 예를 들어 **y = ax + b** 인 경우 **Parameter 는 {a, b}**
> 	- a 와 b 의 값에 따라 **선의 기울기가 달라진다.**

<br><br>
# Perceptron
> **Linear Model 의 가장 기본적인 형태.**
> - 인공 신경망, AI 등의 기본이 되는 Unit 이 Perceptron.
> - 선형 분류기