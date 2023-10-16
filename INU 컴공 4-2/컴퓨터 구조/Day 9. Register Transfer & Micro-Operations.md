# Register Transfer Language
> **CPU 는 Register 간의 전송(수학적 처리) 을 통해 Data 가 처리된다.**
> <br>
> **Micro Operations (마이크로 연산)**
> - **Register 에 저장된 Data 들의 조작.**
> 	- **한 Clock** 안에 Register 의 전송 동작.
> - Ex) shift, count, clear, load
> <br>
> **Register Transfer Language**
> - **Micro 연산을 간편하게 표현하는 언어**
> - **Register 는 (직)사각형 으로 표현**
> 	![[스크린샷 2023-10-17 오전 8.00.26.png]]
> 	- Register 의 **이름**으로 표현
> 	- Register 를 **구성하는 각 Bit** 로 표현
> 	- Register 의 **최상위, 최하위 Bit** 로 표현
> 	- Register 의 **상, 하위 Bit (H, L) 를 각각** 표현
> <br>
> **일반적인 표현 방식**
> ![[스크린샷 2023-10-17 오전 8.22.54.png]]
> ![[스크린샷 2023-10-17 오전 8.24.42.png]]
> - **P**
> 	- Control Circuit 에서 발생하는 신호
> - **R1, R2**
> 	- Register
> - **P 가 1일 때 Load 되어 R1 에 있는 값이 R2로 전송된다.**
> 	- 병렬 Load 를 가지는 Register
> 		- Load 가 0 일때 외부 입력 R1 이 R2 로 전송되는 방식으로 구현 가능. (파란색 Box 부분)
> - **Exchange, 즉 두 Register 의 값의 교환은 다음과 같이 표현 할 수 있다.**
> 	- **T: R2 <- R1, R1 <- R2**
> 	- " , (쉼표) " 는 동시에 발생하는 연산을 나타낼 때에 사용

<br><br>
## Register Transfer Language 의 구성요소
![[스크린샷 2023-10-17 오전 8.29.40.png]]
> **Register**
> - Alphabet 이나 숫자를 붙혀 표현
> - 최상/하위 Bit, 상/하위 Bit 등으로 표현
> <br>
> **Arrow (<-)** 
> - Data 의 전송을 표현
> - 방향은 (Destination <- Source) 으로 고정하여 사용.
> <br>
> **Comma ( , )**
> - 동시에 발생하는 Micro Operation 을 표현

