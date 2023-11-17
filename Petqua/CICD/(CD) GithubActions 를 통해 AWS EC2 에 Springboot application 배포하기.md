---
Author: CarefreeLife98
Date: 2023-11-17T09:14:00
Agenda: 
tags:
  - Petqua
---
# 1. EC2 생성 및 설정
![[스크린샷 2023-11-17 오전 9.21.26.png]]
> CodeDeploy 생성 시 어떤 인스턴스에 배포할 지 구분하는 값으로 태그를 사용하기 때문에 EC2에 태그를 추가해주어야 한다.


## 1.1 CodeDeploy 를 위한 S3FullAccess 역할 생성 및 부여
![[스크린샷 2023-11-17 오전 9.27.28.png]]![[스크린샷 2023-11-17 오전 9.27.41.png]]
> S3FullAccess 권한을 가진 역할을 생성.

![[스크린샷 2023-11-17 오전 9.29.07.png]]
> 이전에 생성한 배포 대상 EC2 에 해당 역할을 부여.

<br><br>

## 1.2 EC2에 CodeDeploy Agent 설치
```bash
# 패키지 설치
sudo yum update
sudo yum install ruby
sudo yum install wget

# CodeDeploy 설치
wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install
chmod +x ./install
# 최신 버전의 CodeDeploy 설치
sudo ./install auto

# 서비스 실행 확인
sudo service codedeploy-agent status
```

> **wget \https://bucket-name.s3.region-identifier.amazonaws.com/latest/install**
> - `bucket-name`은 해당 리전의 CodeDeploy 리소스 키트 파일이 포함되어 있는 Amazon S3 버킷의 이름이며, `region-identifier`는 리전의 식별자입니다.
> - 예:
> 	- `https://aws-codedeploy-us-east-2.s3.us-east-2.amazonaws.com/latest/install`


# 2. AWS S3 생성 및 설정

> AWS S3 버킷은 이미지 / zip 파일 등 정적 리소스들을 저장하기 위한 스토리지 서비스이다.
> - 여기서는 빌드한 스프링 부트 코드를 압축해서 S3에 저장한 후 EC2 서버에서 S3에 존재하는 프로젝트 압축 파일을 가져올 것.

## 2.1 AWS S3 버킷 생성
![[스크린샷 2023-11-17 오전 9.46.58.png]]

# 3. CodeDeploy 생성

> AWS 리소스에 배포를 도와주는 CodeDeploy 생성 및 설정

## 3.1 CodeDeploy 전용 IAM 역할 생성 및 부여
![[스크린샷 2023-11-17 오전 9.49.44.png]]![[스크린샷 2023-11-17 오전 9.49.52.png]]
> AWS IAM 에서 CodeDeployRole 을 생성한다.

## 3.2 CodeDeploy 애플리케이션 생성
![[스크린샷 2023-11-17 오전 9.51.36.png]]
![[스크린샷 2023-11-17 오전 9.56.51.png]]

## 3.3 CodeDeploy 애플리케이션 배포 그룹 생성
![[스크린샷 2023-11-17 오전 9.59.29.png]]
> - 원하는 배포 그룹 이름을 설정
> - 이전에 생성한 CodeDeplot 역할을 입력

![[스크린샷 2023-11-17 오전 10.00.40.png]]
> - 블루 그린 배포는 무중단 배포.
> - 현재 위치로 배포하게 되면 배포 실행 시 인스턴스가 업데이트를 하게 되며 서비스가 잠시 끊길 수 있다.
> - 이전에 설정한 EC2 인스턴스의 태그를 입력

![[스크린샷 2023-11-17 오전 10.04.11.png]]
> - AWS CodeDeploy Agent 설치는 한번만 진행.
> - 배포 방식은 AllAtOnce 로 구성.
> - 로드 밸런싱은 특별히 사용하지 않으므로 비활성화.

## 3.4 Github Action Secret - AWS Credentials 추가
![[스크린샷 2023-11-17 오전 11.33.37.png]]
> **프로젝트가 있는 Github repository 의 Actions Secret 추가**
> - AWS_ACCESS_KEY
> - AWS_SECRET_KEY


# 4. AppSepc 파일 작성

> 1. EC2: server
> 2. Storage: S3
> 3. Deploy: AWS CodeDeploy
> <br><br>
> Spring boot 프로젝트를 배포할 클라우드 인프라가 준비 되었으니 이제 각 인프라가 잘 작동할 수 있도록 세부 작업을 해야 한다.
> -  CodeDeploy 에서 배포를 위해 참조할 [AppSpec 파일](https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/application-specification-files.html)
> 	- AppSpec 파일을 사용해서 **프로젝트의 어떤 파일들을 EC2 의 어떤 경로에 복사할지 설정** 가능하고, **배포 프로세스 이후에 수행할 스크립트를 지정**하여 자동으로 서버를 띄울 수도 있다.
> 	- AppSpec 파일은 기본적으로 [루트 디렉터리에 위치](https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/reference-appspec-file-validate.html)해야 함.
> 	- 애플리케이션에서 EC2/온프레미스 컴퓨팅 플랫폼을 사용하는 경우 AppSpec 파일은 항상 YAML 형식

