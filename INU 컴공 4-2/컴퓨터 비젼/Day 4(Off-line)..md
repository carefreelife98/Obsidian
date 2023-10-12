# Pixels and Filter
## Type of Images
**Binary Image**
- 0, 1 로 구현

**Gray Scale Image**
- 0 ~ 255 로 구현
	- 어두운 곳 : 0
	- 밝은 곳 : 255

**Color Image**
- Channel 이 세 개로 늘어남. (R, G, B)
- 각 채널의 조합으로 Color 을 표현.

## Image Smaping and Quantization

**Sampling**
- DPI , PPI
- 선명하고 깔끔한 이미지를 만들어 낼 수 있다.

**Quantize**
- 어떤 단계가 존재한다.
- Discrete

## Histogram
```python
def histogram(im):  
	h = np.zeros(255)  
	for row in im.shape[0]:
		for col in im.shape[1]: 
			val = im[row, col] 
			h[val] += 1
```
## Image as Functions
- Image 는 특정한 x, y 위치의 Indensity 의 높낮이를 가진 함수라고 생각 할 수 있다.

## Linear System (Filters)
- **Shift Invariance**
	- 입력 값의 변화를 주게되면 Output 도 같은 변화를 겪은 후에 반환된다.
	- Invariant : 불변
- **LSI (Linear Shift Invariant)**
	- F라는 입력이 발생하면 분리.
		- 상수(F[0, 0]) * 기저(d[n-0, m-0])
		- ...
	- **필터 행렬 * 기존 이미지 행렬 이라고 생각하면 될 듯?**

## 2D Convolution ( * )
![[스크린샷 2023-09-20 오후 6.34.22.png]]
1. X축 Flip
2. Y축 Flip
3. Function 과 Flip 된 이미지 h 를 곱한다.
- **상단의 수식이 곧 하단의 1, 2, 3 단계.**

# Edges
- 일반적인 이미지와 Edge 만 드러난 이미지를 볼 때 사람의 뇌는 비슷한 활동을 함.
- Edge 만으로도 충분한 정보제공을 할 수 있다.

**Goals**
- Image 의 **불연속성**을 파악 (이미지가 나뉘는 부분)
- 2D 이미지를 **Set of Curves 로 변환**

**Note**
- More Compact than Pixels
	- Memory Saving?

## Edge Detection
> **Edge 는 가파른 절벽과 같음.**
- **이미지 내의 굴곡 파악** 
	- (법선 Vector?)
- **Depth**
	- 명암
- **Surface Color**
	- 색상 별 분리
- **조명 (illumination)**


## Discrete Derivatives in 1D
![[스크린샷 2023-09-20 오후 7.01.02.png]]
- **이미지에서의 미분**
	- Central Difference [-1, 0, 1] 기억하기

## Discrete Derivatives in 2D
- **Gradient Vector**
	- **편미분** : x / y 에 중 하나에 대해서만 미분한다.

- X 방향의 미분 필터
	![[스크린샷 2023-09-20 오후 7.06.08.png]]

- Y 방향의 미분 필터
	![[스크린샷 2023-09-20 오후 7.06.23.png]]


![[스크린샷 2023-09-20 오후 7.08.48.png]]
- 위와 같은 미분 필터 적용 시 
	- **Gradient (변화점) 을 찾아내어 Edge 를 그려 낼 수 있다.**

## A Simple Edge Detector
- 이미지는 함수와 같다. f[x, y]
- 해당 이미지에 미분을 해줌으로서 Gradient를 찾을 수 있다.

## Effects of Noise
- 문제 발생.

![[스크린샷 2023-09-20 오후 7.23.03.png]]
- **Noise 가 이미지에 추가되었을 시, 위와 같이 이미지의 Intensity 에 변화**가 생김.

![[스크린샷 2023-09-20 오후 7.23.14.png]]
- 이를 다시 Gradient 를 취하게 되면 **Noise로 인해 Intensity의 변화가 잦으므로 위와 같은 Gradient 가 발생**하게 되고, **이 경우 Edge 를 찾기 어려워 진다.**

- **이를 해결하기 위해 Smoothing 사용.**

## Smoothing
방법 1. **Mean Smoothing**
![[스크린샷 2023-09-20 오후 7.28.07.png]]

방법 2. **Gaussian Smoothing**
![[스크린샷 2023-09-20 오후 7.28.19.png]]
- Gaussian 을 미분한 것 : DoG
	- DoG 를 사용해도 Edge를 찾을 수 있다.

## Sobel Operator
- DoG를 먼저 사용해서 noise 를 지우고, 이후에 미분 필터를 적용해도 같은 결과 발생하는가?
	-> Canny filter


## Canny Edge Detector
- 가장 널리 사용되는 Edge Detector.
- 