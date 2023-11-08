---
Author: CarefreeLife98
Date: 2023-11-08T16:04:00
Agenda: 
tags:
  - Algorithm
---
# 스트링 탐색 알고리즘의 설계
> **중점**
> - 필연적으로 잘못된 시작 (False Start) 가 발생함.
> - **False start 의 횟수와 길이를 줄이는 것이 중점.**

<br><br>
## 직선적 알고리즘

## KMP 
**\[기말 시험] next\[M]**

```python
initNext(p[]):
	M <- 패턴의 길이;
	next[0] <- -1;
	for(i <- 1, j <- 0; i < M; i <- i + 1, j <- j + 1) do {
		next[i] <- j;
		while(j >= 0 and p[i] != p[j]) do {
			j <- next[j];
		}
	}
end initNext()
```