# 1. Selection Sort

**ADL**

```
selection_sort(a, n)    for (i <- 1; i <= n-1; i <- i+1) do {        min_num <- i;        for(j <- i+1; j <= n; j <- j+1) do {            if (a[j] < a[min_num]) then                min_num <- j;				}        a[min_num]과 a[i]를 교환;		}end selection_sort()
```

  

**Source Code**

```python
import random, timedef selection_sort(a, n):    for i in range(1, len(a)):        min_num = i        for j in range(i+1, len(a)):            if a[j] < a[min_num]:                min_num = j        a[min_num], a[i] = a[i], a[min_num]def check_sort(a, n):    is_sorted = True    for i in range(1, n):        if a[i] > a[i + 1]:            is_sorted = False        if not is_sorted:            break    if is_sorted:        print("정렬 완료")    else:        print("정렬 오류 발생")if __name__ == "__main__":    # for j in range(1, 4):    #     N = 5000 * j    #     a = []    #     a.append(None)    #     for i in range(N):    #         a.append(random.randint(1, N))    #     start_time = time.time()    #     selection_sort(a, N)    #     end_time = time.time() - start_time    #     print(f'선택 정렬의 실행 시간 ({j}N = %d) : %0.3f' % (N, end_time))    #     check_sort(a, N)    for j in range(1, 4):        N = 5000        a = []        a.append(-1)        for i in range(N):            a.append(random.randint(1, N))        if j == 1:            print("초기 데이터 랜덤 상태")        elif j == 2:            a.sort()            print("\n초기 데이터 정렬 완료")        else:            a.sort(reverse=True)            print("\n초기 데이터 역순 정렬 완료")        start_time = time.time()        selection_sort(a, N)        end_time = time.time() - start_time        print(f'선택 정렬의 실행 시간 (N = %d) : %0.3f' % (N, end_time))        check_sort(a, N)
```

  

  

# 2. Bubble Sort

**ADL**

```
bubble_sort(a, n)    for(i <- n; i >= 1; i <- i-1) do        for(j <- 1; j <= i; j <- j+1) do            if(a[j] > a[j+1]) then                a[j]와 a[j+1]을 교환;end bubble_sort()
```

  

**Source Code**

```
import randomimport timedef bubble_sort(a, n):    for i in range(n, 0, -1):        for j in range(1, i):            if a[j] > a[j+1]:                a[j], a[j+1] = a[j+1], a[j]                def checkSort(a, n):    is_sorted = True    for i in range(1, n):        if a[i] > a[i+1]:            is_sorted = False        if not is_sorted:            break    if is_sorted:        print("정렬 완료")    else:        print("정렬 오류 발생")if __name__ == "__main__":    for j in range(1, 4):        N = 5000        a = []        a.append(-1)        for i in range(N):            a.append(random.randint(1, N))        if j == 1:            print("초기 데이터 랜덤 상태")        elif j == 2:            a.sort()            print("\n초기 데이터 정렬 완료")        else:            a.sort(reverse=True)            print("\n초기 데이터 역순 정렬 완료")        start_time = time.time()        bubble_sort(a, N)        end_time = time.time() - start_time        print(f'버블 정렬의 실행 시간 (N = %d) : %0.3f' % (N, end_time))        checkSort(a, N)
```

  

# 3. Insertion Sort

**ADL**

```
insertion_sort(l, n):    for(i <- 1; i <= n; i <- i+1) do {        key <- l[i];        j <- i-1;        while (j >= 0 && key < l[j]) do {            l[j + 1] <- l[j];            j <- j-1;				}        l[j + 1] <- key;		}end insertion_sort()
```

  

**Source Code**

```
import randomimport timedef insertion_sort(l, n):    for i in range(1, n + 1):        key = l[i]        j = i - 1        while j >= 0 and key < l[j]:            l[j + 1] = l[j]            j -= 1        l[j + 1] = keydef checkSort(a, n):    is_sorted = True    for i in range(1, n):        if a[i] > a[i+1]:            is_sorted = False        if not is_sorted:            break    if is_sorted:        print("정렬 완료")    else:        print("정렬 오류 발생")if __name__ == "__main__":    for j in range(1, 4):        N = 5000        a = []        a.append(-1)        for i in range(N):            a.append(random.randint(1, N))        if j == 1:            print("초기 데이터 랜덤 상태")        elif j == 2:            a.sort()            print("\n초기 데이터 정렬 완료")        else:            a.sort(reverse=True)            print("\n초기 데이터 역순 정렬 완료")        start_time = time.time()        insertion_sort(a, N)        end_time = time.time() - start_time        print(f'삽입 정렬의 실행 시간 (N = %d) : %0.3f' % (N, end_time))        checkSort(a, N)
```

  

# 4. Shell Sort

**ADL**

```
shell_sort(arr, n)    gap <- n을 2로 나눈 몫;    while(gap > 0) do {        for(i <- gap; i <= n; i <- i+1) do {            temp <- arr[i];            j <- i;            while(j >= gap && arr[j - gap] > temp) do {                arr[j] <- arr[j - gap];                j <- j - gap;						}            arr[j] <- temp;				}        gap <- gap//2;		}end shell_sort()
```

  

**Source Code**

```
import randomimport timedef shell_sort(arr, n):    # Start with a large gap and reduce it in each iteration    gap = n // 2    while gap > 0:        for i in range(gap, n + 1):            temp = arr[i]            j = i            # Move elements of the sub-array with gap to the right            while j >= gap and arr[j - gap] > temp:                arr[j] = arr[j - gap]                j -= gap            arr[j] = temp        gap //= 2def checkSort(a, n):    is_sorted = True    for i in range(1, n):        if a[i] > a[i + 1]:            is_sorted = False        if not is_sorted:            break    if is_sorted:        print("정렬 완료")    else:        print("정렬 오류 발생")if __name__ == "__main__":    for j in range(1, 4):        N = 5000        a = []        a.append(-1)        for i in range(N):            a.append(random.randint(1, N))        if j == 1:            print("초기 데이터 랜덤 상태")        elif j == 2:            a.sort()            print("\n초기 데이터 정렬 완료")        else:            a.sort(reverse=True)            print("\n초기 데이터 역순 정렬 완료")        start_time = time.time()        shell_sort(a, N)        end_time = time.time() - start_time        print(f'쉘 정렬의 실행 시간 (N = %d) : %0.3f' % (N, end_time))        checkSort(a, N)    # for j in range(1, 4):    #     N = 5000 * j    #     a = []    #     a.append(-1)    #     for i in range(N):    #         a.append(random.randint(1, N))    #     start_time = time.time()    #     shell_sort(a, N)    #     end_time = time.time() - start_time    #     print(f'쉘 정렬의 실행 시간 ({j}N = %d) : %0.3f' % (N, end_time))    #     checkSort(a, N)
```

  

  

  

  

  

  

  

  

![[Images/Untitled 49.png|Untitled 49.png]]

|   |   |   |   |
|---|---|---|---|
|실행 데이터 크기|N|2N|3N|
|선택 정렬|0.434|1.727|3.876|
|버블 정렬|1.041|4.156|9.46|
|삽입 정렬|0.476|1.922|4.269|
|쉘 정렬|0.011|0.024|0.037|

  

![[Images/Untitled 1 6.png|Untitled 1 6.png]]

|   |   |   |   |
|---|---|---|---|
|기존 데이터 상태|랜덤|정렬|역순 정렬|
|선택 정렬|0.437|0.434|0.481|
|버블 정렬|1.045|0.638|1.396|
|삽입 정렬|0.48|0.001|0.935|
|쉘 정렬|0.013|0.004|0.007|