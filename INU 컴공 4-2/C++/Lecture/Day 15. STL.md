---
Author: CarefreeLife98
Date: 2023-11-17T10:06:00
Agenda: 
tags:
  - CPP
---
# Iterator
> **Iterator == Smart Pointer**
> - Iterartor 사용 시 객체를 변수 (기본형) 다루듯이 사용 가능.
> - Iterator 는 항상 STL Container 와 쌍으로 사용한다.

## iterator 의 for, while 문에서의 사용 ( 꼭 알아두기)
```cpp
//  
// Created by 채승민 on 11/17/23.//  
  
#include "listout.h"  
#include "iostream"  
#include "list"  
#include "algorithm"  
  
using namespace std;  
  
int main(){  
    int arr[] = {2, 4, 6, 8};  
    list<int> theList;  
  
    for (int k = 0; k < 4; k++) {  
        theList.push_back( arr[k]);  
    }  
    list<int>::iterator iter;  
    for (iter = theList.begin(); iter != theList.end(); iter++) {  
        cout << *iter << ' ';  
    }  
    cout << endl;  
      
    iter = theList.begin();  
    while (iter != theList.end())  
        cout << *iter << ' ';  
    cout << endl;  
      
    exit(0);  
}
```

## iterator - find
```cpp
//  
// Created by 채승민 on 11/17/23.//  
  
#include "iostream"  
#include "list"  
  
using namespace std;  
  
int main(){  
    list<int> theList(5); // container  
    list<int>::iterator iter; // iterator  
    int data = 0;  
    for (iter = theList.begin(); iter != theList.end(); iter++) {  
        *iter = data += 2;  
    }  
    iter = find(theList.begin(), theList.end(), 8);  
    if (iter != theList.end()) {  
        cout << "\nFound 8.\n";  
    } else  
        cout << "\n Did not Find 8.\n";  
    return 0;  
}
```

## iterator - copy()
```cpp
//  
// Created by 채승민 on 11/17/23.//  
  
#include "iostream"  
#include "vector"  
#include "iterator"  
  
using namespace std;  
  
int main(){  
    int beginRange, endRange;  
    int arr[] = {11, 13, 15, 17, 19, 21, 23, 25, 27, 29};  
    vector<int> v1(arr, arr + 10);  
    vector<int> v2 (10);  
    cout << " Enter range to be copied (ex: 2 5): ";  
    cin >> beginRange >> endRange;  
    vector<int>::iterator iter1 = v1.begin() + beginRange; // 2개 건너 뛴 값을 가리키고 있음  
    vector<int>::iterator iter2 = v1.begin() + endRange; // 5개 건너 뛴 값을 가리키고 있음  
    vector<int>::iterator iter3;  
      
    // copy(원본 시작 iterator, 원본 끝 iterator, 복사받는 시작 iterator)    iter3 = copy(iter1, iter2, v2.begin());  
    iter1 = v2.begin();  
    cout << " - " << iter3 - v2.begin() << endl;  
    cout << "x" << v2.end() - v2.begin() << endl;  
  
    while (iter1 != iter3) {  
        cout << *iter1++ << ' ';  
    } cout << endl;  
  
    return 0;  
}
```

## iterator - reverse
```cpp
//  
// Created by 채승민 on 11/17/23.//  
#include "iostream"  
#include "list"  
  
using namespace std;  
  
int main(){  
    int arr[] = {2, 4, 6, 8, 10};  
    list<int> theList;  
    for (int j = 0; j < 5; j++) {  
        theList.push_back(arr[j]);  
    }  
    list<int>::reverse_iterator revit;  
    revit = theList.rbegin();  
    while (revit != theList.rend()) {  
        cout << *revit++ << ' ';  
    }  
    cout << endl;  
      
    // 아래와 같이 for 문의 파라미터를 거꾸로 사용하면 reverse iterator 기능을 충분히 할 수 있다.  
    // 따라서 reverse iterator 는 잘 사용하지 않음.  
    list<int>::iterator it;  
    for (it = theList.end(); it != theList.begin(); it--) {  
          
    }  
    
    return 0;  
}
```
> **일반적인 iterator 의 for 문 파라미터를 내림차순으로 사용하면 reverse iterator 의 기능을 충분히 해준다.**
> - 따라서 reverse iterator 는 잘 사용하지 않는다.

<br><br>

## iterator - inserter
```cpp
//  
// Created by 채승민 on 11/17/23.//  
#include "iostream"  
#include "deque"  
#include "algorithm"  
  
using namespace std;  
  
int main(){  
    int arr1[] = {1, 3, 5, 7, 9};  
    int arr2[] = {2, 4, 6};  
    deque<int> d1;  
    deque<int> d2;  
  
    for (int i = 0; i < 5; i++) {  
        d1.push_back(arr1[i]);  
    }  
    for (int j = 0; j < 3; j++) {  
        d2.push_back(arr2[j]);  
    }  
  
    copy(d1.begin(), d1.end(), back_inserter(d2));  
//    copy(d1.begin(), d1.end(), front_inserter(d2));  
//    copy(d1.begin(), d1.end(), inserter(d2, d2.begin()));  
  
    cout << "\nd2: ";  
      
    for(int k = 0; k < d2.size(); k++) {  
        cout << d2[k] << ' ';  
    }cout << endl;  
  
    return 0;  
}
```

![[스크린샷 2023-11-17 오전 10.48.23.png]]
> copy(d1.begin(), d1.end(), back_inserter(d2));

![[스크린샷 2023-11-17 오전 10.48.38.png]]
> copy(d1.begin(), d1.end(), front_inserter(d2)); 

![[스크린샷 2023-11-17 오전 10.48.53.png]]
> copy(d1.begin(), d1.end(), inserter(d2, d2.begin()));  


# Associative Container
> Container 는 List / deque 등과 같은 선형 자료 구조를 위해 주로 사용.
> - 비선형 자료구조 (set, map) 등을 위한 Associative container 이 존재한다.

## map
> **[중요] map 을 사용하는 방법**
> - **for 문에서 map 을 초기화** 하는 부분을 잘 살펴보자.

## multimap
> **중복을 허용하는 map.**
> - 중복 값을 검출하지 않고 별도로 관리한다.
> - **[중요]** multimap.insert(**make_pair(key, value)**) 형태를 많이 사용.


> Hash 수업 x
> ![[스크린샷 2023-11-17 오전 11.11.57.png]]


# Assignment
> setpers.cpp 
> - person class 를 만든다.
> - person 의 field 를 구성.
> - 각 person 을 multiset 을 이용하여 set 구현
> - **person 의 이름을 넣으면 전화 번호가 나오는 전화번호부 구현하기.**

