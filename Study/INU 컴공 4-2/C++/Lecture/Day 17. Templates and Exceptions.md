---
Author: CarefreeLife98
Date: 2023-11-27T13:10:00
Agenda: 
tags:
  - CPP
---
> **Template**
> - 여러 가지 Type 을 사용하여 객체를 사용 할 수 있도록 해줌.

# Function Template
> int, long, float 에 대한 절댓값을 구하는 함수 작성 시
> - 각 Data type 별로 다른 함수를 작성해야함
> - **이 경우 Call 과 동시에 Type 을 명시하여 사용 할 수 있는 함수가 Template.**

```cpp
template <class T>
T abs(T n) {
	return (n < 0) ? -n : n;
}
```

> 사용자 정의 자료 구조를 만들 때에는 무조건 Template 을 사용해서 만들어야 한다.