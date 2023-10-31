---
Author: CarefreeLife98
Date: 2023-10-09T18:24:00
Agenda: |-
  1. Decoder
  2. Encoder
  3. Multiplexer
tags:
  - Computer_Structure
---
# Decoders
> **n 개의 Input 을 받아 서로 다른 m 개의 Output 을 반환하는 것.** 
> - n-to-m-line Decoder
> - n X m Decoder
> <br>
> **Decoder 는 Combination Circuit 중 하나이다.**

<br><br>

## 2 X 4 decoder (2-to-4-line decoder)
![[스크린샷 2023-10-09 오후 6.39.44.png]]
> **일반적인 2 X 4 Decoder 의 모습은 위와 같다.**
> - Decoder 는 Combination Circuit 중 하나이므로 진리표 작성 가능.
> <br>
> **Enable 기능의 추가**
> - **Enable 이 0 이면 Input 에 상관없이 0** 반환 (AND Gate 동작하지 않음)
> - **Enable 이 1 이면 Input 에 따라 정상 동작.** (AND Gate 정상 동작)

<br><br>

## 3 X 8 Decoder
![[스크린샷 2023-10-09 오후 6.42.08.png]]
![[스크린샷 2023-10-09 오후 6.44.25.png]]
> **3 X 8 Decoder 도 2 X 4 Decoder 와 원리 및 동작은 같다.**

<br><br>

### 3 X 8 Decoder = 2 * (2 X 4 Decoder)
![[스크린샷 2023-10-09 오후 11.35.52.png]]
> **3 X 8 Decoder 는 두 개의 2 X 4 Decoder 로 구현할 수 있다.**
> <br>
> 3 X 8 Decoder 의 진리표를 보자.
> - **A_2 가 0 일때, 우측 상단 부분의 2 X 4 Decoder 가 동작하고 있다.**
> - **A_2 가 1 일때, 좌측 하단 부분의 2 X 4 Decoder 가 동작하고 있다.**
> <br>
> 따라서, 두 개의 2 X 4 Decoder 를 구성한 후, 각 Decoder 의 Enable Signal을 A_2 에 연결하고, 한 Decoder 의 Enable signal 앞에 Inverter 를 추가하여 0과 1의 A_2 Input을 구현한다.
> - **A_2 가 0인 경우 (Enable Signal)**
> 	- D0, D1, D2, D3 의 Output 을 반환하는 2 X 4 Decoder 동작
> - **A_2 가 1인 경우 (Enable Signal)**
> 	- D4, D5, D6, D7 의 Output 을 반환하는 2 X 4 Decoder 동작

<br><br>

## Active-Low Decoder (NAND Decoder)
![[스크린샷 2023-10-09 오후 6.46.59.png]]
![[스크린샷 2023-10-09 오후 6.50.13.png]]
> **NAND Gate 로 구성되어 있는 Decoder.**
> - 앞단의 Inverter 도 NAND 로 변환이 가능하므로 NAND Gate 로만 구설할 수 있는 Decoder 이다.

<br><br>

# Encoders
![[스크린샷 2023-10-09 오후 11.38.04.png]]
> **Encoder 는 Decoder 의 반대 기능을 하는 Device 이다.**
> - Decoder 의 Input, Output 을 역전.

<br><br>

# Multiplexers (MUX, Data Selector)

> **n 개의 Input 을 1개의 Output 으로 반환.**
> - n개의 input 중 하나의 Input 을 선정하여 반환함.
> <br>
> **Selection Signal 존재.**
> - Selection Signal 에 의해 Output 이 결정됨.
> - **Input 의 개수에 따라 필요 개수가 달라짐.**
> 	- **Input 이 2개**
> 		- **1개**의 Selection Signal (2개의 조합)
> 	- **Input 이 4개**
> 		- **2개**의 Selection Signal (4개의 조합)
> 	- **Input 이 8개**
> 		- **3개**의 Selection Signal (8개의 조합)

<br><br>

## 4 X 1 MUX (4-to-1-Line)
![[스크린샷 2023-10-09 오후 11.58.57.png]]
![[스크린샷 2023-10-10 오전 12.02.01.png]]
> 4개의 Input(I0, I1, I2, I3) 중에서 하나의 Output(Y) 반환
> - Selection Signal 2개 필요.
> <br>
> **Multiplexer 는 단순히 Input 을 선정하여 반환하는 것이므로 진리표가 필요없음.**
> - 대신 **`Function Table`** 을 사용.
> <br>
> **Multiplexer 도 Enable Signal 을 사용.**
> - Enable 이 0 인 경우 하나의 Output Y는 0을 반환.

<br><br>

## Quadruple 2-to-1 Line Multiplexer (2 X 1 MUX)
![[스크린샷 2023-10-10 오전 12.07.44.png]]
> Input 2개 (A, B) 에 하나의 Output (Y) 반환.
> - **하지만 각 Input 은 실제로 4개의 Line 을 가지고 있음. (= Quadruple)**
> 
> Selection Signal 은 한 개. (Input 이 2개 이므로)





