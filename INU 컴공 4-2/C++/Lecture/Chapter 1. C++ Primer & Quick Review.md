---
Author: CarefreeLife98
Date: 2023-10-22T15:58:00
Agenda:
  - 1. C++ Programming Elements
tags:
  - CPP
---
# Basic C++ Programming Elements
> **C++**
> - C 언어에서 발






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