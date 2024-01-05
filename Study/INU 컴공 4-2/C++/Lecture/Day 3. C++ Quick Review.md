  

**Call By Reference**

- C++ 의 주요한 특징 중 하나.
- 값의 복사가 일어나지 않으므로 시스템 측면에서 효과적이다.

  

**Const**

- Read Only
- Immutable

  

**Function Overloading**

- 함수의 Name 이 같아도 input (Parameter) 의 값에 따라 다르게 동작한다.
- 다형성

  

## Class Structure

- C 의 Structure(구조체)는 문법적으로 결합되어 있는 것이 아님.

  

**Private**

- 클래스 내부에 Public으로 설정된 함수에 의해서만 사용하겠다.
- Bation Server (Public) 을 통해 Private Subnet 접근하는 느낌

## Class 사용 예제

```
\#include <cstdlib>#include <iostream>using namespace std;class Counter { // a simple counterpublic:	Counter(); // initialization	int getCount(); // get the current count	void increaseBy(int x); // add x to the countprivate:	int count; // the counter¡¯s value};int main() {	Counter ctr;	cout << ctr.getCount() << endl;	ctr.increaseBy(3);	cout << ctr.getCount() << endl;	ctr.increaseBy(5);	cout << ctr.getCount() << endl;}Counter::Counter() { count = 0;}int Counter::getCount() { return count; }void Counter::increaseBy(int x) { count += x; }
```

  

# Big Picture & Warm Up

**OOP (Object Oriented Programming)**

- 객체 지향
- Data Encapsulation & Data Hiding
- TDD

  

**OOP 언어의 특징**

- Objects
- Classes
- Reusability
- Creating New Data Types
- **Polymorphism** and Overloading
    - 다형성
    - 똑같이 생겼으나 만나는 객체에 따라 다른 기능을 한다.

  

**상속의 문제**

1. 코드가 계속 중복됨.
2. 코드의 재사용이 어려워짐.

  

**Solution**

- **변경되는 부분과 변경되지 않는 부분을 확실히 구분하자.**
    - **바뀌는 부분들만 뽑아서 캡슐화**
    - 인터페이스로서 구현

  

클래스 내부에 인터페이스가 들어가게 되면 추상 클래스가 된다.

  

업캐스팅

  

Composition - 객체가 객체를 포함하는 것 (상속 X)

- Flexibility 측면에서 상속보다 좋음.