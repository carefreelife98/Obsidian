
# Android
## Launcher Icon
- 각 해상도 별 Android Lancher Icon 이 존재한다.

## drawable - XML
- ic_launcher_background.xml
- ic_launcher_foreground.xml

## /java/com.프로젝트이름
- **테스트 목적의 폴더**
	- **ExampleInstrumentedTest**
		- 안드로이드 하드웨어 테스트 용.
		- `assertEquals("프로젝트이름", 하드웨어에 설치된 프로젝트이름)` : 하드웨어에 해당 프로젝트가 제대로 설치가 되었는지 확인.
	- **ExampleUnitTest**
		- 소프트웨어 테스트 용.


# Kotlin
## Kotlin 특징
- **Static type (중요)**
	- **프로그램 구성 요소의 타입을 Compile Time 에 알 수 있다.**
- **Concise & Safe**
	- Python, Swift 등 최근 언어의 추세를 따른다.
- **Multi-Paradigm Language**
	- 함수형 프로그래밍 + 객체 지향 프로그래밍(OOP)
	- 함수형 프로그래밍
		- 순수 함수
		- 람다 식 - First Class Citizen
		- 고차 함수

### Intellij 환경 설정
`cmd + ,` (설정)
- Editor - General - Auto Import - Kotlin 아래 두 가지 활성화 하기
	- **Add Unambiguous imports on the fly**
	- **Optimize imports on the fly**




