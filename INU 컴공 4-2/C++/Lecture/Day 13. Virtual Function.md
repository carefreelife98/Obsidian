---
Author: CarefreeLife98
Date: 2023-10-27T10:32:00
Agenda:
  - 1. Polymorphism
tags:
  - CPP
---
# Polymorphism 
```c++
//  
// Created by 채승민 on 10/27/23.
//  
  
#include "iostream"  
using namespace std;  
  
class base {  
public:  
    virtual void show(){cout << "Base" << endl;}  
};  
  
class A: public base {  
public:  
    virtual void show() {cout << "A" << endl;}  
};  
  
class B: public base {  
public:  
    virtual void show(){cout << "B" << endl;}  
};  
  
int main(){  
    base *ptr[2];  
    
    // 각각의 Function 주소를 배열에 저장.
    // virtual keyword 가 붙어 있어서 다형성을 구현 가능.
    ptr[0] = new A(); ptr[1] = new B();  
    for (int i = 0; i < 2; i++) {  
        ptr[i]->show();  
    }  
};
```
> **다형성**
> - Function 의 외형은 같은 Signature 을 가지지만, 실제로 실행되는 동작은 다른 동작.
> - virtual 이라는 keyword 를 반드시 부모 class 의 function 에 지정해주어야 한다.

```cpp
//  
// Created by 채승민 on 10/27/23.
//  
  
#include "iostream"  
using namespace std;  
  
class baseWithNotVirtual {  
public:  
    void show(){cout << "Base" << endl;}  
};  
  
class Derv1: public baseWithNotVirtual {  
public:  
    void show() {cout << "A" << endl;}  
};  
  
class Derv2: public baseWithNotVirtual {  
public:  
    void show(){cout << "B" << endl;}  
};  
  
int main(){   
    Derv1 dv1;  
    Derv2 dv2;  
    baseWithNotVirtual* ptr;  
      
    ptr = &dv1;  
    ptr->show();  
    ptr = &dv2;  
    ptr->show();  
};
```
> **위와 같이 Virtual keyword 를 사용하지 않는다면 절대 객체 기준으로 함수가 호출되지 않는다.**
> - **virtual keyword 사용시 객체 기준으로 함수를 호출함. (Dynamic / Late binding)**
> - virtual keyword 가 없다면 **Type 기준으로 함수를 호출**하게 된다.
> 	- 따라서 위 코드의 실행 결과는 "Base" 두 번 출력.

<br><br>
## Abstract Classes and Pure Virtual Functions
> **`virtual ~ () = 0;`**
> - **virtual keyword 함수에 = 0 을 할당하여 추상 클래스로 지정 가능.**
> - **`abstract`** keyword 도 사용 가능.

```cpp
//  
// Created by 채승민 on 10/27/23.
//  
  
#include "iostream"  
using namespace std;  
  
class base {  
public:  
    virtual void show() = 0;  
};  
  
class Derv1: public base {  
public:  
    void show() {cout << "Derv1" << endl;}  
};  
  
class Derv2: public base {  
public:  
    void show(){cout << "Derv2" << endl;}  
};  
  
  
int main(){  
    // Base bad; // 추상 클래스이기 때문에 객체 직접 생성은 불가능.
    base* arr[2];  
    Derv1 dv1;  
    Derv2 dv2;  
  
    arr[0] = &dv1;  
    arr[1] = &dv2;  
    arr[0]->show();  
    arr[1]->show();  

	return 0;
};
```
> 최근에는 **`abstract`** keyword 도 사용 가능하다.

<br><br>
## Virtual Destructor
```cpp
//  
// Created by 채승민 on 10/27/23.
//  

#include "iostream"    
using namespace std;  
  
class Base {  
public:  
    ~Base(){ // virtual ~Base() // virtual destructor  
        cout << "Base Destroyed\n" ;  
    }  
};  
  
class Derv: public Base {  
public:  
    ~Derv(){cout<<"Derv Destroyed\n";}  
};  
  
int main(){  
    Base *pBase = new Derv;  
    delete pBase;  
    return 0;  
}
```
- Virtual Keyword 를 붙혀 주어야 부모 클래스와 자식 클래스 모두의 소멸자를 호출 할 수 있다.
<br><br>
## Friend Functions
> **서로 다른 Class 내의 Data 에 접근 가능하도록 특정 Class 에게 Friend keyword 를 통해 권한을 준다.**
> <br>
> 잦은 사용을 권하지 않음. (OOP 철학을 위반하게 됨)

![[스크린샷 2023-10-30 오후 1.18.46.png]]

# Doubly Linked List

# Iterator
> 순회하는 것에 사용


# this Pointer
> **this Pointer**
> - 자기 자신 객체의 주소 값이 저장되어 있음.

```kotlin
#include <iostream>  
using namespace std;  
  
class what {  
private:  
    int alpha;  
public:  
    void tester() {  
	    // 이렇게 쓰지 않아도 되지만 this 를 통해 자기 자신의 속성에 접근할 수 있다는 것을 알 수 있음. 
        this->alpha = 11;
        cout << this->alpha;  //same as cout << alpha;  
    }  
};  
  
int main() {  
    what w;  
    w.tester();  
    cout << endl;  
    return 0;  
}
```