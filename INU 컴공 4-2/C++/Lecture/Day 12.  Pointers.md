
# Statically and Dynamically typed languages
> **C, C++**
> - **Type Casting** 이 되므로 "약" 타입의 언어이다.

<br><br>
# Function Pointers
**일급 함수 (First Class) 기능**
> JavaScript, Frontend 개발에서 주로 사용하는 개념.
> - 함수를 리턴
> - 함수를 인자 값으로 사용

<br><br>
![[스크린샷 2023-10-16 오후 1.15.54.png]]
> 상수로 선언되어 불변의 속성을 가진 변수에 수정을 시도하여 에러 발생?

<br><br>
# Memory Management : new and delete
> Stack Memory 에 미리 정해 놓은 사이즈가 아니면 에러가 발생.
> - 따라서 우리는 **항상 동적으로 배열의 크기를 할당해야 한다.**
> - 배열의 끝을 나타내는 Null 값까지 생각해서 배열의 크기는 목표 크기보다 +1 하여 설정.

## Memory Management : A String Class using new

#