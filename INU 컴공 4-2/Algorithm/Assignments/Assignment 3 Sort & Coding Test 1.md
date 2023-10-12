# Quick Sort
## ADL
```c
quickSort(a, l, r) {
    If (r > l) Then {
        i <- Partition(a, l, r);
		Call quickSort(a, l, i-1);
		Call quickSort(a, i+1, r);
    }
}

End quickSort

Partition(a, l, r) {
    v <- a[r];
    i <- l – 1;
    j <- r;
    
    While (1) {
        i <- i + 1;
        While (a[i] < v) {
            i <- i + 1;
        }
        j <- j – 1;
        
        While (a[j] > v) {
            j <- j – 1;
        }

        If (i >= j) Then {
            Break
        }
        a[i] 와 a[j] 교환;
    }
    a[i] 와 a[r] 교환;
    
    Print(a);
    Return i;
}
End Partition
```
## Source Code
```python
import random, time, sys  
def quickSort(a, l, r):  
    if r > l:  
        i = partition(a, l, r)  
        quickSort(a, l, i-1)  
        quickSort(a, i+1, r)  
  
def partition(a, l, r):  
    v, i, j = a[r], l-1, r  
    while True:  
        i += 1  
        while a[i] < v:  
            i += 1  
        j -= 1  
        while a[j] > v:  
            j -= 1  
        if i >= j:  
            break  
        a[i], a[j] = a[j], a[i]  
    a[i], a[r] = a[r], a[i]  
    return i  
  
def checkSort(a, n):  
    isSorted = True  
    for i in range(1, n):  
        if a[i] > a[i+1]:  
            isSorted = False  
        if isSorted == False:  
            break  
    if isSorted:  
        print("정렬 완료\n")  
        print()  
    else:  
        print("정렬 오류 발생\n")  
        print()  
  
if __name__ == "__main__":  
    for j in range(1, 4):  
        N = 5000 * j  
        a = [-1]  
        for i in range(N, 0, -1):  
            a.append(random.randint(1, N))  
        print(f"퀵 정렬 전 배열: {a}")  
        start_time = time.time()  
        quickSort(a, 1, N)  
        end_time = time.time() - start_time  
        print(f"퀵 정렬 후 배열: {a}")  
        print(f'퀵 정렬의 실행 시간 ({j}N = %d) : %0.3f' % (N, end_time))  
        checkSort(a, N)  
#  
# if __name__ == "__main__":  
#     for y in range(1, 4):  
#         N = 500  
#         sys.setrecursionlimit(3002)  
#         a = [-1]  
#         for x in range(N, 0, -1):  
#             a.append(random.randint(1, N))  
#         if y == 1:  
#             print(f"초기 데이터 랜덤 상태: {a}")  
#         elif y == 2:  
#             a.sort()  
#             print(f"\n초기 데이터 정렬 완료: {a}")  
#         else:  
#             a.sort(reverse=True)  
#             # "a[0] = -1" 은 제자리에 다시 위치  
#             a[0], a[len(a) - 1] = a[len(a) - 1], a[0]  
#             print(f"\n초기 데이터 역순 정렬 완료: {a}") 
#
#         start_time = time.time()  
#         quickSort(a, 1, N)  
#         print(f"정렬 완료: {a}")  
#         end_time = time.time() - start_time  
#         print(f'퀵 정렬의 실행 시간 (N = %d) : %0.3f' % (N, end_time))#         checkSort(a, N)  
  
  
  
# if __name__ == "__main__":  
#     N = 10  
#     sys.setrecursionlimit(3002)  
#     a = [-1]  
#     for i in range(N, 0, -1):  
#         a.append(random.randint(1, N))  
#     start_time = time.time()  
#     quickSort(a, 1, N)  
#     end_time = time.time() - start_time  
#     print('퀵 정렬의 실행 시간 (N = %d) : %0.3f' % (N, end_time))#     checkSort(a, N)
```

## Result
![[Pasted image 20230926034542.png]]
- N의 크기에 따른 실행시간의 변화 측정

![[Pasted image 20230926034549.png]]
- 초기 데이터의 상태에 따른 실행 시간의 변화 측정

