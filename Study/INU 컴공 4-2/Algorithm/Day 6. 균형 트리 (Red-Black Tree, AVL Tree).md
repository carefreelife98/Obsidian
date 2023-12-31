---
Author: CarefreeLife98
Date: 2023-10-09T15:01:00
tags:
  - Algorithm
---
# 균형 트리 (Balanced Tree)
## 2-3-4 Tree
> **Tree 의 균형에 신경쓰지 않아도 균형을 갖춘 트리가 된다.**
> - 만들어 나가는 **모든 과정의 모든 트리는 균형된 상태로 존재하며 진행**됨.
> 1. 하나의 노드 안의 키는 정렬된 순서로 존재.
> <br>
> **최악의 경우**
> - 트리가 **2노드**로 구성되어 있을 시.
> 	- O(logN)
> 
> **트리의 높이 h**
> - 모든 노드가 2노드로 구성 시
> 	- `2^h+1 -1`
> - 모든 노드가 4 노드로 구성 시
> 	- `4^h+1 -1`


```python
# 4 - Node
arr = [2, 1, 8, 9, 7, 3, 5, 4, 5]

# Step 1 - key 3개 삽입 가능
node1 = [1, 2, 8]

# Step 2 - 각 원소를 Tree 에 삽입, Node 가 꽉 찰 시 node1 의 가운데 원소를 위로 올림
	# 2
# 1     # 789

# Step 3 - Node 가 꽉 찼으므로 중간 원소를 위로 올림
	# 2, 8
# 1  # 7  # 9

	   # 2, 8
# 1,  # 3, 6, 7  # 9

# Step 4 - Root Node 가 4 노드가 되면 무조건 분할
	# 2, 6, 8 
# 1, # 3, # 7, # 9

	     #6
	#2        #8
#1  #3,4,5   #7 #9

	     #6
	#2        #8
#1    #4    #7 #9
	# 3, 5
```

<br><br>


## Red-Black Tree
> **Link 를 Red Link / Black Link 로 표현하는 방식**
> - Red Link 로 연결 -> 하나의 노드이다 -> 연결되어 있음
> - 좌우 균형이 맞지 않는다.
> 	- Root 에서 각 외부 노드 까지의 Black link 개수 관점에서 균형이 맞는 것.
> 
> **특징**
> 1. Root / nil node(Leaf Node)는 모두 Black
> 	- Red-Black Tree 에서 말단 노드는 nil node 라고 불림. (= Leaf Node)
> 2. Root 에서 nil node(Leaf Node) 까지의 경로 상에는 2개의 연속된 Red node 가 포함되지 않음.
> 	- Red 의 자식은 모두 Black.
> 	- Black 은 연속으로 나타날 수 있음
> 4. Root 에서 각 nil node(Leaf Node) 까지의 경로에 있는 Black node (Link) 의 수는 모두 같음
> 	- 균형 트리 성질 만족
> 5. 동일한 키를 가지는 레코드는 노드의 좌측, 우측 모두 올 수 있음.
> 6. 동일한 키가 여러 개 존재 할 시, 주어진 키를 갖는 모든 노드를 찾는 것이 어려움.
> 
> **노드 수**
> - r 을 서열(rank) 이라 할 때, 노드 수(Key 값의 수) N 은 **`N >= 2^r - 1`**
>
> **시간복잡도**
> **삽입 (높이 h <= 2log(N+1)**
> - O(logN)
> **탐색**
> - 일반 이진 탐색 트리의 탐색 연산 속도와 같음.
> - O(logN)
> <br>
> **구현 시 특징**
> - **삽입 (Insertion)**
> 	- **새로 삽입되는 노드는 항상 Red Node.**
> 		- **새로운 노드가 삽입되어 말단 노드로서 위치하게 된 경우**
> 			- **새로운 노드가 Red node 인 경우 (o)**: 
> 				- **Root node 부터 nil node 까지 Black node 의 개수 영향 X**
> 			- 새로운 노드가 Black node 인 경우 (x):
> 				- Root node 부터 nil node 까지 **Black node 의 개수 증가. (새로운 노드가 Black 이므로)**
> - **회전 (Rotation)**
> 	- **트리의 구조를 바꾸면서도 BST 의 특징을 유지시키는 방법.**
> 		- insertion 을 통한 노드 삽입 과정에서 Red node 가 연속으로 두 개 존재하는 경우 회전.

<br><br>

### 단일 회전
![[스크린샷 2023-10-09 오후 4.36.08.png]]

<br><br>

### 이중 회전
![[스크린샷 2023-10-09 오후 4.36.28.png]]




### 구현
```python
[2, 1, 8, 9, 7, 3, 6, 4, 5]

  #2
#1 #8

# 4노드가 되었으므로 분할 -> Red link를 Black link 로 변환.
# 하위 노드 분할 시 해당 노드 상단의 Link를 Red 로 변환.
# 삽입 과정 중 Red link 가 연속으로 발생 시 회전.
# Red link 가 직선으로 연속 연결 시 한번 회전.
#  " 지그재그로 연속연결 시 두 번 회전.
```

<br><br>

## AVL Tree
> **높이 균형 이진 탐색 트리 (Height-Balanced Binary Search Tree)**
> - 오른쪽 Subtree 와 왼쪽 Subtree 의 높이 차가 2 이상이 되면 **회전을 통해 트리의 높이 차를 1 이하로 유지.**
> - 높이 차가 2 이상인 경우가 2개 이상인 경우 Root 노드에서 먼, 즉 말단 노드에 가까운 경우부터 회전.

[3, 8, 9, 7, 2, 6, 4, 5, 1]

# Hashing
> **모든 키의 레코드를 산술 연산에 의해 한번에 바로 접근할 수 있는 기법.**
> <br>
> **해싱(Hashing)의 단계**
> 1. Hash Function 을 통해 탐색 키를 Hash Table 주소로 변환
> 2. 같은 테이블 주소로 사상되었을 경우에는 충돌을 해결(Collision-Resolution) 해야 함.
> <br>
> **Hash Function**
> - U : Key 의 전체 집합
> - T[0 ... m-1] : Hash Table
> - h : Hash Function
> 	- **key 값을 Table 주소로 변환하는 함수.**
> 	- h: U -> {0, 1, ..., m-1}
> <br>
> **나눗셈법**
> - 다음과 같은 Hash Function 사용.
> 	- `h(k): k mod m`

## 선형 탐사법
> **Hash Function**
> - `h(k) = k mod m`
> <br>
> **개방 주소법 (Open Addressing) 사용**
> - m 값이 입력 원소의 수보다 커야함.
> <br>
> **Clustering 발생**
> - 점유된 위치가 연속적으로 나타나는 부분 존재 시 해당 Cluster 가 점점 더 커짐.
> 	- 이러한 Cluster 는 평균 탐색 시간을 증가시킴.


<br><br>
### 이중 해싱 (Double Hashing)
> Clustering 문제를 해결
> - 두 번째 이후에 탐사되는 위치는