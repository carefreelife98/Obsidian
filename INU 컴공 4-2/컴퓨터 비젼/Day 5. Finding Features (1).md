> 이미지 내에서 특징 찾기

# Local Invariant Features
## Image Matching
> 특징을 추출해야 한다.
> 	- 항공 뷰 -> 길, 교차로
> 	- Panorama Image (Auto Stitch)

## Local Features (지역 특징) 을 사용하는 이유
> Machine Learning 에서는 입력을 입력 그 자체로 사용하지 않고, 입력을 Representation 이라는 형태로 변환 시켜 사용한다.

### Global Representation
> **Global Representation :**
> 	- **Image 전체에 대한 특징**을 찾는 것.
> 	- Image 는 결국 **Vector.**
> **Global Representation 의 문제점:**
> 	- 이 경우, **Image의 Spacial Information 을 잃게 됨.**
> 		- **Spacial Information : 위치, 공간 정보** (이미지 내 특정 객체의 위치 정보)
> 	- 객체의 크기에 따라 문제가 발생.
> 	- **Occlusion(폐색) 발생**
> 		- 특정 물체나 빛이 다른 물체에 가려지거나 차단되는 현상.
> 		- 마스크에 의한 얼굴 인식 잠금 해제: 마스크 착용 후 얼굴 인식하게 되면 Occlusion 발생.
> 	- **Discriminative 감소**
> 		- 이미지 내 객체 별 구분력이 감소함.

### Local Representation
> **Local Representation :**
> 	- **Image 전체를 각 영역 (Grid) 로 나누어 특징을 찾는 것.**
> **Local Representation 의 장점 :**
> 	- Spatial Information 의 보존
> 	- Global Representation 에 비해 상대적으로 Occlusion 감소
> 	- Intra-category Variation (같은 종류 객체 간 약간의 차이) 에 강인함
> 	- Discriminative 증가
> 	- Hierarchical (계층적) Information Processing 용이
> 	- Interpretability (해석 가능성)
> 		- 위와 같은 장점들에 기반해 Global Representation 에 비해 해석가능성 증가.


## General Approach
> 제시된 두 이미지에 대하여,

1. **Find a set of distinctive key points**
	- 특징적인 영역 탐색
2. **Define a region around each keypoint**
	- 특징적인 부분 뿐만 아니라, 그 주변까지 탐색의 범위에 포함.
3. **Extract and Normalize the region content**
	- 탐색한 특징적인 부분이 포함된 이미지를 추출
	- 해당 이미지의 픽셀 분포를 Normalize.
4. **Compute a local descriptor from the normalized region**
	- Local Descriptor == Feature
	- 해당 이미지 내에서 이미지를 표현 할 수 있는 
5. **Match local descriptors**
	![[스크린샷 2023-09-28 오후 5.28.24.png]]
	- Correseponding 하는 Pair 찾기


## 문제점
1. **Detect the some point Independently in both images**
	- **동일한 두 이미지에서 적은 횟수의 Detect 를 하게 될 경우, 두 keypoint 가 다를 수 있다.**
	- 따라서, **Repeatable Detector** 를 사용해 **여러 번의 Detect** 를 수행해야 한다.
2. **Identify its correct correspondence in the other images for each keypoint.**
	- 대응되는 여러 개의 keypoint 가 **이미지를 보는 시점 및 관점(Geometrical) & 빛의 세기 (Illumination)** 에 따라 바뀔 수 있음.
	- 따라서, **신뢰할 수 있고 (Reliable), 구별 가능한(Distictive) Detecting**이 필요함.


## What are Main Questions?
> **Which points are distinctive? are they repeatable?**
> 	- Distictive -> **Features & Keypoints, Salient points**
> 	- Repeatable -> **It can be Re-detected from other views.**
>
> **How to describe a local region?**
> 
> **How to establish correspondence?**
> 	- Ex) Compute matches?


## What is Repeatable & Distictibe Feature?
> 기계가 Matching Task 를 잘 수행하기 위해서 어떠한 영역을 고려해야 할 까?

![[스크린샷 2023-09-28 오후 6.15.30.png]]
> **각 Local 영역의 Repeatable & Distictive 여부**
> **3번 영역**
> 	- Distinctive
> 		- 비슷한 영역이 매우 많음. (불만족)
> **2번 영역**
> 	- Distinctive
> 		- 능선을 따라 비슷한 이미지가 생성됨. (불만족)
> 		- Edge
> **1번 영역**
> 	- Distinctive
> 		- **어떻게 움직이더라도 크게 비슷한 이미지가 생성되지 않음. (만족)**
> 		- **Edge 와 Edge 가 만나 특징을 발생**시킴.
> 			- **Corner** 라고 함.

## Point Features : Corners VS Blob detectors
### Corner Detector
> A corner is defined as the intersection of one or more edges
> 	- **코너(Corner) 는 Edge 와 Edge 가 만나 생기는 경계.**
> **장점**
> 	- Corner 는 **높은 Localization 정확도**를 가진다.
> 	- **Visual Odometry (3D Vision) 에 좋은 성능**을 가짐.
> 		- View 가 다르더라도 Keypoint 의 Matching 을 잘함.
> **단점**
> 	- **Blob Detector 에 비해 상대적으로 낮은 Distinctive 성능.**

### Blob Detector
> A Blob is any other image pattern which is not a corner, that differs significantly from its neighbors in intensity and texture.
> 	- **블롭 (Blob) 은 코너 (Corner) 이 아닌 모든 Image pattern.**
> **장점**
> 	- **Place Recognition 에 유리**함.
> 	- 코너에 비해 상대적으로 **좋은 성능의 Distinctive** 능력.
> **단점**
> 	- 코너에 비해 상대적으로 **부정확한 Localization.**


# Corner Detector
## The Moravec Corner Detector
> **기계가 Corner를 인지하는 방법 중 하나**
> **SSD 의 변화 양상 관측을 통해 Corner를 찾아낸다.**
> 	 - **Small Window** 를 사용.
> 	 - Small Window 를 8방향으로 움직여 **이미지 (픽셀) 의 변화를 관찰.**
> 		 - **변화는 Sum of Squared Difference (SSD) 단위로 측정**됨.

![[스크린샷 2023-09-28 오후 7.29.46.png]]

**Flat**
- 같은 픽셀 값을 가진 평면 위에 Window 가 존재.
- SSD ≈ 0 (변화 없음)

**Edge**
- Edge 의 방향을 따라 Window 이동 시 픽셀 값의 변화 없음.
	- SSD ≈ 0 (변화 없음)
- 하지만 Edge 방향과 다른 방향으로 Window 이동 시 SSD > 0

**Corner**
- 두 가지 이상의 Edge 방향이 존재하며, 해당 Window 에서 픽셀 값에 큰 변화가 존재하는 경우.


## The Harris Corner Detector
> **The Moravec corner detector 기반.**
> 	- **물리적으로 Window를 움직이지 않는다.**
> 	- **Patch 그 자체를 관측.**
> 		- **Different Calculus** 사용.