# Merge Sort
## ADL
```c
merge(a, l, m, r)
    i <- l;
    j <- m + 1;
    k <- l;
	b <- CopyArray(a);
	
	While (i <= m && j <= r) {
        If (a[i] <= a[j]) Then {
            b[k] <- a[i];
            i <- i + 1;
        Else {
            b[k] <- a[j];
            j <- j + 1;
        }
        End If
        k <- k + 1;
    }
    End While

    While (i <= m) {
        b[k] <- a[i];
        i <- i + 1;
        k <- k + 1;
    }
    End While

    While (j <= r) {
        b[k] <- a[j];
        j <- j + 1;
        k <- k + 1;
    }
    End While

    For (p <- l; p >= r; p++) {
        a[p] := b[p]
    }
    End For
End merge
```

## Source Code
```python
import random  
import sys  
import time  
  
def merge(a, l, m, r):  
    i = l  
    j = m+1  
    k = l  
  
    # 배열 b는 함수 외부에서“b = a.copy()” 명령문을 사용하여 주어진다고 가정  
    b = a.copy()  
    # a[i]와 a[j]를 비교하여 작은 값을 b[k]에 저장  
    while i <= m and j <= r:  
        if a[i] <= a[j]:  
            b[k] = a[i]  
            i += 1  
        else:  
            b[k] = a[j]  
            j += 1  
        k += 1  
    while i <= m:  
        b[k] = a[i]  
        i += 1  
        k += 1  
    while j <= r:  
        b[k] = a[j]  
        j += 1  
        k += 1  
    for p in range(l, r + 1):  
        a[p] = b[p]  
  
def mergeSort(a, l, r):  
    if r > l:  
        m = (r+l)//2  
        mergeSort(a, l, m)  
        mergeSort(a, m+1, r)  
        merge(a, l, m, r)  
  
def checkSort(a, n):  
    isSorted = True  
    for i in range(1, n):  
        if (a[i] > a[i+1]):  
            isSorted = False  
        if (isSorted == False):  
            break  
    if isSorted:  
        print("정렬 완료\n")  
        print()  
    else:  
        print("정렬 오류 발생\n")  
        print()  
  
# if __name__ == "__main__":  
#     for j in range(1, 4):  
#         N = 5000 * j  
#         a = [-1]  
#         for i in range(N, 0, -1):  
#             a.append(random.randint(1, N))  
#         print(f"합병 정렬 전 배열: {a}")  
#         start_time = time.time()  
#         mergeSort(a, 1, N)  
#         end_time = time.time() - start_time  
#         print(f"합병 정렬 후 배열: {a}")  
#         print(f'합병 정렬의 실행 시간 ({j}N = %d) : %0.3f' % (N, end_time))#         checkSort(a, N)  
#  
if __name__ == "__main__":  
    for y in range(1, 4):  
        N = 5000  
        sys.setrecursionlimit(3002)  
        a = [-1]  
        for x in range(N, 0, -1):  
            a.append(random.randint(1, N))  
        if y == 1:  
            print(f"초기 데이터 랜덤 상태: {a}")  
        elif y == 2:  
            a.sort()  
            print(f"\n초기 데이터 정렬 완료: {a}")  
        else:  
            a.sort(reverse=True)  
            # "a[0] = -1" 은 제자리에 다시 위치  
            a[0], a[len(a) - 1] = a[len(a) - 1], a[0]  
            print(f"\n초기 데이터 역순 정렬 완료: {a}")  
        start_time = time.time()  
        mergeSort(a, 1, N)  
        print(f"정렬 완료: {a}")  
        end_time = time.time() - start_time  
        print(f'합병 정렬의 실행 시간 (N = %d) : %0.3f' % (N, end_time))  
        checkSort(a, N)  
  
  
# if __name__ == "__main__":  
#     N = 10  
#     sys.setrecursionlimit(3002)  
#     a = [-1]  
#     for i in range(N, 0, -1):  
#        a.append(random.randint(1, N))  
#     print(f"초기 배열: {a}")  
#     start_time = time.time()  
#     mergeSort(a, 1, N)  
#     end_time = time.time() - start_time  
#     print(f"정렬 후 배열: {a}")  
#     print('합병 정렬의 실행 시간 (N = %d) : %0.3f'%(N, end_time))#     checkSort(a, N)
```

## Result
![[Pasted image 20230926035302.png]]
- N의 크기에 따른 실행시간의 변화 측정

![[Pasted image 20230926035305.png]]
- 초기 데이터의 상태에 따른 실행 시간의 변화 측정


