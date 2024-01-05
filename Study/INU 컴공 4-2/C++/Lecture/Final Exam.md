---
Author: CarefreeLife98
Date: 2023-12-13T19:24:00
Agenda: 
tags:
  - CPP
---
# Intro
## Basic C++ Programming Elements
> **C++ 은 C 의 모든 것을 포함하며 객체 지향적인 상위 버전.**
> - Guaranteed initialization
> 	- 보장된 초기화
> - Implicit type conversion
> 	- 암시적 형변환
> - Control of memory management
> 	- 메모리 제어
> - Operator overloading
> 	- 연산자 오버로딩
> - Polymorphism
> 	- 다형성 (오버라이딩, 오버로딩)
> 
> <br>
> **이외에도 C 에서 지원하지 않는 많은 기능을 지원한다.**
> - Symbolic constants
> - In-line function substitution
> - Reference types
> - Parametric polymorphism through templates
> - Exceptions

<br><br>

# C++ Quick Review
## Enumeration
> **Enumerations**
> - 유저가 정의한 자료형. 모든 추상적인 값을 가질 수 있음.

```cpp
enum Day { SUN, MON, TUE, WED, THU, FRI, SAT };
enum Mood { HAPPY = 3, SAD = 1, ANXIOUS = 4, SLEEPY = 2 };

Day today = THU;
Mood myMood = SLEEPY;
```

## C++ Array
```cpp
int a[] = { 10, 11, 12, 13 }; // a[4] 의 선언 및 초기화
bool b[] = { false, true };
char c[] = { 'c', 'a', 't' }
```
> - **한번 선언하면 이후 해당 array 의 크기를 수정할 수 없다.**
> - **선언과 동시에 초기화**가 가능하다.

<br>
```cpp
#include "iostream"  
using namespace std;  
  
char c[] = {'c', 's', 'm'};  
char *p = c;  
char *q = &c[0];  
  
int main() {  
    // mmm 출력.  
    cout << c[2] << p[2] << q[2];  
}
```
> - **array 의 이름은 array 의 첫번째 요소를 가리키는 Pointer 와 동등함.**
> - **배열을 가리키는 변수를 지정할 때에는 당연히 Pointer 형**으로 지정.

**`배열을 사용할 때에는 되도록 C++ Standard Template Library 인 vector 를 사용하자.`**

<br><br>

## C++ Vector
> - C++ Standard Template Library 의 **Vector 자료형을 사용하면 배열의 크기를 동적으로 변경 할 수 있다.**

<br><br>

## STL Strings
```cpp
#include "iostream"  
using namespace std;  
  
int main(){  
    string s = "to be";  
    string t = "not " + s;  
    string u = s + " or " + t;  
  
    if (s > t) {  
        // to be 가 not to be 보다 사전 상 앞에 있습니다.  
        cout << s << " 가 " << t << " 보다 사전 상 앞에 있습니다." << endl;  
        // to be or not to be  
        cout << u << endl;  
    }  
}
```
> **STL Strings**
> - Header file 인 **\<string> 을 Include** 해야 사용할 수 있다.
> - **std namespace 에 포함**되어 있으므로 **`using namespace std;`** 선언시 include 불필요.
> - 문자열 사용 시 편리.
> - **연산자 사용 가능**
> 	- 문자열 합치기 : +
> 	- 비교 연산자 (<, >) : 사전 순서에 기반한 비교 수행
> 	- == : 두 문자열이 동일한지 비교 수행

<br><br>

## Pointers, Dynamic Memory, "new" Operator
> **new Operator**
> - **객체 (Object) 생성 시 필요에 따라 동적으로 생성하는 방법.**
> 	- C++ Runtime system 이 대량의 Heap Memory 를 예약해두는 이유.
> - **new 연산자는 주어진 타입의 객체가 필요로 하는 양의 저장소를 보유하고 있는 Heap Memory 로부터 동적으로 할당한 후 해당 객체에게 Pointer 를 반환한다.**
> - new 사용하여 객체 생성 시 정의 해줘야 할 것
> 	- destructor
> 		- 객체에 할당된 메모리 반환을 위해서
> 	- copt constructor
> 		- 객체의 생성과 함께 해당 객체가 가지고 있는 Member 값도 복사하기 위해서.
> 	- assignement operator (=)
> 		- 오래된 저장 공간을 해제, 새로운 저장 공간 할당 및 멤버 변수의 복사를 위해서.

