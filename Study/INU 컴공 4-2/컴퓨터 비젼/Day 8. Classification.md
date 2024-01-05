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
## 1. Training
![[스크린샷 2023-10-19 오전 5.13.24.png]]
> **학습시키고자 하는 Data 를 이용해서 목표 Model을 학습시킨다.**
> - **Supervised Learning**
> 	- **Data 에 대한 Class, Label (y) 가 존재한다.**
> <br>
> **Data 를 통해 학습시킨다?**
> 1. Image Feature 를 통해 **Image의 특징을 추출**해낸다. (이미지 자체를 학습 시킬 수 없음)
> 	- Ex) \[width, height] 를 통해 2차원 Vector를 생성한다.
> 	- \[width, height, R, G, B] 를 통해 5차원 Vector를 생성한다.
> 2. 해당 Image Feature 에 **Label 을 추가하여 Data 를 생성**한다.
> 3. **생성된 Data 를 Model 에 학습시킨다.**
> 	- 이는 **𝜃 를 결정**하게 되는 과정
> 	- **선형 분류기**
> 		- **데이터를 분류할 때 선을 이용하여 분류 (Linear Classifier)**
> 			- y = ax +b
> 		- 즉, **학습된 Model 의 Parameter** 라는 것은 이 **Model 을 결정하는 a와 b (= 𝜃) 같은 Parameter 이고, 이를 결정해가는 것.**
> 		- Model 은 **한번 학습 시 𝜃 를 생성**하고, 이를 **반복적으로 학습 시 𝜃_1, 𝜃_2 ... 𝜃^ (𝜃*) 이 되어 최적의 𝜃 가 된다.**
> 		- **결국 𝜃^ 또는 𝜃* 가 될 때 까지 보유한 Data 를 통해 반복적으로 학습시키는 것.**

<br><br>
## Testing
![[스크린샷 2023-10-19 오전 5.34.43.png]]
![[스크린샷 2023-10-19 오전 5.35.14.png]]
> **Training 과정에서 존재하지 않았던 Data 를 Model 에 입력.**
> 1. 새로운 Data 에 대한 **예측 (Prediction / Inference) 을 통해 추어진 Data 의 Label 이 무엇인지 추론.**
> 2. 추론의 결과로 **새로운 Image Feature 를 추출.**
> 3. **해당 Image Feature 을 학습된 Model 에 입력.**
> 	- 이전에 찾은 **𝜃^ 또는 𝜃* 이 존재하는 공간에 새로운 Test Data 를 Mapping.**
> 		- 𝜃^ / 𝜃* 라는 선을 기준으로 새로운 Data 의 Label 이 분류된다.
> 	- 그 결과로 **새로운 Data 의 Label 이 결정되며 Model이 학습**하게 된다. 

<br><br>
# Machine Learning Framework - Training : 1. Image Features
![[스크린샷 2023-10-19 오전 5.47.21.png]]
> **Image (Data) 의 특징, 즉 Image Feature 를 추출하는 방법.**
> <br>
> **입력의 차이**
> 1. **Raw Pixels**
> 	- Deep Learning 에서 주로 사용
> 2. **Histogram**
> 	- 고전적인 특징 추출 (Edge, Corner Detection ...)
> 	- Gradient 의 방향이나 Intensity의 Histogram 등을 Vector 로 바꾸어 활용
> 1. **Templates**
> 	- 특정 Template 에 대한 Response 를 활용
> 2. **Local Descriptors**
> 	- 고전적인 특징 추출 (Edge, Corner Detection ...)
> <br>
> **결국 "좋은 특징" 을 찾는 것이 중요하다.**

<br><br>
## Spectrum of Supervision
### Supervision 의 강도에 따른 분류
![[스크린샷 2023-10-19 오전 6.10.27.png]]

