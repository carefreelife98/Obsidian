---
Author: CarefreeLife98
Date: 2023-10-19T04:42:00
Agenda:
  - 1. Machine Learning
  - 2. Classification
tags:
  - Computer_Vision
---
# Machine Learning Framework: Steps
## Training
> **학습시키고자 하는 Data 를 이용해서 목표 Model을 학습시킨다.**
> - **Supervised Learning**
> 	- **Data 에 대한 Class, Label (y) 가 존재한다.**
> <br>
> **Data 를 통해 학습시킨다?**
> 1. Image Feature 를 통해 **Image의 특징을 추출**해낸다. (이미지 자체를 학습 시킬 수 없음)
> 	- Ex) \[width, height] 를 통해 2차원 Vector를 생성한다.
> 	- \[width, height, R, G, B] 를 통해 5차원 Vector를 생성한다.
> 2. 해당 Image Feature 에 Label 을 추가하여 Data 를 생성한다.
> 3. 생성된 Data 를 Model 에 학습시킨다.
> 	- 𝜃