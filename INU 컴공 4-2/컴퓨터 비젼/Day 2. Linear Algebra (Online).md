  

# Vector

- Column Vector
    
      
    
- Row Vector
    
      
    

# Matrix

- 2차원 데이터를 나타냄.
    
      
    
- m 과 n 이 같은 경우, `Square Matrix` 라고 함.

  

- 컴퓨터 비젼
    - 이미지는 행렬 (Matrix) 로서 나타낼 수 있다.

  

- Color Images
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.14.04.png]]
    
    - Channel 이 3.

  

# Matrix Operations

- Addition
    - 행렬간의 덧셈
        - 각 행렬
- Scaling
    - 행렬의 곱 연산 ?
- Inner Product
    - 내적
        - 행렬의 내적
        - 벡터의 내적
            - Unit Vector 인 경우 Geographical 한 결과 도출 가능
    - 결과값
        - Scalar (숫자)
- Multiplication
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.19.55.png]]
    
    - 행렬의 곱 연산
        - 굉장히 특수한 경우에 한정됨.
        - A행렬의 m 값과 B 행렬의 n 값이 같아야 행렬의 곱 연산이 가능.
- Powers(멱)
    - 수의 제곱 (승)
- Transpose
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.22.13.png]]
    
    - m by n 행렬에서 대각선을 기준으로 m과 n의 역전? 을 실행
    
      
    
- Determinant
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.24.05.png]]
    
    - 평행 사변형의 면적을 의미.
        - A행렬과 B행렬의 곱의 Determinant 는 그 반대와 같다.
    - Determinent 가 0
        - Sigular 하다. 뒤에서 더 알아 볼 것.

  

- Trace
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.25.17.png]]
    
    - 증명에서 많이 활용됨.
    
      
    
- Identity Matrixes
    - 이 행렬은 1과 0으로 이루어 졌으며, 대각선 성분에는 1이 존재, 나머지는 0으로 존재.
    - 어떤 행렬에 곱해지더라도 대상 행렬의 값이 바뀌지 않음.
- Diagonal Matrix
    - 대각선 성분을 제외한 모든 성분의 값이 0인 행렬.
- Symetric Matrix
    - 대각선을 기준으로 역전(Transpose)을 시켜도 값이 바뀌지 않는 행렬.
- Skew-Symmetric Matrix
    - .

  

# 행렬의 변환 (Transformation)

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.29.18.png]]

- 가장 간단한 방법은 `Scaling`

  

- Rotation (회전 행렬)
    - 좌표 시스템 존재.
    - Frame 0을 Frame 1 좌표 시스템으로 변환하고 싶을 때 Rotation을 사용
    - Rotation Matrix를 기존 Matrix에 곱해줌으로서 변환 가능.
    - `회전 행렬을 통해 새로운 좌표 시스템을 생성한다`

  

- 여러개의 Matrix를 동시에 Transformarion 하고 싶을 때 ?
    - 오른쪽에서 왼쪽으로 곱해나가는 순서.

  

# Homogeneous System

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.37.58.png]]

곱이 아닌 덧셈 연산 수행시 Broadcasting 방법 가능.

하지만 곱셈 연산 시 한계 존재.

- 곱의 대상 벡터 마지막 요소로서 1이 존재하도록 변환하게 되면 Homogeneous System이 되고 위의 문제를 해결할 수 있게 됨.

  

### 2D Translation using homogeneous coordinates

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.41.38.png]]

- 주어진 p 라는 좌표를 P’ 으로 옮기는 것.

  

### Scaling equation

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.43.32.png]]

- 기존에 가지고 있던 p 라는 점을 P’으로 변환하고 싶을 때 Scaling 수식.
- 가장 마지막 성분에 1 을 추가.

  

### Scaling & Translation

- **Translation 후 Scaling 하는 것과 그 반대의 결과 값은 서로 다르다.**

  

# Scaling & Rotation & Translation

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.49.14.png]]

- Bold 체 정확하지 않으니 확인하며 학습 할 것.

[순서?]

1. Homogeneous System 으로 변환.
2. Rotation
3. Scaling

  

# Matrix Inverse - 행렬의 역행렬

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.09.30.png]]

- 행렬의 역행렬이 항상 존재하는 것은 아니다.
    - Inverse (역행렬) 이 없는 경우, `Singular` 이라고 함.

  

# Matrix Rank

## Linear Independence

- 벡터 V1 은 V2, .. , Vn으로 표현이 가능하다.
    - V1은 V2, … ,Vn 에 의존적 (dependent) 이다.

  

## Matrix Lank

- Column / Row Rank 로 표현이 되며, 그 둘은 항상 같은 값을 가진다.
- m by m Matrix 에서 rank 가 m 인경우 `Full Rank` 라고 한다.

  

# Sigular Value Decomposition (SVD)

- Image Compression 에서 주로 사용함.
    - U 와 V 는 Rotaion Matrix를 의미.
    - 시그마 는 Scaling 의미 ?