# Heap Sort
## ADL
```python
heapify(arr, n, i)
    largest <- i;
    left <- 2 * i + 1;
    right <-2 * i + 2;

    If left < n && arr[left] > arr[largest] Then {
        largest <-left;
    }

    If right < n && arr[right] > arr[largest] Then {
        largest <-right;
    }

    If largest ≠ i Then {
        arr[i] 과 arr[largest] 교환;
        Call heapify(arr, n, largest);
    }
    End If
End heapify

heapSort(arr)
    n <-Length of arr;
    For (i <- n // 2 – 1; i >= 0; i--){
		Call heapify(arr, n, i);
	}

    For (i <- n – 1; i >= 1; i--){
		arr[i] 와 arr[0] 교환;
		Call heapify(arr, i, 0);
	}
End heapSort
```

## Source Code
```python
import random  
import time  
  
  
def heapify(arr, n, i):  
    largest = i  
    left = 2 * i + 1  
    right = 2 * i + 2  
  
    if left < n and arr[left] > arr[largest]:  
        largest = left  
  
    if right < n and arr[right] > arr[largest]:  
        largest = right  
  
    if largest != i:  
        arr[i], arr[largest] = arr[largest], arr[i]  
        heapify(arr, n, largest)  
  
def heapSort(arr):  
    n = len(arr)  
  
    for i in range(n // 2 - 1, -1, -1):  
        heapify(arr, n, i)  
  
    for i in range(n - 1, 0, -1):  
        arr[i], arr[0] = arr[0], arr[i]  
        heapify(arr, i, 0)  
  
def checkSort(a, n):  
    isSorted = True  
    for i in range(1, n):  
        if (a[i] > a[i+1]):  
            isSorted = False  
        if (isSorted == False):  
            break  
    if isSorted:  
        print("정렬 완료\n")  
        print()  
    else:  
        print("정렬 오류 발생\n")  
        print()  
  
if __name__ == "__main__":  
    for j in range(1, 4):  
        N = 5000 * j  
        a = [-1]  
        for i in range(N):  
            a.append(random.randint(1, N))  
        start_time = time.time()  
        heapSort(a)  
        end_time = time.time() - start_time  
        print(f'히프 정렬의 실행 시간 ({j}N = %d) : %0.3f' % (N, end_time))  
        checkSort(a, N)  
  
# if __name__ == "__main__":  
#     for y in range(1, 4):  
#         N = 500  
#         sys.setrecursionlimit(3002)  
#         a = [-1]  
#         for x in range(N, 0, -1):  
#             a.append(random.randint(1, N))  
#         if y == 1:  
#             print(f"초기 데이터 랜덤 상태: {a}")  
#         elif y == 2:  
#             a.sort()  
#             print(f"\n초기 데이터 정렬 완료: {a}")  
#         else:  
#             a.sort(reverse=True)  
#             # "a[0] = -1" 은 제자리에 다시 위치  
#             a[0], a[len(a) - 1] = a[len(a) - 1], a[0]  
#             print(f"\n초기 데이터 역순 정렬 완료: {a}")  
#         start_time = time.time()  
#         quickSort(a, 1, N)  
#         print(f"정렬 완료: {a}")  
#         end_time = time.time() - start_time  
#         print(f'히프 정렬의 실행 시간 (N = %d) : %0.3f' % (N, end_time))#         checkSort(a, N)  
  
  
# if __name__ == "__main__":  
#     arr = [12, 11, 13, 5, 6, 7]  
#     heapSort(arr)  
#     print("Sorted array is:", arr)
```

## Result
![[Pasted image 20230926035520.png]]
- N의 크기에 따른 실행시간의 변화 측정

![[Pasted image 20230926035523.png]]
- 초기 데이터의 상태에 따른 실행 시간의 변화 측정

# Coding Test - 구간 합 구하기
## 문제 : 구간 합 구하기
**`[문제]`**
N×N 개의 수가 N×N 크기의 표에 채워져 있다. (x1, y1)부터 (x2, y2)까지 합을 구하는 프로그램을 작성하시오. (x, y)는 x행, y열을 의미한다.

예를 들어, N = 4이고, 표가 아래와 같이 채워져 있는 경우를 살펴보자.

|   |   |   |   |
|---|---|---|---|
|1|2|3|4|
|2|3|4|5|
|3|4|5|6|
|4|5|6|7|

여기서 (2, 2)부터 (3, 4)까지 합을 구하면 3 + 4 + 5 + 4 + 5 + 6 = 27이고, (4, 4)부터 (4, 4)까지 합을 구하면 7이다.

