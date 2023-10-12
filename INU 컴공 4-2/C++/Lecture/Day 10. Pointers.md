---
Author: CarefreeLife98
Date: 2023-10-09T13:22:00
tags:
  - CPP
Agenda: |-
  1. Addresses and Pointers
  2. The Address of Operator &
  3. Pointers and Arrays
  4. Pointers and Functions
  5. Pointers and C-Type Strings
---
# Pointer
> **Pointer 를 사용하는 이유?**
> - 배열의 원소에 접근하기 위해
> - Call by Reference 를 통해 원본 데이터에 접근하기 위해
> - 배열과 문자열을 함수에 전달하기 위해
> - System 으로부터 메모리를 할당 받기 위해
> - Linked List 와 같은 자료 구조를 생성하기 위해

<br><br>

# Addresses and Pointers

<br><br>

# Pointer to void 
> 모든 Data type 의 데이터를 가리킬 수 있다 (저장?).
> - 어떤 목적으로 공간을 사용할 지 모르기 때문에 사용.
> - 범용적인 목적으로 사용할 수 있는 Pointer.
> - 잘 사용하지는 않는다.

`reinterpret_cast<Data Type>`
- 특정 Data Type 으로 강제 형 변환

<br><br>

# Pointers and Function (중요)
> Pointer 로 Argument 를 전달하는 세 가지.
> 
> - **Reference를 전달 할 수 있다.**

## Reference 를 통해 Function 호출 (원본 접근)
> **`&` 라는 Reference Type 존재**
> 


<br><br>

## Pointer 를 통해 Function 호출 (원본 접근)
> 변수명 앞에 * (Dereference operator) 존재

<br><br>

### 변수 하나에 여러 연산자 사용 시
> **`*ptrd++`**
> - 위와 같이 변수의 앞 뒤에 연산자가 존재 하는 경우 어떤 것부터 실행될까?
- **Unary Operator, 즉 피연산자가 하나만 존재하는 연산자는 우측 결합부터 진행**한다.
	- **`*(ptrd++)` 과 같이 실행됨.**

## Pointer 와 Function 의 관계
<br><br>

# Function Pointer
> C / C++ / Java 는 보통 함수를 변수에 저장하지 못한다.
> - 함수를 변수처럼 다룰 수 있는 언어가 존재.
> 	- 함수를 변수에 대입
> 	- 함수를 인자 값으로 전달
> 	- 함수를 리턴.
> 	= First class (JS, React, Node.js ...) 기능을 지원.
> - **Event Driven Programming 이 가능해짐.**
> 	- **Event : 사용자의 입력**
> 		- 예상 불가능.
> 	- Event 발생 시 프로그램이 적절한 처리를 해주어야 함.
> 		- Event Handler. (CallBack)
- **Function Pointer**를 사용하여 C++ 에서도 가능케 할 수 있다.