```yml
version: 0.0  
os: linux  
  
files:  
  - source:  /  
    destination: /home/ec2-user/app  
    overwrite: yes  
  
permissions:  
  - object: /  
    pattern: "**"  
    owner: ec2-user  
    group: ec2-user  
  
hooks:  
  AfterInstall:  
    - location: scripts/stop.sh  
      timeout: 60  
      runas: ec2-user  
  ApplicationStart:  
    - location: scripts/start.sh  
      timeout: 60  
      runas: ec2-user
```
> [AppSpec 파일 작성 예시](https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/reference-appspec-file-example.html#appspec-file-example-server)

## 4.1 AppSpec - 배포 스크립트 작성 (stop.sh, start.sh)
```sh
#!/usr/bin/env bash  
  
PROJECT_ROOT="/home/ubuntu/app"  
JAR_FILE="$PROJECT_ROOT/spring-webapp.jar"  
  
DEPLOY_LOG="$PROJECT_ROOT/deploy.log"  
  
TIME_NOW=$(date +%c)  
  
# 현재 구동 중인 애플리케이션 pid 확인  
CURRENT_PID=$(pgrep -f $JAR_FILE)  
  
# 프로세스가 켜져 있으면 종료  
if [ -z $CURRENT_PID ]; then  
  echo "$TIME_NOW > 현재 실행중인 애플리케이션이 없습니다" >> $DEPLOY_LOG  
else  
  echo "$TIME_NOW > 실행중인 $CURRENT_PID 애플리케이션 종료 " >> $DEPLOY_LOG  
  kill -15 $CURRENT_PID  
fi
```
> **애플리케이션이 이미 떠있으면 종료하는 스크립트.**

```sh
#!/usr/bin/env bash  
  
PROJECT_ROOT="/home/ubuntu/app"  
JAR_FILE="$PROJECT_ROOT/spring-webapp.jar"  
  
APP_LOG="$PROJECT_ROOT/application.log"  
ERROR_LOG="$PROJECT_ROOT/error.log"  
DEPLOY_LOG="$PROJECT_ROOT/deploy.log"  
  
TIME_NOW=$(date +%c)  
  
# build 파일 복사  
echo "$TIME_NOW > $JAR_FILE 파일 복사" >> $DEPLOY_LOG  
cp $PROJECT_ROOT/build/libs/*.jar $JAR_FILE  
  
# jar 파일 실행  
echo "$TIME_NOW > $JAR_FILE 파일 실행" >> $DEPLOY_LOG  
nohup java -jar $JAR_FILE > $APP_LOG 2> $ERROR_LOG &  
  
CURRENT_PID=$(pgrep -f $JAR_FILE)  
echo "$TIME_NOW > 실행된 프로세스 아이디 $CURRENT_PID 입니다." >> $DEPLOY_LOG
```
> **애플리케이션을 실행하는 스크립트.**
> - Github Actions Workflow 에서 빌드를 하기 때문에 JAR 파일만 복사 후 실행.


# 5. Github Actions Workflow 파일 작성
```yaml
name: Deploy to Amazon EC2

on:
  push:
    branches:
      - main

# 본인이 설정한 값을 여기서 채워넣습니다.
# 리전, 버킷 이름, CodeDeploy 앱 이름, CodeDeploy 배포 그룹 이름
env:
  AWS_REGION: ap-northeast-2
  S3_BUCKET_NAME: petqua-github-actions-was
  CODE_DEPLOY_APPLICATION_NAME: petqua-was
  CODE_DEPLOY_DEPLOYMENT_GROUP_NAME: petqua-was-deployGroup

permissions:
  contents: read

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    environment: production

    steps:
    # (1) 기본 체크아웃
    - name: Checkout
      uses: actions/checkout@v3

    # (2) JDK 11 세팅
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        distribution: 'temurin'
        java-version: '11'
    
    - name: Run chmod to make gradlew executable
      run: chmod +x ./gradlew

    # (3) Gradle build (Test 제외)
    - name: Build with Gradle
      uses: gradle/gradle-build-action@0d13054264b0bb894ded474f08ebb30921341cee
      with:
        arguments: clean build -x test

    # (4) AWS 인증 (IAM 사용자 Access Key, Secret Key 활용)
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.PETQUA_AWS_ACCESS_KEY }}
        aws-secret-access-key: ${{ secrets.PETQUA_AWS_SECRET_KEY }}
        aws-region: ${{ env.AWS_REGION }}

    # (5) 빌드 결과물을 S3 버킷에 업로드
    - name: Upload to AWS S3
      run: |
        aws deploy push \
          --application-name ${{ env.CODE_DEPLOY_APPLICATION_NAME }} \
          --ignore-hidden-files \
          --s3-location s3://$S3_BUCKET_NAME/$GITHUB_SHA.zip \
          --source .

    # (6) S3 버킷에 있는 파일을 대상으로 CodeDeploy 실행
    - name: Deploy to AWS EC2 from S3
      run: |
        aws deploy create-deployment \
          --application-name ${{ env.CODE_DEPLOY_APPLICATION_NAME }} \
          --deployment-config-name CodeDeployDefault.AllAtOnce \
          --deployment-group-name ${{ env.CODE_DEPLOY_DEPLOYMENT_GROUP_NAME }} \
          --s3-location bucket=$S3_BUCKET_NAME,key=$GITHUB_SHA.zip,bundleType=zip

```

# 6. Test

> 1. Local 에서 Code Push
> 2. Github Actions 동작
> 3. AWS S3 에 application.zip 형태로 업로드
> 4. AWS CodeDeploy 에 의해 배포 그룹 내 EC2에 해당 파일 복사.