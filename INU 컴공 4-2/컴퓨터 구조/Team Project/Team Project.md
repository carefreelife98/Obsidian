---
Author: CarefreeLife98
Date: 2023-11-14T19:59:00
Agenda: 
tags:
---
# 표 5-6 작성
**Fetch**
- T0: AR <- PC
- T1: IR <- M[AR], PC <- PC + 1

**Decode**
- T2: D0, ... , D7 <- Decode IR(5-7), 
- AR <- IR (0-4)

**Memory-Reference**
- T4 <- T3, T5 <- T4 로 변경
	- AND
	- ADD
	- LDA
	- STA
	- BUN

# logisim 모듈 구현
