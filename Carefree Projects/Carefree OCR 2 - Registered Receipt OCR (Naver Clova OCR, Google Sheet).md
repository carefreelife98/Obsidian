---
Author: CarefreeLife98
Date: 2023-12-23T18:27:00
Agenda: 
tags:
  - ToyProject
---
# Agenda

> 회사에서 **매년 우편물 등기 영수증을 일일히 Excel 에 수작업으로 옮기는 작업**을 하고 있는데, 담당자께서 다음과 같은 프로그램이 있다면 편리할 것 같다 함에 진행한 개인 토이 프로젝트.
> 
> **요구 사항**
> - **등기 영수증을 사진 촬영 / 스캔 하여 해당 Text 들이 Google Sheet 에 자동으로 Numbering 후 입력 되었으면 좋겠다.**
> - **등기 영수증으로부터 사용할 정보**
> 	- 일자
> 	- 개수
> 	- 등기 번호
> 	- 각 등기 별 배송 조회 링크
> 	- 우편 번호
> 	- 법인 명
> 	- 수신인
> 	- 주소


# 전체 구조 및 Architecture

## 1. AWS Cloud Infrastructure
![[Pasted image 20231227113005.png]]
![[스크린샷 2023-12-27 오전 11.33.47.png]]


# 1. Cloud Infra 구축

# 2. CI / CD Pipeline 구축

# 3. Application 개발
# 4. Trouble Shooting

## Use JsonReader.setLenient(true) to accept malformed JSON
![[스크린샷 2023-12-23 오후 6.41.21.png]]
> Google Sheet API 를 사용하기 위한 계정 인증을 위한 JSON 파일을 CI/CD 환경에서 안전하게 사용하기 위해 Github Secrets 를 사용했다.
> - 하지만 Google Sheet API 를 요청하자, 이전에 발생하지 않던 에러가 발생했다.
> - **발생한 에러: `com.google.gson.stream.MalformedJsonException: Use JsonReader.setLenient(true) to accept malformed JSON at line 2 column 4 path $.`**
> 	- Malformed (흉한) Json Exception 이라는 에러 이름에서 JSON 파일의 형식이 잘못되었음을 유추하고 EC2 에 접속하여 해당 JSON 파일을 열어보았다.
>
> **해당 JSON 파일의 모습.**
> ![[스크린샷 2023-12-23 오후 6.48.41.png]]
> - 위처럼 JSON 파일에서 "" 가 전부 빠져 있어 에러가 발생한 모습을 볼 수 있었다.
> <br><br>
> **해결 방법**
> - Github Actions 의 Workflow file 의 옵션 중 create-json 을 사용하여 해결.

```yml
- name: create-json
      id: create-json
      uses: jsdaniell/create-json@1.1.2
      with:
        name: "secrets.json"
        json: ${{ secrets.SECERT_JSON }}
```

> **위 코드를 Workflow 파일에 추가하여 Github secrets 에 저장된 JSON 형식의 secret 을 정상적으로 사용할 수 있었다.**

