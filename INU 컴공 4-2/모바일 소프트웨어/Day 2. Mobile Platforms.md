  

> 중요한 이슈는 Mobile 통신

  

**Packet**

- Header
- Body

  

## HSDPA (High Speed Downlink Packet Access)

- 서버로부터 Packet을 다운로드 받아 사용

> Down / Up Link  
> - Down Link : 기지국 —Packet→ 단말  
> - Up Link : 단말 —Packet→ 기지국

  

## Smart Phone

> Resistive VS Capacitive Touch Screen  
> - 감압식 (Resistive) → 누른다  
> - 정전식 (Capacitive) → 만진다

  

## Why Android?

- Open Source and Open Platform
- Java / Kotlin
    - 개발 과정의 언어 장벽 X
- 성능
    - Dalvik Virtual Machine
        - JVM 이 아닌 스마트폰 전용 VM 개발.
        - Mac OS 수준의 그래픽 처리 속도.
- 완벽한 API 제공
    - Upgrade 잘 됨.

  

## Android

- Feature
    - Dalvik VM
    - WebKit 기반 내장 웹 브라우저
    - SQLite (내장 DB)

  

## Android Platform

- **Android Stack → 4개의 계층**
    - 각 Layer 는 역할이 분리되어 작동.
    - **하위 Layer 에서 상위 Layer 로 작업 후 결과 전달.**
<br><br>
- **Layered Architecture**
    - **L4: Linux Kernel** (검증된 Core Platform)
        - 하드웨어는 Linux Kernel에서 담당하기 때문에 카메라가 세개든 네개든 신경쓰지 않아도 된다.
    - **L3: 라이브러리** : Java가 아닌 C 로 작성. (속도 중시)
        - Android Runtime에 의해 실행됨.
            - **Dalvik VM** : Register-based VM
        - **Android 실행 파일 Format : `.dex`** (Dalvik Executable)
    - **L2: Android API** (Application 개발자가 작업하는 영역) - JAVA 구현
    - **L1: Application**

  

## Dalvik 과 ART

- **JIT(Just-In-Time) Compilation**
    - JVM : interpreter
        - 한번에 한 줄 씩 Byte Code를 읽어 실행
    - **JIT**
        - **전체 Byte Code 중에서 실행 빈도가 높은 함수(Hotspot) 은 Native Code로 미리 번역**
- **AOT (Ahead Of Time) Compilation 과 ART (현재 Android 의 Strategy)**
    - **ART : Android Run Time**
        - VM이 아닌 Runtime Library
    - **특징**
        - **전체 Byte Code를 미리 Native Code로 변환하여 메모리에 저장.**
            - App 설치시에만 필요한 과정.
            - APP 실행시에는 Interprete 없이 바로 Native Code 로서 실행.

  

## Application Framework

- 가장 큰 특징 : **Reusable**

  

  

---

# Android App Structure

## [중요] Android Application 의 Component

- **Activity (필수 - 유일하게 화면을 가진 Component)**
    - Android Application 제작 시 반드시 구축해야 하는 Component.
    - 사용자 인터페이스(User Interface, UI) 를 구성하는 기본 단위.
    - 사용자와의 상호작용을 위해 필수적으로 요구되는 Component.
    - Layout
- **Service**
    - **Background 에서 실행**되는 Component.
    - UI 가 없으므로 Activity 와 연동하여 구현됨.
        - 예) Music Play
- **Broadcast Receiver (Broadcast: 불특정 다수)**
    - **시스템이 전달하는 내용을 수신.**
        - 예) 배터리 용량 부족!
    - UI 없음.
        - 수신된 전달 내용 처리 결과를 Activity 에게 전달.
- **Content Provider**
    - **Data를 관리하며 요청을 받아 다른 Application에게 Data를 제공.**
    - **DB** 라고 보면 될 듯?
        - **SQL 쿼리를 사용하여 질의응답.**

  

## Component 간의 통신

**Reusable**
- Android App. 은 타 App의 Component 를 사용할 수 있다.

  

**Intent - Component 와 Component 를 이어주는 기능.**

> 어떤 Component 와 정보를 주고 받으려면 이를 Android System에 전달.  
> **System은 지정된 Component를 찾는다.**  
> **- Component가 지정되어 있지 않을 경우 가장 적합한 Component를 찾아서 해당 Component 를 실행해준다.**

- 일종의 택배 기사.
- 어떠한 Component가 필요하다라는 요청을 보내면 Intent 가 전달해줌.

  

**Manifest - 우리가 만든 앱 (Packaging 된) 에 존재하는 Component 와 같은 정보 가짐.**

- Component 들은 택배 박스로서 Packaging.
- 해당 택배 박스에 Labeling 되어 있는 정보가 Manifest File.