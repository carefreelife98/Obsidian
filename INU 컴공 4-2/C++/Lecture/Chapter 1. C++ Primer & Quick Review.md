---
Author: CarefreeLife98
Date: 2023-10-22T15:58:00
Agenda:
  - 1. C++ Programming Elements
tags:
  - CPP
---

# Basic C++ Programming Elements
![[스크린샷 2023-10-22 오후 4.28.58.png]]
> **C++**
> - **C 언어에서부터 진화한 프로그래밍 언어.**
> 	- **C++ 은 Superset of C** 이다 (초집합, C++ 은 C 전체를 포함하는 더 넒은 범위를 가짐.)
> 	- **OOP**(Object-Oriented Programming, 객체 지향 프로그래밍) 를 대표하는 성격의 **Class 가 존재.**
> 		- **안전한 초기화 지원 (Guaranteed Initialization)**
> 		- **암시적 형 변환 지원 (Implicit type conversion)**
> 			- 데이터 유형 간 자동 변환.
> 			- 개발자가 명시적으로 형 변환을 지정하지 않아도 언어 또는 컴파일러가 자체적으로 데이터를 필요한 형식으로 변환해준다.
> 		- **메모리 관리 가능**
> 		- **연산자 오버로딩 지원 (Operator Overloading)**
> 		- **다형성 지원 (Polimorphism)**
> 			- 다른 객체들이 동일한 인터페이스를 통해 다양한 방식으로 동작할 수 있는 능력.
> 			- 상속, 추상 클래스, 인터페이스 및 메서드 오버로딩과 같은 다양한 프로그래밍 기술을 통해 구현
> 			- 코드의 유연성을 높이고 재사용성을 증가시킴.
> - **C언어에 존재하지 않는 많은 기능을 지원.**
> 	- **Symbolic Constants (상수 값 정의)**
> 		- **상수 값**을 나타내는데 symbolic constant를 사용.
> 		- 프로그램 내에서 특정 상수 값을 반복해서 사용해야 할 때, 해당 값을 일관되게 참조하고 변경하기 쉽게 만들 수 있다.
> 		- 일반적으로 `#define` 지시어를 사용.
> 			- **`#define PI 3.14159265`**
> 		- `const` keyword 사용 가능 (C++)
> 			- **`const double PI = 3.14159265;`**
> 	- **In-line fuction substitution**
> 		- 컴파일러가 **인라인 함수를 호출하는 부분을 해당 함수의 본문으로 직접 대체**하는 최적화 기술.
> 		- 함수 호출의 **오버헤드를 감소시키고 실행 시간을 최적화.**
> 		- 보통 **짧고 자주 호출되는 함수에 사용.**
> 		- `inline` 키워드를 사용하여 정의
> 			```cpp
> 			 inline int add(int a, int b) {
			    return a + b;
				}
> 			```
> 	- Reference types








## STL Array

- **Vector** : 동적 배열 (자주 사용하게 될 것)

  

## STL strings

- STL 에서 정의한 하나의 자료구조.

  

```
string s = "John"; // s= "John"int i = s.size(); // i = 4char c = s[3]; // c = 'n's += " Smith"; // now s = "John Smith"
```

- string 에는 연산자가 Override 되어 있으므로 연산자 사용 가능

  

## Class

- C 에서 Structure 로 정의하던 것을 C++ 에서는 Class 로 정의하게 된다.

  

## Pointers, Dynamic Memory, “new” Operator

- 동적 할당 = malloc 이 아닌 `new` 를 사용 (더욱 간단해짐).
- `delete` 를 통해 동적할당 해제.
    - 동적 할당된 배열 삭제 시 `delete []` 사용.

  

## References

- 존재하는 이름에 새로운 이름을 부여하는 것.
- 하나의 이름에 두 개의 공간을 부여.
- 값의 복사가 되는 형식이 아닌 넘겨줄 때 해당 방? 의 주소를 알려주는 것.