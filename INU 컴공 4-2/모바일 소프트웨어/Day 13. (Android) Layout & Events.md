---
Author: CarefreeLife98
Date: 2023-10-10T16:01:00
Agenda: |-
  1. Layout
  2. Event Handler
tags:
  - Android
  - Kotlin
---
# Layout : Part 1
## Layout

> **Layout : View (ViewGroup) 의 위치 & 크기**를 지정
> 	- `ViewGroup` = `Layout`
> 		- View 및 ViewGroup 을 담을 수 있는 **Container**
> 	- `View` = `widget`
> 		- **미리 만들어 놓은 뷰 (View)**


## View = 계층 구조
> **View 는 계층 구조**로 이루어짐.
> <br>
> - **Android 화면 (외부) = UI (User Interface)**
> 	- user 로부터 **입력을 받아 처리한 결과**를 보여줌.
> <br>
> - **Android 화면 (내부) = 계층 구조 (Hierarchical Structure)**
> 	- view 는 자신을 관리하는 ViewGroup 에 포함.
> 		- **ViewGroup 은 Child node 에 해당하는 view** 를 가짐.

<br><br>

# Android In-Class
## Palette
> **Text**
> - **TextView 는 Read-Only.**
> 	- Read 만 가능하며, Write 불가.
> 	- TextView 를 제외한 **나머지 Template 은 전부 Read & Write 가능.**
> <br>
> **Google**
> - Google Map 과 같은 기능 사용 가능.
> <br>
> **Legacy**
> - 오래된 Templates.
> - **RelativeLayout 을 제외하고 잘 사용하지 않는다.**

<br><br>

## Design
### Declared Attribute

> 사용자가 직접 정의한 속성을 볼 수 있음.

<br><br>

# Linear Layout (시험)
> 가장 중요한 속성은 **Orientation**


![[스크린샷 2023-10-10 오후 7.25.55.png]]


![[스크린샷 2023-10-10 오후 7.26.04.png]]

## Layout Weight
![[스크린샷 2023-10-10 오후 7.29.14.png]]
> 해당 Component 의 차지 비율을 Orientation (Horizontal / Vertical) 기준에 맞춰 설정.
> - 일반적으로 정수 타입의 비율로 설정. (1:2:1)..

<br><br>

# Relative Layout
> **모든 Layout 에서 가장 중요한 것은 View 의 Id 속성.**
> - Id 를 통해 Layout 과 Kotlin Code 가 상호 작용하여 Event 처리를 한다.
> <br>
> **Relative Layout**
> - 어떤

<br><br>

1. 문자열 출력을 위한 Text View 배치
	<br>
2. Layout Margin 부여
	![[스크린샷 2023-10-12 오후 6.12.19.png]]
	<br>
2. **PlaintText 배치**
	- **XML Code 내에서는 EditText 라는 이름으로 생성됨 (중요)**
	<br>
3. **layout_below 옵션 설정**
	![[스크린샷 2023-10-12 오후 6.16.52.png]]
	- 어떤 View 밑에 붙혀 추가 할 것인지 설정하는 옵션.
	<br>
4. **Alignment 옵션 설정**
	![[스크린샷 2023-10-12 오후 6.23.39.png]]

<br><br>
# Table Layout
![[스크린샷 2023-10-12 오후 6.25.41.png]]
> **다루기 매우 쉬운 Layout.**

<br>

1. **Table Row 추가.**
	![[스크린샷 2023-10-12 오후 6.26.22.png]]
	<br><br>
2. **Table Row 에 textView, PlainText 배치.**
	![[스크린샷 2023-10-12 오후 6.29.06.png]]
	<br><br>
3. **stratchColumn, layoutColumn, spanColumn속성 추가**
	![[스크린샷 2023-10-12 오후 6.33.17.png]]
	![[스크린샷 2023-10-12 오후 6.33.55.png]]
	![[스크린샷 2023-10-12 오후 6.34.13.png]]
	<br><br>

<br><br>

# FrameLayout
> **버튼을 누를 때 마다 이미지 변환 발생.**

<br>
1.  **ImageView 배치**
	![[스크린샷 2023-10-12 오후 6.39.08.png]]
	- ImageView - Common Attribute - srcCompat 위치에서 이미지 선택.
	<br><br>
2. **adjustViewBounds 속성**
	- True 로 설정 시 View
	<br><br>
3. **visibility 속성 설정**
	![[스크린샷 2023-10-12 오후 6.43.25.png]]
	- visible : 보임
	- Invisible : 보이지 않음
	- Gone : 이미지 뷰가 차치하는 공간 자체가 사라짐.
	<br><br>
4. **Code 추가 (모듈러스 연산)**
	```kotlin
fun onClicked(view: View) {  
    val imageView = findViewById<ImageView>(R.id.imageView)  
    val imageView2 = findViewById<ImageView>(R.id.imageView2)  
    val imageView3 = findViewById<ImageView>(R.id.imageView3)  
  
    // 모든 view 초기화 - INVISIBLE    imageView.visibility = View.INVISIBLE  
    imageView2.visibility = View.INVISIBLE  
    imageView3.visibility = View.INVISIBLE  
  
    count += 1  
    when (count % 3) {  
        0 -> imageView.visibility = View.VISIBLE  
        1 -> imageView2.visibility = View.VISIBLE  
        2 -> imageView3.visibility = View.VISIBLE  
    }  
}
	```
	




![[스크린샷 2023-10-10 오후 4.05.14.png]]
> **Landscape Mode**
> - 가로 화면
> 
> **Portrait Mode**
> - 세로 화면

## Constraint
![[스크린샷 2023-10-10 오후 4.23.06.png]]
> Constraint 는 최대 4개 ~ 최소 2개 까지 생성 가능.

<br><br>
# 3 ways to build an Event Handler
## Event Handler 를 내부 클래스 (Inner Class) 로 정의



## Anonymous Class (무명 클래스) 를 사용

## Activity Class 에서 Listener Interface 를 상속 (구현 상속)

