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
T0: AR<-PC
T1: IR<-M[AR],PC<-PC+1   
T2:D0,. . . .D7<- Decoded IR(5~7)
AR-< IR(0~4)
AND 0 0 0 D0T3   DR<-M[AR]
 D0T4     AC ← AC ^ M[AR], SC ← 0 
ADD 0 0 1 D1T3   DR<-M[AR]
D1T4 AC<-AC+DR, E<-Cout, SC<-0                  
LDA 0 1 0  D2T3  DR<-M[AR]
            D2T4  AC ← DR , SC ← 0 
STA 0 1 1   D3T3  M[AR] ← AC, SC ← 0 
BUN 1 0 0   D4T3  PC ← AR, SC ← 0


# Logisim Module Naming
## Timing Control
