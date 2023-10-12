
# Static Class Data
> Gloval Variable 과 유사함.
> 
> **Static Class 내부에 정의된 Object 만 해당 Data 를 사용할 수 있음**
> **-> 전역 변수이지만 접근 제어가 됨.**

```C++
#include <iostream>

using namespace std;

class ship {}
	private:
		angle latitude, longitude;
		static int count;
		unsigned int shipNumber;
	public:
		ship() {
			count++;
			shipNumber = count;	
		}
		void getPosition();
		void display() const;
};

int ship::count = 0; // C++ 에서 count 변수 초기화 하는 방법
```


# Standard Template Library (STL)
## Vector
```C++
#include <cstdlib>

#include <iostream>

#include <vector>

  

using namespace std;

  

enum MealType { NO_PREF, REGULAR, LOW_FAT, VEGETARIAN };

  

class Passenger {

public:

bool isFrequentflyer() const;

void makeFrequentFlyer(const string& newFreqFLyerNo);

private:

string name;

MealType mealpref;

bool isFreqFlyer;

string freqFlyerNo;

};

  

int main(){

vector<int> scores(100);

vector<char> buffer(500);

vector<Passenger> passenList(20); // 20개가 넘어가면 자동으로 스케일 아웃.

  

int i = 10;

cout << scores[i] << endl; // 벡터는 배열처럼 사용이 가능한 장점이 있다.

cout << buffer.at[i] << endl;

buffer.at[2 * i] = 'a';

buffer.at[i] = buffer.at[2 * i];

cout << buffer.at[i] << endl;

vector<int> newScores = scores;

  

cout << scores.size() << endl;

scores.resize(scores.size() + 10);

cout << scores.size() << endl;
}
```

## Iterator
> **C++ 의 Iterator 는 객체이며 그 내부에는 필드가 존재하고, 해당 필드는 포인터 필드이다.**

```C++
#include <cstdlib>
#include <iostream>
#include <vector>

vector<int> temp;

temp.push_back(20);
temp.push_back(50);
temp.push_back(100);
temp.push_back(200);

// iterator 생성 방식.
// 일반적으로 Iterator 로 사용할 벡터 생성 직후에 Iterator 도 생성해둔다.
vector<int> scores(100);
vector<int>::iterator iter_scores;

// iterator 은 포인터 기능을 하는 객체이므로 연산자 오버로딩이 가능하다.
// 따라서 iter_scores++ 이 가능해진다.
// 이 경우, iter_scores++ 은 다음 원소를 가리키게 된다.
for(iter_scores = temp.begin(); iter_scores != temp.end(); iter_scores++){
	cout << *iter_scores << endl;
}
```

- 실행 결과
	![[스크린샷 2023-09-18 오후 2.45.56.png]]