```cpp
#include "iostream"  
  
using namespace std;  
  
enum MealType { NO_PREF, REGULAR, LOW_FAT, VEGETARIAN };  
  
struct Passenger {  
    string name;  
    MealType mealPref;  
    bool isFreqFlyer;  
    string freqFlyerNo;  
};  
  
int main(){  
    Passenger *p;  
  
    // p 는 new Passenger 를 가리킴. (new 에 의해 반환된 포인터)  
    p = new Passenger;  
  
    // -> 를 통해 객체의 속성에 접근  
    p->name = "CarefreeLife98";  
    p->mealPref = NO_PREF;  
    p->isFreqFlyer = false;  
    p->freqFlyerNo = "NONE";  
  
    cout << p->name + ' '  
        << p->mealPref << p->isFreqFlyer << p->freqFlyerNo << endl;  
};
```

<br><br>

> **delete Operator**
> - **객체를 소멸시키고 해당 객체에게 할당된 메모리 공간을 Heap Memory 에게 반환한다.**
> 	- C++ 에는 Java 에 존재하는 Garbage Collect 기능이 없다.

```cpp
delete p; // p 가 가리키고 있는 객체를 소멸시키고 할당된 메모리를 반환한다.
```

<br><br>

> **new 를 사용한 배열 할당**
> - **배열의 첫번째 원소를 가리키는 포인터를 반환한다.**
> <br>
> **delete[]** 를 통해 배열의 삭제 및 메모리 반환이 가능함.

```cpp
int main(){  
    char *buffer = new char[500];  
    buffer[3] = 'a';
    
    // buffer[3] = a 출력
    cout << "buffer[3] = " << buffer[3] << endl;  
    
    delete[] buffer; // 배열 buffer 삭제 및 할당된 메모리 반환
}
```

<br><br>
> **메모리 누수를 막기 위해 new 를 통해 생성한 객체는 반드시 delete 로서 지워주어야 한다.**

<br><br>

## References &
> **객체와 같은 Type 의 변수 앞에 & 를 붙여 선언 후 동일한 Type 의 객체를 할당하면 해당 객체를 직접 참조, 수정 및 변경 할 수 있게 된다.**

```cpp
#include "iostream"  
  
using namespace std;  
  
int main() {  
    string DevOps = "Unknown";  
    // Who is DevOps? Unknown  
    cout << "Who is DevOps? " << DevOps << endl;  
  
    // reference 생성 (me)    
    string& me = DevOps;  
    me = "Carefreelife98";
      
    // Now, Who is DevOps? Carefreelife98  
    cout << "Now, Who is DevOps? " << DevOps << endl;  
}
```

<br><br>

## Named Constants, Scope and Namespaces
**Keyword : Const**
> **변수의 Type 앞에 `Const` 선언 시 변경 불가능한 상수화가 된다.**
> - const 변수는 **선언 시 반드시 초기화**를 해야 함.
> <br>
> **const 포인터 변수**
> const 포인터 변수를 사용하는 경우 두 가지
> 1. const 위치가 가장 앞에 있으면서 **포인터 변수가 가리키는 값에 대하여 상수화를 시키는 경우**


## Argument Passing
```cpp
//  
// Created by 채승민 on 12/14/23.
//  
#include "iostream"    
using namespace std;  

// value = 값의 전달 (원본 영향 x), ref = 값의 참조(원본에 영향)
void func(int value, int &ref){  
    value++;  
    ref++;  
    cout << value << endl;  // 2
    cout << ref << endl;  // 6
}  
  
int main() {  
    int cat = 1;  
    int dog = 5;  
    func(cat, dog);  
    cout << cat << endl;  // 1 : func 함수에서 값을 복사하여 사용
    cout << dog << endl;  // 6 : func 함수의 ref 로 인해 원본 변경
  
    return EXIT_SUCCESS;  
}
```

> **함수의 인자에 & operator 를 붙여 해당 인자의 원본을 참조 할 수 있다.**
> - default 는 인자 값이 복사되어 함수 내부로 전달되지만, & operator 사용 시 값의 참조가 일어나 인자의 원본을 변경 할 수 있게 된다.


## STL (Standard Template Library)

![[스크린샷 2023-12-15 오전 6.07.34.png]]
![[스크린샷 2023-12-15 오전 6.07.44.png]]
![[스크린샷 2023-12-15 오전 6.08.02.png]]



# Iterator Linked List
![[스크린샷 2023-12-15 오전 6.08.40.png]]
![[스크린샷 2023-12-15 오전 6.08.50.png]]![[스크린샷 2023-12-15 오전 6.09.30.png]]