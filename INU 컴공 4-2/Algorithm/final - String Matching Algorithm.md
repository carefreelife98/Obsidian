---
Author: CarefreeLife98
Date: 2023-12-10T16:07:00
Agenda: 
tags:
  - Algorithm
---
# KMP
> **전체 Text 에서 찾고자 하는 특정 패턴의 내부 구성 요소 반복 정보를 기반으로 불필요한 비교를 줄이는 알고리즘.**
> - 패턴과 같은 크기를 가진 **패턴을 구성하는 요소의 반복 횟수**를 나타내는 **next 배열**을 생성.
> - **불일치 발생 시**
> 	- **해당 패턴 요소의 index - 1 번째의 next 배열 값을 index 로 가지는 패턴 요소를 불일치 발생 위치로 옮겨 다시 비교 시작.** 

## KMP - next 배열 구하기 시험용
> j 와 next\[j] 는 text 와 pattern 의 비교 index.
> - 비교 시작 시 text 는 index 1 부터 시작, pattern 은 index 0 부터 시작
> - 일치 시 next\[j] 값이 증가한다.
> - 불일치 발생 시 불일치 지점 다음 위치부터 재비교.


# Rabin- Karp
> 1. Pattern 의 길이를 기준으로 Pattern 의 Hash 값을 계산.
> 2. Sliding Window를 통해 잡히는 Text 와 Pattern 의 Hash 값이 같은 경우 Pattern 과 Text 의 구성 요소를 비교 (Hash 값이 충돌 할 수 있기 때문에)
> 3. 일치 시 String Matching 성공
