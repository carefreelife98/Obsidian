  

**요구사항**

- 선형대수
- Vector Calculus
- Data Structure
- Algorithm
- Machine Learning

  

## Computer Vision

- Definition
    - 디지털 정보(Image)로부터 Data 추출하는 것
    - Image 를 사용하기 위한 Algorithm Building 하는 것.
- 융합형 학문
    - 생물, 물리, 수학…
    - 뇌 과학 → 최근 Computer Vision 의 주요 분야

  

- 주요 Problem
    
    - 3차원 / 2차원 사이의 간극(괴리)를 줄이는 것
    
      
    
- Trouble Shootings
    - 뇌 과학에서 많은 영감.
    - 사람의 Vision System의 연구, Algorithm 으로서 해결

  

- 사람의 시각 Vision 두 가지 중점
    - Sensing device
        - 센서
    - Interpreting device
        - 처리

  

- Receptive Field (수용장)
    - 고양이 실험
        - 어떤 물체를 보게 되면 전극이 발생.
        - 해당 전극을 분석하여 Vision System 연구.

  

- 사람의 Vision System 특징
    - 속도
        - 생존을 중점으로 빠른 속도를 자랑
    - 멍
    - 문맥 파악

  

- 그래서 Computer Vision 이란?
    - 기계에 Vision System 을 구축하여 `습득, 처리, 분석, 이해 실현`
        
        - 이미지(2d) / 비디오(3d)
        
          
        

## Brief History of Computer Vision

  

1. Picture Handling System (1960s)
    
    - 2차원 ↔ 3차원 간의 간극 연구
    
      
    
2. Stage of Visual Representation (1970s)
    
    - Target 과 Background 분리 작업
    
      
    
3. Recognition via Parts (1980s)
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.33.25.png]]
    
    - 인식
        - Part 별로 분리하여 인식
        - Skeleton Model
            - 관절 / 뼈 기반 분리 인식

  

1. Recognition via Detection
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.35.57.png]]
    
    - Edge Detection (이미지를 강렬하게 만들어 줄 수 있다.)
        
        - 갑작스럽게 변하는 지점을 파악
        - 변하는 지점을 Edge로 만들어준다
        
          
        
2. Recognition via Grouping (1980s)
    - Perceptual Grouping (Segmentation)
        - `동일한` 객체 (Instance) 끼리 묶어 인식
            - Background 끼리
            - Object 끼리

  

1. Recongnition via Matching (1990s)
    - SIFT(Scale Invariant Feature Transform) → Local Descripter

  

1. Face Detection (2000s) → 얼굴 인식
    - Computer Vision에서 가장 성공적인 산출물.
        - Edge Feature (명암에 기반)
        - Edge Feature 을 Template 으로서 사용하여 얼굴의 명암을 구분하여 얼굴 인식

  

1. PASCAL visual object challenge
    - Classification / detection
    - Segmentation
    - Action classification

  

1. ImageNet
    
    - Annotation(Label) 이 붙은 양질의 Big Data set
    
      
    
      
    
      
    

## Four Main Task of Computer Vision

1. 이미지 분류
2. 이미지 내 Object의 위치 분석
3. Object Detection → 1, 2 혼합형
4. Segmentation

  

## New Tasks of Computer Vision

1. Data generation : 데이터 생성
    
    - Noise ?
    
      
    
2. Defusion Model
    
    - 생성 모델의 한 갈래.
    - Random Process 실행을 통해 Noise 제거
    - 좀 더 Real 한 이미지 데이터 생성
    
      
    
3. Domain Adaptation
    - 실제 Domain(data) 과 가상의 Domain(data) 호환시키기
        - Machine 이 시뮬레이션(가상의 data) 를 통해 학습 후 실 세계에서 제대로 적용할 수 있도록 해주는 기법

  

1. Multi Modal Learning
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.58.26.png]]
    
    - 다양한 Modal 을 통해 Model 을 학습시키는 기법