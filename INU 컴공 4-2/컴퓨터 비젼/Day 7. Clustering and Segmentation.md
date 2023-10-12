---
Author: CarefreeLife98
Date: 2023-10-11T18:05:00
Agenda: 1. Image Segmentation
tags:
  - Computer_Vision
---
# Image Segmentation
## The Goal of Segmentation
**Goal**
> 1. **비슷한 값을 가진 Pixel을 묶어 그룹화.**
> 2. 비슷한 값을 가진 Pixel을 객체로 대함.
> 3. **시각적으로 유사한 Pixel을 묶어 그룹화.**

<br><br>
**Human Segmentation**

<br><br>

## Main Progresses for Segmentation
> Tokens
>
>
> **Segmentation 을 위한 두 가지 방법**
> 1. Bottom-up
> 	- 유사한 것들끼리 묶어나가는 방식
> 2. Top-down
> 	- 

<br><br>
## Segmentation as Clustering
> **Clustering**
> - Unsupervised learning method
> 	- Supervised : {x, y}
> 	- Semi-Supervised : {x, y} {u }
> 	- Unsupervised : {u}
> 		- Label 이 존재하지 않음
> - **Data 가 Fearure Vectors 로 구성되어 있어야 함.**
> 	- `Cosine Similarity` 를 주로 사용.



![[스크린샷 2023-10-11 오후 6.16.34.png]]
> 어떠한 것을 Point로 사용 할 것인가?
> 어떤 점을 비슷한 점으로 볼 것인가?

<br>
<br>
![[스크린샷 2023-10-11 오후 6.18.31.png]]
> **Cluster 의 계층 (Hierarchi) 을 나눌 수 있다.**
> - Animal(Dog, Cat, Horse..), Plant(Tree, Flower ...)

<br><br>
## Clustering 방식
> **Agglomerative Clustering (= Dendrogram)**
> - Bottom-up Hierarchical Cluster
> <br>
> - **장점**
> 	- **Adaptive Shape**
> 		- Shape 이 급작스럽게 생성되는 것이 아닌 점점 확장되는 모습을 가짐.
> - **단점**
> 	- 
> <br>
> - **Algorithm**
> 	- **Initialization**
> 		![[스크린샷 2023-10-11 오후 6.46.52.png]]
> 		- 모든 데이터는 각각의 Cluster.<br>
> 	- **Repeat**
> 		![[스크린샷 2023-10-11 오후 6.47.35.png]]
> 		- 비슷한 Cluster 끼리 Pairing.
> 		- Pairing 시 하나의 Cluster 가 된다.
> 		<br>
> 	- **Until**
> 		![[스크린샷 2023-10-11 오후 6.48.08.png]]
> 		- Cluster 의 목표 개수 도달 시 종료.
> 		- 하나의 (전체의) 클러스터가 생성되면 종료.
> <br>
> **k-means**
> - 가장 일반적인 형태의 Clustering 방식
> - 초기화 (Initialization)에 굉장히 민감함.
> - Clustering의 결과가 좋지 않을 수 있다.
> - 
> - 2D Image 를 1D 로 표현.
> - Data 들의 각 군집 내부에서 해당 군집의 Center 에 존재하는 Data 탐색
> 	- SSD (Sum of Square Distance) 알고리즘을 사용.
> - **Objective Function**
> 	- 줄여나가야 할 대상 탐색
> <br>
> **Algorithm**
> ![[스크린샷 2023-10-11 오후 7.11.55.png]] 
> 1. **Initialization (b)**
> 	- 랜덤한 Cluster Center C_0 를 선택.
> 2. **E-step (Assignment Step) (c)**
> 	- 각 Data 에 Cluster Label 을 할당하는 과정.
> 3. **M-step (Update step) (d)**
> 	- 각 Cluster의 Center 를 갱신한다.
> 4. **Iterate (E, M Step) (e, f)**
> 	- 최대 Iteration 조건 도달 시까지
> 	- 더 이상 변화하는 것이 없을 때까지
> <br>
> **Choosing K - 가장 이상적인 K 선택하기**
> - 정답은 없음
> 1. **모든 K 에 대하여 Objective Function 수행.**
> 	- 그래프가 꺾이는 부분 (Elbow point) 의 K를 Pick 한다.
> 2. **Silhouette value**
> 	- Cluster 내의 Data 가 얼마나 유사한지 측정
> 	- Cluster 간의 분리가 잘 되어 있는지 측정.
> <br>
> **Distance Measure : 거리 측정 방식**
> 1. Euclidian (Most Common)
> 2. Cosine
> 3. Non-lenear (Kernel Method)
> <br>
> **Feature Space**
> - **Color 라는 조건만으로 Image Segmentation 은 부정확함.**
> 	- Pander 와 나무의 밝은 부분 RGB 값이 유사한다면 나무까지 Pander 로 인식 할 수 있음.
> - **Texture Similarity**
> 	- 이미지의 질감을 기준으로 Clustering 할 수 있음.
> - **Position**
> 	- 같은 Intensity 를 가지더라도 다른 위치에 존재하는 Cluster 를 Segmentation 할 수 있다.
> <br>
> **K-means Clustering 의 한계**
> - **Hard Assignment**
> 	- 모든 Point 는 Cluster 에 속하거나, 속하지 않거나 둘 중 하나로 존재함.
> 	- Flexible 하지 못하다.
> - **Outlier examples**
>
> <br>
>> 쉽게 말해서, 장님이 코끼리를 만져가며 코를 찾는 것처럼, Local Optimum 을 계속 찾아가며 확장해 가는 방식으로 실제 d*, c* 를 찾는다.
>
> <br>
> **Mean-shift Clustering**
> - 평균을 옮기는 Clustering
> ![[스크린샷 2023-10-11 오후 7.34.20.png]]<br>
> **중요한 가정**
> - **Data 분포는 pdf 를 이루고 있다.**
> 	- pdf : Gaussian
> - **각 pdf (Gaussian)의 Peak Point 를 찾는 것을 목표로 하는 Clustering 방식.**
> <br>
> **Main Idea**
> 1. Random Data 를 선택.
> 2. Center of Mess 탐색
> 3. 평균을 이동
> 	- Source ---Mean Shift vector---> Target
> 4. Iteration
> <br>
> **장점**
> 
> 
> **단점**
> 

<br><br>

## Example of Segmentation
**Image Segmentation**
![[스크린샷 2023-10-11 오후 6.23.52.png]]

**Video Segmentation**
![[스크린샷 2023-10-11 오후 6.24.04.png]]

## Gestalt Theory
![[스크린샷 2023-10-11 오후 6.25.46.png]]
> **Gestalt**
> - 전체, 그룹을 의미.
> <br>
> **Gestalt Theory**
> - 여러 Part 가 모여 전체를 구성한다.

<br><br>
## Similarity & Proximity

## Common fate
> 부드럽게 이어진 곡선 (Line)

<br><br>
## Contunuity
> 연속성

<br><br>
## Closure

