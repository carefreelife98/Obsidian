  

## 1. Activity 선정

- 가장 먼저 생성 할 것.

  

## 2. Project 및 SDK Setting

- Package 이름은 Unique 한 것 사용.
    - Play Store에 올라갈 이름.

  

## 3. Version

**같은 Android Patform Version을 가지더라도 사용하는 API Level 버전이 다를 수 있다.**

- Android Platform Version
    - 주 버전
- API Level
    - 부 버전 (상세)

  

# 시작하기

  

1. Android studio
2. More actions
    1. SDK
    2. Google Atom System Image
        - 가상 기기에서 사용 할 수 있도록 해줌.
    3. SDK Tools → Google Play Service 꼭 체크해주기
    4. 실물 폰 테스트용도로 사용시 Google USB Driver 체크.
3. New Project
    1. Activity → Phone and Tablet → Empty Views Activity 선택
        1. Name : 대문자 및 띄어쓰기 사용 가능
        2. 하지만 Package Name 에서 대문자는 소문자로, 띄어쓰기는 삭제되어 삽입된다.
        3. **Minimum SDK**
            1. 아래 나오는 Percentage 는 높을수록 대중화 될 수 있다는 의미.(Downward Compatibility)
            2. API 25 (7.1.1)
            3. Kotlin DSL (Recommended)
        4. 디버깅 시 빨간 점 → Toggle 이라고 함
        5. 편집창 → Design and BluePrint
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.59.28.png]]
            
            - 화면 보기 설정 (취향대로)
            - 그 오른쪽 화면 설계 시 Portrait / Landscape 눌러보기
            - 그 아래 눈알 아이콘 → Show System UI = 실제 폰 화면처럼 보기
        6. 단축키
            
            - `Ctrl + Tab` : Android Studio 내의 창 전환 (Ctrl 누르고 있어야함)
            - `Cmd + e` : 최근 창 전환
            
              
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.12.10.png]]
            
            - 안드로이드 작업 시에는 View 를 Android 에서 보는 것이 간결하다.
            
              
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.13.52.png]]
            
            - Manifest File
            
              
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.15.56.png]]
            
            - 우리가 작성한 코드는 Java Directory 아래에 존재.
            - res → layout 에는 activity_main.xml 존재.
            
              
            
    2. Package
        - 우리가 만든 모든 것이 Package 에 들어있다.
        - 고유한 이름을 사용.
            - `도메인이름.프로젝트이름`
    3. import
        
        - `패키지이름.클래스이름`
        - 외울 필요 X (IDE 에서 자동 생성 해줌)
        - Androidx / Android
            - `androidx : extension (확장)`
            - `android : 초창기 버전부터 존재하던 패키지.`
        - Import 문의 가장 마지막 문을 보면 대강 어떤 코드인지 알 수 있다.
        - `compat` : 호환 가능하다는 의미
        
          
        
    4. Class
        - Class 이름은 곧 File의 이름.
            - Class 이름과 File 이름을 맞춰주어야 에러 발생하지 않음.
        - 클래스 이름 옆의 `:` 은 상속을 나타냄 (`= extends`)
        - 클래스 상속 과 인터페이스 상속
            - `:` 옆의 부모 클래스에 `()` 가 존재하면 클래스 상속이다.
                - `()` 는 생성자 호출을 나타냄.
                - 클래스 상속은 부모가 하나 밖에 없다. (단일 상속)
            - 클래스 상속 단축키
                - `부모 클래스 클릭 후 Ctrl + h`
                    
                    - 상속 관계 확인 가능
                    
                      
                    
        - `override fun ..`
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.30.20.png]]
            
            - 부모 클래스로부터 상속받은 method 를 자식 클래스에서 재정의.
            - `fun` : Function 을 나타냄.
            - `onCreate(이름 : Type)`
                - Former Parameter (형식인자)
            - `?`
                - null safe - Null 값일 수도 있다는 것을 나타냄. (NullPointerException 예외 처리)
            - `. 표기법`
                - 객체의 method 나 property 를 호출하는 문장.
            - `super`
                - Super 클래스를 나타냄 (부모 Class)
            - **[중요]** `on ~`
                - 메소드 이름이 `on` 으로 시작하면 `Event Handler`
                - `이벤트가 발생했을 때 이벤트를 처리하는 callback method`
                - `Activity 생성 시 Android System에서 자체적으로 호출하는 메소드.`
                    - `사용자가 직접 해당 메소드를 호출 할 수 없음.`
                - Activity 의 `LifeCycle` 과 관련.
            
              
            
            - `res`
                
                - 안드로이드 시스템에서 사용하는 모든 정수 상수를 정의한 디렉토리.
                
                  
                
            - `R(.java)`
                
                ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.44.19.png]]
                
                - res 밑에 정의된 모든 정수 상수를 관리하는 프로그램.