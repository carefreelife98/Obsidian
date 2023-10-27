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
    ptr[0] = new A(); ptr[1] = new B();  
    for (int i = 0; i < 2; i++) {  
        ptr[i]->show();  
    }  
};
```
