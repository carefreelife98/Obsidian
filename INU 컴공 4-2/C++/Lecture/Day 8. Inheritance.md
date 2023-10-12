> **Reusability 를 실현하기 위한 방법 중 하나가 상속. (Inheritance)**

# Derived Class and Base Class
> 상속은 기본적으로 변하지 않는 것 (필드, 메소드) 들을 슈퍼 클래스에 담아두고 하위 클래스에서 재활용하여 사용하는 것.


# Overriding
> Polymorphism


# Multiple Inheritance
> **다중 상속은 좋지 않다. (시험)**
> - 모호한 부분이 존재한다.
> - 컴파일러가 에러를 발생시키는 경우 많음.
> 	- Ambiguity in Multiple Inheritance PPT 보기.
> 		- 어떤 것을 상속 받아야 하는지 잘 모르는 문제.
- Public / Private 으로 상속을 받았을 때 어떤 차이점이 있는지? 테스트 해봐라
	- Access 하는 Keyword 차이가 있을 것 같은데...~



# Abstract Class
> 추상클래스로는 객체를 만들 수가 없다.<br>
> 
> **`virtual ~ = 0;`** 
> - 자신을 상속받는 하위 클래스에서 무조건 구현해야 하는 메소드라고 지정.




# 상속을 Class Diagram 으로 표현하기
> 관계의 정의 방식
> Inheritance  == Generalization


# Composition: A Stronger Aggregation
> 굉장히 강력한 Aggregation 관계. 굉장히 중요하다.
> **연관된, 포함된 클래스가 기존 클래스와 생명주기를 함께한다.**



