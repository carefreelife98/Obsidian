---
Author: CarefreeLife98
Date: 2023-10-08T17:32:00
Agenda: |-
  1. Overloading Unary Operators
  2. Overloading Binary Operators
  3. Data Conversion
  4. UML Class Diagrams
  5. Keywords explicit and mutable
tags:
  - CPP
---
# Overloading Unary Operator
## Agenda - Unary Operator
> **대표적인 예시로 증강 / 감소 연산 (++ , --)**
> - 전위, 후위에 사용될 수 있음.<br><br>
> `operator` 라는 Keyword 에 증/감 연산자를 붙혀 사용.
> - 원하는 **객체의 증/감 연산에 대한 "가능성"을 부여**<br><br>
> **중요한 것은 정의와 내부 동작이 동일**해야 한다는 것.
> - ++ Operator 를 구현 하였으나, 내부 동작이 -- 로 실행되면 안된다.

```cpp
// countpp1.cpp  
// increment counter variable with ++ operator  

# include <iostream>  
using namespace std;  
////////////////////////////////////////////////////////////////  
class Counter {  
private:  
    unsigned int count;  // count
public:  
    Counter() : count(0) {}  // Constructor no args 
    unsigned int get_count() {  return count; }  // return count
    void operator ++ ()  
    {  
        ++count;  
    }  
};  
  
int main()  
{  
    Counter c1, c2;                     //define and initialize  
    cout << "\nc1=" << c1.get_count();  //display  
    cout << "\nc2=" << c2.get_count();  
    ++c1;                               //increment c1  
    ++c2;                               //increment c2  
    ++c2;                               //increment c2  
    cout << "\nc1=" << c1.get_count();  //display again  
    cout << "\nc2=" << c2.get_count() << endl;  
    return 0;  
}
```

<br><br>

## Prefix, Postfix (전 / 후위 증강) Unary Operator
> **Prefix Notation**
> - **`Counter operator ++ ()`**
> 	- `++c1;`
> 	<br><br>
> **Postfix Notation**
> - **`Counter operator ++ (int)`**
> 	- `c1++;`
> 	- **(int) 는 Integer 를 뜻하는 것이 아님.**
> 		- Postfix, 후위 증강임을 나타내는 약속.

```cpp
// postfix.cpp  
// overloaded ++ operator in both prefix and postfix  

#include <iostream>  
using namespace std;  
////////////////////////////////////////////////////////////////  
class Counter {  
private:  
    unsigned int count;  
public:  
    Counter() : count(0) {}  
    Counter(int c) : count(c) {}  
    unsigned int get_count() const  
    { return count; }  
    
    // prefix, 전위 증강
    Counter operator ++ ()          //increment count  
    {                               //increment count, then return  
        return Counter(++count);    //an unnamed temporary object  
    }  

	// Postfix, 후위 증강
    Counter operator ++ (int)
    {  
        return Counter(count++);  
    }  
};  
////////////////////////////////////////////////////////////////  
int main() {  
    Counter c1, c2;  
    cout << "\nc1=" << c1.get_count();  
    cout << "\nc2=" << c2.get_count();  
    ++c1;  
    c2 = ++c1;  
    cout << "\nc1=" << c1.get_count();  
    cout << "\nc2=" << c2.get_count();  
    c2 = c1++;  
     
    cout << "\nc1=" << c1.get_count();    //display again  
    cout << "\nc2=" << c2.get_count() << endl;  
    return 0;  
}
```

<br><br>

# Overloading Binary Operators
> **두 객체로 이루어진 피연산자를 갖는 Operator.**
> - 연산자 Overloading 이 가능하며 이를 통해 객체에서도 사용자가 정의한대로 동작하도록 만들 수 있다.
> <br><br>
> **Arithmetic Operators**
> - `dist3 = dist1 + dist2;`
> <br><br>
> **Comparison Operators**
> - `return (bf1 < bf2) ? true : false;`
> <br><br>
> **Arithmetic Assignment Operators**
> - `dist1 += dist2;`
> - `dist3 = dist1 += dist2;`
