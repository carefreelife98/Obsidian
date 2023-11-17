---
Author: CarefreeLife98
Date: 2023-11-17T12:51:00
Agenda: 
tags:
  - Petqua
---
# CodeDeployPlugin::ScriptError - Script at specified location
```bash
ERROR [codedeploy-agent(9614)]: InstanceAgent::Plugins::CodeDeployPlugin::CommandPoller: Error during perform: InstanceAgent::Plugins::CodeDeployPlugin::ScriptError - Script at specified location: scripts/stop.sh run as user ec2-user failed with exit code 1
```
> start.sh / stop.sh  의 Project_Root 가 Ubuntu 로 되어 있어서 발생한 문제. ec2-user 로 변경 후 해결.


# CodeDeployPlugin::ScriptError - Script does not exist at specified location:
```sh
Error during perform: InstanceAgent::Plugins::CodeDeployPlugin::ScriptError - Script does not exist at specified location: /opt/codedeploy-agent/deployment-root/1961be8e-557c-40b8-b4c1-779cb95c8455/d-R18DNS0Y1/deployment-archive/scripts/stop.sh
```
> /scripts/stop.sh
> - 위 경로에 쉘 스크립트 파일이 존재하지 않아서 발생한 문제.
> - scripts 디렉토리 생성 후 스크립트 파일을 해당 디렉토리로 옮겨 해결.


# CodeDeploy 배포 성공 후 애플리케이션 실행 실패
```sh
[ec2-user@ip-172-31-44-42 app]$ cat deploy.log
Fri 17 Nov 2023 03:44:43 AM UTC > 현재 실행중인 애플리케이션이 없습니다
Fri 17 Nov 2023 03:44:44 AM UTC > /home/ec2-user/app/petqua.jar 파일 복사
Fri 17 Nov 2023 03:44:44 AM UTC > /home/ec2-user/app/petqua.jar 파일 실행
Fri 17 Nov 2023 03:44:44 AM UTC > 실행된 프로세스 아이디  입니다.
[ec2-user@ip-172-31-44-42 app]$ cat error.log
nohup: failed to run command ‘java’: No such file or directory
```
> 실행된 프로세스 아이디가 로그에 남지 않아 에러 로그를 확인해보니 EC2 에 java 가 깔려 있지 않아서 실행 자체가 되지 않음.
> - `sudo yum install java-11-amazon-corretto-headless`
> 	- GUI 등을 제외한 서버 전용 headless jdk 11 을 설치.


# CodeDeploy 배포 후 애플리케이션 실행 중 EC2 권한 문제
> 배포 성공 후에도 ps 명령시 프로세스가 안뜨길래 application.log를 확인해보았다.
> - 아래와 같은 에러 발생
> - `AwsSecretsManagerPropertySourceLocator : Fail fast is set and there was an error reading configuration from AWS Secrets Manager`


![[스크린샷 2023-11-17 오후 1.17.43.png]]
> Spring boot application 내부 bootstrap 코드에서 AWS Secret Manager 에 접근하여 AWS credential 을 가져와야 하는데 기존 EC2에게 준 역할에 S3 FullAccess 권한만 있어서 생긴 문제.

![[스크린샷 2023-11-17 오후 1.20.07.png]]
> **EC2 에게 부여한 역할 `EC2_S3FullAccessForCodeDeploy` 에 `SecretsManagerReadWrite` 권한 부여.**

