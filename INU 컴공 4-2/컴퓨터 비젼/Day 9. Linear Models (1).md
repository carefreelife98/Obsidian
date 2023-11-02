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
### 예시
