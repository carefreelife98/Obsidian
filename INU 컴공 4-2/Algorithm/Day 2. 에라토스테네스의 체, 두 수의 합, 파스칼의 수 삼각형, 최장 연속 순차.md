  

1. 에라토스테네스의 채

![[Images/Untitled 48.png|Untitled 48.png]]

```python
# eratos 
eratos(a, n)

          a[1] ← 0;

          for (i ← 2; i < n/2; i ← i + 1) do

                    for (j ← 2; i×j ≤ n; j ← j + 1) do

                              a[i×j] ← 0;

          return a;

end eratos()
```

```python
# Source Code
# 에라토스테네스의 채
def eratos(a, n):  
    a[1] = 0  
    for i in range(2, n+1):  
        if not i < n/2:  
            return a  
        j = 2  
        while i * j <= n:  
            a[i*j] = 0  
            j = j+1

# Main Function
if __name__ == "__main__":	
	N = int(input('N = '))  
	while N < 1:  
	    print(N, '은(는) 자연수가 아닙니다.')  
	    N = int(input('N = '))  
	A = list(range(N+1))  
	res = eratos(A, N)  
	for i in range(N+1):  
	    if res[i]:  
	        print(i, end=' ')
```

  

2. 두 수의 합

![[Images/Untitled 1 5.png|Untitled 1 5.png]]

```python
# ADL
twoSum(a[], n, t)
	for (i ← 0; i < n; i ← i + 1) do
		for (j ← i + 1; j < n; j ← j + 1) do
			if (a[i] + a[j] = t) then
				print(i+1 번째와 j+1 번째 원소);
end twoSum()
```

```python
# Source Code
def two_sum(a, n, t):  
    i = 0  
    while i < n:  
        j = i + 1  
        while j < n:  
            if a[i] + a[j] == t:  
                print(f"{a[i]} 번째와 {a[j]}번째 원소")  
            j += 1  
        i += 1  

# Main function
if __name__ == "__main__":	
	N = int(input('리스트의 원소 개수 : '))  
	A = []  
	for x in range(N):  
	    A.append(random.randint(1, N * 2))  
	print('리스트 : ', A)  
	T = int(input('목표값 입력 : '))  
	print('두 수의 합이 %d인 원소 쌍' % T)  
	two_sum(A, N, T)
```

  

3. 파스칼의 수 삼각형

![[Images/Untitled 2 5.png|Untitled 2 5.png]]

```python
# ADL
pascalTriangle(h)
	res ← 공백 리스트;
	pre ← 공백 리스트;
	pre에 1 추가;
	res에 pre 추가;
	for (i ← 1; i < h; i ← i + 1) do {
		cur ← 공백 리스트;
		cur에 1 추가;
		for (j ← 0; j < pre의 길이 - 1; j ← j + 1) do
			cur에 pre[j]와 pre[j+1]의 합 추가;
		cur에 1 추가;
		res에 cur 추가;
		pre ← cur;
	  }
	  return res;
end pascalTriangle()
```

```python
# Source Code
def pascal_triangle(h):  
    res = []  
    pre = [1]  
    res.append(pre)  
    i = 1  
    while i < h:  
        cur = [1]  
        j = 0  
        while j < len(pre) - 1:  
            temp = pre[j] + pre[j + 1]  
            cur.append(temp)  
            j += 1  
        cur.append(1)  
        res.append(cur)  
        pre = cur  
        i += 1  
    return res  
if __name__ == "__main__":   
    H = int(input('높이 입력 : '))  
    result = pascal_triangle(H)  
    for i in range(len(result)):  
        for j in range(len(result[i])):  
            print(result[i][j], end=' ')  
        print()
```

  

4. 최장 연속 순차

![[Images/Untitled 3 5.png|Untitled 3 5.png]]

```python
# ADL
longestSequence(a[], s, n)
	max_len ← 1;
	for (i ← 0; i < n; i ← i + 1) do {
		left ← a[i] - 1;
		right ← a[i] + 1;
		count ← 1;
		while (left ∈ s) do {
			count ← count + 1;
			s에서 left 제거;
			left ← left - 1;
		}
		while (right ∈ s) do {
			count ← count + 1;
			s에서 right 제거;
			right ← right + 1;
		}
		max_len ← max(max_len, count);
	  }
	  return max_len;
end longestSequence()
```

```python
# Source Code
def longest_sequence(a, s, n):  
    max_len = 1  
    i = 0  
    while i < n:  
        left = a[i] - 1  
        right = a[i] + 1  
        count = 1  
        while left in s:  
            count += 1  
            s.discard(left)  
            left -= 1  
        while right in s:  
            count += 1  
            s.discard(right)  
            right += 1  
        max_len = max(max_len, count)  
        i += 1  
    return max_len  

# Main function
if __name__ == "__main__":  
    N = int(input('난수의 개수 : '))  
    while N < 1:  
        print('난수의 개수는 자연수여야 합니다.')  
        N = int(input('난수의 개수 : '))  
    A = []  
    for i in range(N):  
        A.append(random.randint(1, N))  
    print('리스트 : ', A)  
    S = set(A)  
    print('집합 : ', S)  
    print('최장 연속 순차의 길이 : ', longest_sequence(A, S, N))
```

  

  

## ADL (Algorithm Description Language)

- 알고리즘 기술을 위해 정의한 언어
- 사람이 이해하기 쉽고, 프로그래밍 언어로의 변환이 용이
- 의사 코드(pseudo-code) : ADL 과 약간의 자연어로 기술한 것
- ADL 알고리즘에서 프로그램으로의 변환
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.02.35.png]]
    

- ADL Data : 숫자, 부울 값, 문자
- ADL의 명령문
    - 종류 : 지정문, 조건문, 반복문, 함수문, 입력문, 출력문
    - 명령문 끝에는 세미콜론 ; 을 사용

  

### ADL - 지정문