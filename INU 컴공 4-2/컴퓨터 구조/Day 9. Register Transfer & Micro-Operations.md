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