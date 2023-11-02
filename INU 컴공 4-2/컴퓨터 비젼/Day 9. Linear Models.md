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
## Agenda of Perceptron
> **Linear Model 의 가장 기본적인 형태.**
> - 인공 신경망, AI 등의 기본이 되는 Unit 이 Perceptron.
> - 선형 분류기
> 	- 선으로서 분리되지 못하는 Data Set 의 경우가 존재하면 학습이 불가능함.
> <br>
> **어떠한 Input 에 대해 판단을 내리기 위해서는 특정 기준 값이 필요함.**
> - **Input Data 의 속성 값이 숫자로서 표현되고 가중치와 같은 속성이 존재하게 되면 특정 기준 값과 비교하여 True / False 로서 판단이 가능하게 됨.**

<br><br>
![[스크린샷 2023-11-03 오전 3.00.17.png]]
> **학습이 되어야 할 Parameter 값에서 threshold (기준 값) 를 Subtract 한 결과의 부호로서 판단.**<br>
> **sign 함수**
> - y = sign(a)
> 	- **a 가 음수인 경우 y = -1**
> 	- **a 가 양수인 경우 y = 1**

<br><br>
## Two-dimensional Case - 결정 경계 (Decision Boundary)
![[스크린샷 2023-11-03 오전 3.04.42.png]]
> **결정 결계 (Decision Boundary)**
> - 2차원 에서 정의되는 y = ax + b 라는 기준 선.
> <br>
> **Learning Algorithm 이 하는 역할은 Parameter(weights) 의 설정**
> - Data Set 에 맞춰 적절히 작동하도록 결정 경계선의 기울기(parameter. {a, b})를 찾아가는 Algorithm.

<br><br>
![[스크린샷 2023-11-03 오전 3.12.47.png]]
> **Learning Algorithm 에 의해 찾은 이상적인 Parameter 는 Hypothesis(g) 가 된다.**
> - g 는 Opimal Choice 가 된다. (가장 이상적인 결정 경계 직선)

<br><br>

## Perceptron Learning Algorithm (PLA)
![[스크린샷 2023-11-03 오전 3.29.15.png]]
> **Hypothesis(g) 를 생성하기 위하여 가지고 있는 Data Set 에 대해 가장 이상적인 결정 경계를 생성 할 수 있는 Parameter (w) 를 결정하는 알고리즘.**

<br><br>
![[스크린샷 2023-11-03 오전 3.44.26.png]]
> 위처럼 주어진 Perceptron 의 weight 를 **sign 함수의 값에 따라 업데이트 해가며 이상적인 weight 를 찾아간다. (Iteration 을 통해서)** 

<br><br>
# Linear Classification
> Classification 이란 Category 를 분류하는 것.

## Linear Model for Binary Classification
> 가장 쉬운 Classification.
> - **Binary, 즉 0과 1 (-1 or 1) 두개의 Category 로 분류.**
> - Perceptron 을 활용.
> - **Vector 로서 존재하는 Data 를 특정 공간 d 에 Mapping 시켜 생성되는 Output y 를 h(x) = sign(w^T x) 을 활용하여 분류.**
> - 