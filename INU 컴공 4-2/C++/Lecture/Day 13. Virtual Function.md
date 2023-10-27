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