> **Fully Supervised**
> - Label 의 Spectrum 을 보았을 때, 그 정도(수) 가 많은 경우
> - 즉, **주어진 이미지에 대해서 모든 Label을 알고 있는 상황.**
> <br>
> **Unsupervised**
> - 학습하는 Data 에 대해 **Label 이 아예 존재하지 않는 경우**
> - **각 이미지의 주요하거나 빈번하게 나타나는 특징을 기반으로 Model 이 학습하기를 바라는 방식.**
> <br>
> **Weakly Supervised**
> - **Label 이 존재하기는 하나, 그 강도가 낮은 경우.**
> 	- Pixel-Level Label (Strong)
> 		- Pixel 단위로 Label 이 존재.
> 	- **Image-Level Label (Weak)**
> 		- **Image 단위로 Label 이 존재.**
> 		- Image 는 분류 가능하지만 해당 Image 의 위치 (B/F Ground or Geological location) 를 알지 못하는 경우

<br><br>
# Machine Learning Framework - Training : 2. Training
![[스크린샷 2023-10-19 오전 6.40.58.png]]
> **Model 이 학습한다는 것은?**
> - **Prediction Function (f) 의 Parameter 에 대한 학습.**
> 	- x: Image (Image Feature)
> 	- f: Prediction Function
> 	- y: Output (Label) 

<br><br>

![[스크린샷 2023-10-19 오전 6.32.53.png]]
> <br>
> 특정한 입력을 함수에 넣었을 때, 그것이 y 로 가는 과정.
> - 즉 **Input -> Output 으로 가는 과정 (Mapping) 을 결정해주는 것.**
> - **Decision Boundary (𝜃)** 를 학습하게 됨.
> 	- 새로운 Image 의 Feature 를 추출.
> 	- Decision Boundary(𝜃) 가 존재하는 공간에 Mapping.
> 	- 해당 결과에 의해 적절한 Label 의 영역에 배치 및 Label 할당.

<br><br>
# Generalization
> **학습된 모델의 일반화 과정.**

## Bias-Variance Trade-off
![[스크린샷 2023-10-19 오후 1.54.49.png]]
> **Bias : 편향**
> - **학습된 Model 의 평균적인 예상 수행 결과와 실제 수행 결과의 밀집 위치 차이**
> 	- **Low Bias**
> 		- 예상 수행 결과와 실제 수행 결과가 비슷하게 발생
> 	- **High Bias**
> 		- 예상 수행 결과와 실제 수행 결과의 차이 발생
> <br>
> **Variance : 편차**
> - **학습된 Model 의 수행 결과 밀집도**
> 	- **High Variance**
> 		- 밀집도가 낮음
> 	- **Low Variance**
> 		- 밀집도가 높음

<br><br>
## Generalization Error Effects
> **Overfitting 및 Underfitting 에 의해 Bias 와 Variance 가 발생.**
> - Training error 와 Test error 가 발생. 

![[스크린샷 2023-10-19 오후 2.05.36.png]]
> **Underfitting**
> - 지나치게 단순한 Model 을 사용해서 Fitting 할 시 Underfitting 이 발생.
> 	- High Bias 와 Low Variance 발생.
> 	- High Training error 와 High Test error 발생
> - 쉽게 생각하면 옷을 입을 때 Loose Fit 으로 입는 것과 비슷함.


<br><br>
![[스크린샷 2023-10-19 오후 2.13.40.png]]
> **Overfitting**
> - 지나치게 복잡한 Model 을 사용해서 Data 를 Fitting 할 시 Overfitting 이 발생.
> 	- Low Bias 와 High Variance 발생.
> 	- Low Training error 와 High Test error 발생
> 		- Training 에 대한 Error 크게 감소시킴.
> 		- 파란색 선과 빨간색 점의 차이를 크게 감소시킴.
> 			- 하지만 **새로운 Test Data 가 입력 될 시, Test Error 가 급격하게 커짐.**

<br><br>
## Bias-Variance Trade-off 를 통해 설명하기
> **위의 두 가지 상황 (Underfitting, Overfitting) 을 우리는 Bias-Variance Trade-off 를 통해 설명 할 수 있다.**

