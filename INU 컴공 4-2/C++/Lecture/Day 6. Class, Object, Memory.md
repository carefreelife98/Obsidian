> **Funtion 은 Class 에 기반하여 한번만 생성된다.**
> - Function을 사용할 때마다 Memory 를 사용하는 것이 아님.

# (중요) Static Class Data
> **Static Class 는 전역 변수처럼 사용되지만, 모든 객체가 사용할 수 있는 것이 아닌, 해당 Static Class 로부터 만들어진 객체만 사용이 가능하다.**

# Const and Classes
> C++ 에서는 **Const 를 함수에 많이 붙여 사용함.**
- 함수의 Signature 에 Const 존재 여부가 중요.
	- **같은 Class 안에 해당 Const 함수와 공존하는 여러 필드 값 (Private 상수 등) 에 접근하여 수정(Write) 불가.**
	- 값을 읽는 것(Read)은 가능하다.
- Class 내에 const 함수를 선언 후 타 위치에서 해당 const 함수를 구현 할 때에도 const 를 붙혀 주어야 한다.
- **Const 는 사용 할 수 있으면 최대한 많이 사용해라.**

```c++
#include <cstdlib>  
#include <iostream>  
  
using namespace std;  
  
class aClass {  
private:  
    int alpha;  
public:  
    void nonFunc(){ alpha = 99; }  
    void conFunc() const;  
};  
  
void aClass::conFunc() const {  
    cout << alpha << endl;  
}
```

```c++
class aClass {  
private:  
    int alpha;  
public:  
    void nonFunc(){ alpha = 99; }  
    void conFunc() const;  
    
	// conFunc2 에서 오류가 발생하는 이유는? / 해결 방법은?
    int* conFunc2() const{  
        return &alpha;
    }  
};
```
- **문제의 원인 :**
	- const Function 은 관련 필드의 값을 변경 할 수 없지만, 해당 필드의 주소를 `* (포인터)` 를 통해 반환하게 되면 외부에서 해당 필드 값을 변경할 수 있는 여지를 주게 된다.
- **해결 방법 :**
	- **Return Type 에도 const 를 붙혀준다.**
	- 해당 **변수(필드)값도 const 값으로 만들어 (상수화) 변경 불가**하게 만들어 준다.
	```c++
// ...

const int* conFunc2() const{  
    return &alpha;  
}

// ...

int main(void){  
    aClass A;  
    const int* temp = A.conFunc2();  

	// *temp = 10;
    cout << *temp << endl;  
}
	```
- 위와 같이 **const function 의 Return Type 도 const 로 설정.**
- 타 위치에서 **해당 const 함수의 return 을 받을 때에도 const type**으로 받아줘야 한다.

## Const Object (상수 객체)
> `const Object ObjectName` 과 같이 **Object를 Const 형식으로 생성** 할 수 있다.
> - 이러한 경우, **생성된 객체 내부의 const member function 만 호출이 가능**하게 된다. (Read Only)
> - const 가 아닌 member 객체를 호출하게 되면 에러가 발생.

### 참고(중요) - Initializing (이상적인 C++ 생성자 생성방법)
> Body 내부가 아닌 Body 외부에 Signature 를 사용하여 생성자 만들기
> - 해당 방법으로 생성자 만드는 것에 습관 들이자.

```c++
// 예시
public:
	Distance(int fit, float in) : feet(ft), inches(in) {}
```

- Member 변수가 상수인  경우 (const) 위와 같이 생성자를 생성해야 해당 변수를 Initialize 하여 사용 할 수 있다.

```c++
//  
// Created by 채승민 on 2023/09/22.
//  
  
#include <cstdlib>  
#include <iostream>  
using namespace std;  
  
class Passenger {  
private:  
    const int age;  
    const string name;  
public:  
    void getField() const {  
        cout << "age: " << age << " & name: " << name << endl;  
    }  
    Passenger() : age(26), name("Chae") {}  
  
//    // Error  
//    Passenger() {  
//        age = 26;  
//        name = "Chae"  
//    }  
};  
  
int main(){  
    Passenger P1;  
    P1.getField();  
}
```


## STL 및 Operator (포인터 객체) - 중요
### STL - String Class
> String이 객체, 클래스 인 것을 생각.
> - 내부 메소드가 존재 할 것이라 생각.
> - 연산자 오버로딩 존재 (문자열 간의 + 연산이 가능.. etc)

```c++
//  
// Created by 채승민 on 2023/09/22.//  
  
#include <cstdlib>  
#include <string>  
#include <iostream>  
  
using namespace std;  
  
int main(){  
    string s = "a dog";  
    s += "is a dog";  
      
    cout << s.find("dog") << endl; // index 2  
  
    // index 11 ( 5번째 이후 index 부터 찾기)  
    cout << s.find("dog", 5) << endl;  
      
    // string::npos == -1  
    if (s.find("doug") == string::npos) // true  
        cout << "true" << endl;  
      
    cout << s << endl;  
    cout << "substr: " <<s.substr(7, 5) << endl;   
      
    s.replace(2, 3, "frog");  
    cout << "replace: " << 5 << endl; //11   
      
	s.erase(6, 3);
	
    s.insert(0, "is ");
}
```

