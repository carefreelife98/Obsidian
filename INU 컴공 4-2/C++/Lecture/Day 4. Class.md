  

# Class 생성 기본 틀

```
class Name {private:	int a;	string name;public:	Name();	~Name();};Name::Name(){	a = 10;	name = "Carefree";};
```

- Public 에는 보통 Method 노출.
- Private 에는 보통 필드(변수) 작성.

  

## 생성자

- 객체가 생성될 때에 해당 함수의 필드값을 초기화 해주는 역할.
- 객체 생성 시 방법은 많음.
    
    ```
    class Passenger {public:	Passenger();															// default constructor	Passenger(const string& nm, MealType mp, const string& ffn = "NONE");	// constructor given member values	Passenger(const Passenger& pass);										// copy constructor	bool isFrequentFlyer() const;											// is this a frequent flyer?	void makeFrequentFlyer(const string& newFreqFlyerNo);private:	string name;	MealType mealpref;	bool isFreqFlyer;	string freqFlyerNo;};int main() {	Passenger p1;										// default constructor	Passenger p2("Gi Seok Park", NO_PREF, "72527988");	// 2nd constructor	Passenger p3("Sungjin Ryu", REGULAR);				// not a frequent flyer	Passenger p4(p3);									// copied from p3	Passenger p5 = p2;									// copied from p2	Passenger* pp1 = new Passenger;						// default constructor	Passenger* pp2 = new Passenger("Joe Blow", NO_PREF);// 2nd constr.	Passenger pa[20];									// uses the default constructor}
    ```
    
    - 생성자에 변수 필드를 할당해주고, 메인 함수에서 해당 객체 생성시에 변수를 넣어 생성할 수 있다.
        - `p2, p3`
    - 또한 생성 시 Default 값으로 초기화 하도록 생성자를 작성할 수 있다.
        - `p2`
    - 일반적으로 사용하는 경우
        - `p1, p2, p3`

  

## 소멸자

```
class Vect {		// a vector classpublic:	Vect(int n);	// constructor, given size	~Vect();		// destructorprivate:	int* data;		// an array holding the vector	int size;		// number of array entries};Vect::Vect(int n) {		// constructor	size = n;	data = new int[n];	// allocate array}Vect::~Vect() {		// destructor	cout << "delete" << endl;	delete[] data;	// free the allocated array}
```

- Class 를 통해 Object를 만들고 `해당 Object 가 죽을때에 실행됨.`
- `소멸자를 호출 하는 유일한 경우는 동적 할당이 되어 있을 때.`
    - 객체 생성시 동적 할당 받은 Heap 영역 메모리를 해당 객체가 죽을때에 반환해준다.
        - `delete[] (객체이름)`
    - 일반적으로 해당 함수 내에 선언만 해두고 실제 메소드는 해당 클래스 바깥에 작성한다.
    - 이때, 해당 클래스를 명시 해준 후 `::` 뒤에 해당 소멸자를 추가해주어 함수를 작성한다.

  

# Header 설정 이유

- 헤더(\#include / \#ifndef .. / \#endif)
    - 여러 cpp 파일에서 동일한 헤더 사용 시 추후 모듈화 할 시 같은 헤더를 가진 여러 cpp 파일로 인해 충돌 발생 가능.

  

# 추상 클래스 생성하기

```
class Ifly{public:	virtual void fly(){} // 추상 클래스 생성 - virtual};
```

- 추상 클래스
    - 해당 객체 사용 시 무조건 구현해야 하는 메소드.
    - Java 의 Interface 와 비슷한 기능을 하게 된다.

  

  

# QnA

- C++은 메인 함수 위에 모든 메소드들이 정의되지 않아도 되는지?