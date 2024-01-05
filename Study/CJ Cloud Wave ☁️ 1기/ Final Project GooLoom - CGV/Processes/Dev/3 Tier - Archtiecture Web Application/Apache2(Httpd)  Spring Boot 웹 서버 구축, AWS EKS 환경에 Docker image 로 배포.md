  

EKSCTL eks 클러스터 연결

```
eksctl register cluster --name lastdance --provider my-provider --region region-code
```

  

Dockerfile 만들기

```
FROM eclipse-temurin:17-jdk-alpineVOLUME /tmpARG JAR_FILECOPY ${JAR_FILE} app.jarENTRYPOINT ["java","-jar","/app.jar"]
```

  

# 3 Tier Architecture 로서 Apache2(Httpd) 와 함께 Spring Boot 웹 서버 구축 및 AWS EKS 환경에 Docker image 로 배포하기

  

## 1. 간단한 웹 서버 구축 및 DB 연동 테스트

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.49.48.png]]

- JDBC 쓰다가 JPA 사용하니 신세계가 펼쳐졌다.
- Primary key 중복 문제 등 모든 트러블 슈팅 성공
- 코드 길이도 210 줄에서 약 5줄로 줄어들었다.
- 승재 형 감사합니다..

  

## 2. Docker Image Build

1. Intellij 에서 우측 Gradle 이모티콘 → Tasks → build → bootJar 실행
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.53.23.png]]
    
      
    
2. 그럼 좌측 프로젝트 → build → libs 에 .Jar Snapshot이 생긴다.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.54.05.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.40.22.png]]
    
    ```
    $ java -jar *.jar
    ```
    
    - 생성된 .jar 파일은 추후 배포 전 Local 환경에서 시험 동작 시켜보는 것이 좋다.
    
      
    
3. 해당 파일을 기반으로 동작할 Dockerfile을 프로젝트의 최상단 경로에 작성
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.20.42.png]]
    
    ```
    # Use the offical OpenJDK base imageFROM openjdk:19CMD ["/.gradlew", "clean", "package"]ARG JAR_FILE_PATH=build/libs/*.jarCOPY ${JAR_FILE_PATH} spring.jar# Expose the port app is running on(change to match app’s port)EXPOSE 8080# Command to run the applicationENTRYPOINT ["java", "-jar", "spring.jar"]
    ```
    
      
    
4. Dockerfile이 존재하는 프로젝트 최상단 경로에서 Docker Image 빌드 및 확인 후 Docker hub Push.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.01.47.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.04.30.png]]
    
    ```
    docker buildx build --platform=linux/amd64 -t csm4903/gooloom-was:v2.0.1 .
    ```
    
    - `M1 Mac`을 사용한다면 위 사진처럼 `buildx` 라는 멀티 플랫폼 빌드 툴을 사용해서 amd64 아키텍쳐로 빌드해야 오류가 발생하지 않는다.
    
      
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.07.09.png]]
    
    - 이미지 빌드가 성공했다면 후에 EKS에서 끌어다 사용 할 수 있게 Docker hub로 push 해보자.
    
      
    
5. AWS EKS 에 Bastion을 통해 접속 후 WAS 클러스터에 yaml apply를 통한 pod 생성
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.10.28.png]]
    
    - AWS EKS 클러스터 중 두번째 Private Subnet에 존재하는 WAS 클러스터 (lastDance2)에 접근.
    
      
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.14.06.png]]
    
    - test 용 yaml 파일을 작성 후 `kubectl apply -f (파일이름.yaml)` 실행.
    - 이전에 Docker hub에 Push 해놓은 이미지 이름과 버전을 잘 명시해주자.
    - `-n (namespace 이름)` 옵션을 추가해서 특정 namespace에 생성해주어도 좋다.
    
      
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.26.36.png]]
    
    - `kubectl get po -w` 실행 시 Running 되는 것을 볼 수 있다.