표에 채워져 있는 수와 합을 구하는 연산이 주어졌을 때, 이를 처리하는 파이썬 프로그램을 작성하라.


**`[입력]`**
- 첫째 줄에 표의 크기 N과 합을 구해야 하는 횟수 M이 주어진다. (1 ≤ N ≤ 1024, 1 ≤ M ≤ 100,000)

- 둘째 줄부터 N개의 줄에는 표에 채워져 있는 수가 1행부터 차례대로 주어진다.

- 다음 M개의 줄에는 네 개의 정수 x1, y1, x2, y2 가 주어지며, (x1, y1)부터 (x2, y2)의 합을 구해 출력해야 한다.

- 표에 채워져 있는 수는 1,000보다 작거나 같은 자연수이다. (x1 ≤ x2, y1 ≤ y2)


**`[출력]`**
총 M줄에 걸쳐 (x1, y1)부터 (x2, y2)까지 합을 구해 출력한다.


**`[실행 예]`**
4 3

1 2 3 4
2 3 4 5
3 4 5 6
4 5 6 7

2 2 3 4
3 4 3 4
1 1 4 4

27
6
64


## Source Code
```python
import random  
def gen_matrix(n, m):  
    try:  
        # (n,m) 범위 설정 확인  
        if 1 >= n >= 1024 or 1 >= m >= 100000:  
            return "1 <= n <= 1024, 1 <= m <= 100,000 범위가 잘못되었습니다."  
        print()  
        print(f"{n} X {n} 행렬 생성:")  
        a = []  
        for _ in range(n):  
            b = []  
            for i in range(n):  
                num = random.randint(1, 9)  
                b.append(num)  
            a.append(b)  
        for i in a:  
            print(i)  
        return a  
    except Exception as e:  
        print(f"Error Occurred: [{e}]")  
  
def gen_coordinate(a, n, m):  
    print(f"\n위에서 생성된 {n} X {n} 행렬에서 다음 {m}개의 좌표 구간 합 결과를 반환:")  
  
    xy = []  
    for i in range(m):  
        temp = []  
        for j in range(4):  
            temp.append(random.randint(0, n - 1))  
        xy.append(temp)  
    for a in xy:  
        print(a)  
    return xy  
  
def sum_mat_bydiff(l, x1, y1, x2, y2):  
    total = 0  
    for i in range(x1, x2 + 1):  
        for j in range(y1, y2 + 1):  
            total += l[i][j]  
            print(f"total = {total}, l[{i}][{j}] = {l[i][j]}")  
    return total  
  
def sum_result(matrix, coordinate: []):  
    try:  
        for idx, value in enumerate(coordinate):  
            print(f"\n {idx + 1} / {len(coordinate)} 번째 좌표 [{value}] 간 값의 합 계산 시작")  
            x1, y1, x2, y2 = 0, 0, 0, 0  
            for j in range(4):  
                match j:  
                    case 0: x1 = value[j]  
                    case 1: y1 = value[j]  
                    case 2: x2 = value[j]  
                    case 3: y2 = value[j]  
            print(f"(x1, y1) = ({x1}, {y1}), (x2, y2) = ({x2}, {y2})")  
            if x1 == x2 and y1 == y2:  
                print("두 좌표가 같습니다.")  
                total = matrix[x1][y1]  
            else:  
                min_x, max_x = min(x1, x2), max(x1, x2)  
                min_y, max_y = min(y1, y2), max(y1, y2)  
                total = sum_mat_bydiff(matrix, min_x, min_y, max_x, max_y)  
            print(f"Total = {total}")  
    except Exception as e:  
        print(f"Error Occurred: [{e}]")  
  
if __name__ == "__main__":  
    print("n, m  입력:")  
    n, m = map(int, input().split())  
  
    mat = gen_matrix(n, m)  
    co_ordinate = gen_coordinate(mat, n, m)  
    sum_result(mat, co_ordinate)
```

## Result

![[Pasted image 20230926035621.png]]
![[Pasted image 20230926035626.png]]

### 실행 방법
1.    표의 크기 N과 합을 구해야 하는 횟수 M을 띄어쓰기로 구분하여 입력.
2.    이후 N x N 행렬과 두 좌표 (x1, y1) <-> (x2, y2) 및 그 횟수(M) 는 Random 으로 자동 생성됨.
