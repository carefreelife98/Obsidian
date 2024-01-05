  

# Image Sampling and Quantization

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.32.32.png]]

## Type of Images

- Binary Image
    - 밝은곳은 1 or 255
    - 어두운 곳은 0 or 0
- Gray Scale Image
    - 0 ~ 1 사이의 다양한 값을 갖는 Pixel 들의 모임.
    - Channel 은 하나.
- Color Image
    - 여러가지의 channel을 가짐.
        - 일반적으로는 R, G, B 세 개의 Channel.

  

## Binary Image Representation

  

## Grayscale Image Representation

- 0에 가까울 수록 어두운 값을 가짐.

  

## Color Image (One Channel)

  

## Color Image Representation

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.26.39.png]]

  

## Images are Sampled

- **Resolution**
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.29.11.png]]
    
    - 이미지의 크기를 크게 할 것인지 작게 할 것인지
        - 특정 단위를 이용하여 표현. (높을수록 고화질)
            
            - DPI (Dots per Inch)
                - 일반적으로 72 DPI 사용
            - PPI (Pixels per Inch)
            
              
            
              
            

# Image Histogram

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.34.07.png]]

- 특정한 밝기에 대한 빈도 정보를 제공하는 것.
- Histogram 의 각 단위는 `bin` 이라고 함.
- 예시
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.36.26.png]]
    
      
    

  

# Images as Functions

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.41.49.png]]

- 이미지는 일반적으로 디지털(Discrete) 이미지 이다.
    - 2차원 공간 (Regular Grid) 라는 곳에 Pixel 로서 존재.
- Cartesian Coordinate
    - 이미지는 좌표 (행렬) 로서 나타낼 수 있다.
- 이미지의 좌표는 Intensity 라고 볼 수 있다.

  

  

# Linear System

## Systems and Filters

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.46.53.png]]

- Image Filtering
    - 주어진 이미지에 Filter / Kernel을 Apply
    - 해당 Filter가 우리가 원하는 대로 기존 이미지를 새로운 이미지로 Transformation 해준다.
- 목표
    - 이미지의 특정한 특징을 찾기 위함.
    - 약해진 Signal을 강화하기 위함.
- Noise
    - 소금/후추 Noise
    - Super-Resolution
        - 이미지의 고화질 화
    - In-Painting
        - 기존 이미지의 Context 와 어울리는 이미지로 변경.
- Define a System as a Unit
    
    - (n, m) : 좌표
    
      
    

## 2D Discrete Space System

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.48.50.png]]

  

  

# Filter Example

## 1. Moving Average - Image를 부드럽게 (Blurr)

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.51.02.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.53.54.png]]

- 빨간색 필터가 움직이며 새로운 이미지를 만들어 낸다.
    
    - 좌측: 입력 이미지
    - 우측 : 출력 이미지
    
      
    

## 2. Image Segmentation - Image를 Binary 화 (극단화)

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.56.29.png]]

- Pixel의 Intensity 가 100을 넘어가면 255로 변환.
    - 그렇지 않으면 0으로 변환.

  

# 여러가지 Filter 의 속성

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.57.43.png]]

  

# Spatial Properties

- Causality
    - 과거와 현재의 입력에만 효과.
    - 미래의 입력에는 효과가 없음
- Shift Invariance
    - ?

  

# Linear System (=Filters)

- 이미지를 구성하는 행렬이 특정 함수 (Filter) 를 거쳐 새로운 이미지로 반환 된다.
- **[중요 수식?]**
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.02.29.png]]
    
    - Super Position
    
      
    
      
    

# LSI (Linear Shift Invariant) Systems

- `Linear System + Shift Invariance System`

  

- 2D Impulse Function
    - 파동, 파장
    - (0, 0) 에서만 1의 값을 가짐.
    - 이 과정을 통해 ~System 이 어떤 동작을 하는지 알 수 있음.
- Sum of Delta Function

  

  

# Convolution

## 2D Convolution (*)

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.16.26.png]]

1. X 축에 대한 Flip 수행
2. Y 축에 대한 Flip 수행
3. 우리가 가진 기존 이미지와 위에서 Flip된 이미지 간의 곱셈.

  

- Kernel 과 Image가 겹치지 않는 부분은 0 으로 계산하고 날려버림.

  

  

# Correlation (**)

- Noise
- **`Correlation 이란, 두 시그널의 관련성을 나타내는 수치이다.`**