![[스크린샷 2023-10-19 오후 2.29.23.png]]
> **Model 이 Simple 하다는 것은 상대적으로 Parameter 가 적은 것.**
> - y = ax + b
> 	- **너무 적은 Parameter 를 가진 Model 은 부정확 할 수 있다.**
> <br>
> **Model 이 Complex 하다는 것은 상대적으로 Parameter 가 많은 것.**
> - y = ax^6 + bx^5 + cx^4 ... fx + g
> 	- **너무 많은 Parameter 를 가진 Model 도 오히려 더욱 부정확 할 수 있다.**
> <br>
> **전체적으로 계산 할 Error**
> - **`E(MSE) = noise^2 + bias^2 + Variance`**
> 	- **MSE : Mean Squared Error**
> 		- Error 의 Squared 를 **평균화** 한 것.
> 		- Error 의 구성 요소
> 			- **Noise**
> 				- 필연적으로 발생하는 Data noise 에 의한 Error.
> 				- 줄일 수 없는 Error.
> 			- **Bias 에 의한 Error**
> 			- **Variance 에 의한 Error**

<br>
<br>
![[스크린샷 2023-10-19 오후 2.40.52.png]]
> Model 에 대한 Complexity 를 증가시켜 나가게 되면 Training Error 는 감소한다.
> 또한, Test Error 도 감소하지만 새로운 Data 에 대하여 Inferrence 하는 경우 Test Error 가 급격하게 증가한다.
> - 즉, **점차 복잡한 모델을 사용하며 Data 를 학습 시킬수록 기존에 가지고 있던 Data 에 대한 오류는 점차 감소**한다.
> - **Test Error 도 감소하지만 새로운 Data 에 대해서는 Test Error 가 급격하게 증가**한다.
> <br>
> 따라서,
> **Underfitting**
> - 너무 **Simple 한 Model** 로 Data fitting 시 **올바르게 Fitting 하지 못한 경우.**
> <br>
> **Overfitting**
> - 너무 **Complex 한 Model** 로 Data fitting 중 **새로운 데이터 학습에 의해 Test Error 가 커진 경우.**

<br>
<br>
![[스크린샷 2023-10-19 오후 2.43.28.png]]
> **Model 학습을 위한 Test Data 가 적은 경우 Model 의 일반화 성능이 떨어짐.**

<br><br>![[스크린샷 2023-11-03 오후 4.54.04.png]]
> **Data 의 수 와 Error 의 관계**
> <br>
> 1. **Data 의 수가 증가할수록 Trainig Error 는 증가**한다.
> 	- 이는 Data 가 많아짐으로서 Data 의 불가항력적인 noise 도 많아져 **Data noise 에 의한 Error 가 증가**하기 때문.
> 2. **Data 의 수가 증가할수록 Bias 와 Variance 에서 오는 Test Error 는 점차 감소**한다.
> <br>
> 이처럼 **Training Error 와 Testing Error 에서 발생하는 간극(차이)을 Generalization Error** 라고 한다.

# The Perfect Classification Algorithm
![[스크린샷 2023-11-03 오후 5.02.34.png]]
> **Objective Function**
> - Loss Function.
> - **해결하고자 하는 문제에 대하여 발생하는 Error 들의 심각성 정도에 따라 다른 Penalty 를 부여.**
> <br>
> **Parameterization**
> - Parameter 를 설정하는 것.
> - **Model 의 복잡도를 결정.**
> 	- 1차원 모델을 사용할 것인지 2차원 모델을 사용할 것 인지..
> <br>
> **Regularization**
> - Data / Model 에 대한 규제를 추가.
> - Machine Learning 에서 사용
> <br>
> **Training Algorithm**
> - 어떻게 우리가 설정한 목적 함수를 잘 학습 시킬 것인가? 에 대한 알고리즘
> 	- SGD
> 	- Adam
> - Optimizer
> <br>
> **Inference Algorithm**
> - Inference 를 하는 방법에 따라서도 성능의 차이가 발생한다.