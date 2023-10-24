  

# Code - MainActivity (1/6)

  

`:`

- Class 에서의 : 은 상속을 의미.
- override 에서의 : 은 Parameter 의 Type을 의미.

  

# XML

- Root Tag 에만 Namespace 정의 가능.
    
    - Tag 이름 다음에 여러가지 속성을 정의. (= View 의 속성)
    
      
    

# Device Manager

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-14_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.11.53.png]]

- 장치를 선택하는 기준 (적당히 선정하는 것이 중요)
    - 크기
        - `"` : 인치를 나타냄.
    - Resolution
        - 사각형의 너비와 높이
        - 너비 부터 선정해야함. 그 이후 높이 설정
            - EX) 1080px X 1920px (Width X Height)
    - Density (밀도)
        
        - 밀도가 높을 수록 더욱 풍부한 화면 표현 가능.
        
          
        

## ABI

- ISO 파일?
- Android OS Image 와 같은 느낌인듯

  

## Verify Configuration

- Enable Keyboard Input

  

# Assignment

  

## Attribute Tab

- Button 추가 시 우측의 Attribute Tab 에서 XML Tag 로서 추가됨.
    - Button 우클릭 → Center → Horizontally in Parent
    - Constarint 지정 가능 및 오류 해결됨.
- `ID`
    - `Scope`
        - 변경 사항의 적용 범위 설정
- 문자열 Resource 로 리소스 관리하기
    - `Declared Attributes`
        - 내가 설정한 값들의 모음.
        - 각 리소스 별 가장 오른쪽의 아이콘 선택.
            - `+` → `String Value`
                - Resource name (Key)
                - Resource Value (Value)
                - `File name : strings.xml`
        - XML 파일 참조 시 코드
            - `@string/(Resource name)`
            - `/app/java/res/values/strings.xml` 에서 확인 가능.
- Button 에 함수 추가하기 (JS 스타일)
    - 순서
        - Button 선택
        - 돋보기로 onClick 검색
        - 이름 추가후 Enter
        - xml 파일에 등록된 것 확인 및 해당 이름 클릭 후 전구 선택 및 Create
    - Button 누를 시 TextView 변경하기
        
	```kotlin
// @+id/myTextViewmText.text = "안녕, 안드로이드 13~"	
val mText = findViewById<Textview>(R.id.myTextView)
	```