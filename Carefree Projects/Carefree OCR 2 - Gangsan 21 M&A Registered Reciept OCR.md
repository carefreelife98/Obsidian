---
Author: CarefreeLife98
Date: 2023-12-23T18:27:00
Agenda: 
tags:
  - ToyProject
---
# Agenda


# 전체 구조 및 Architecture

# 1. Application 개발

# 2. Cloud Infra 구축

# 3. CI / CD Pipeline 구축
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
> - 위 처럼 JSON 파일에서 "" 가 전부 빠져 있어 에러가 발생한 모습을 볼 수 있었다.