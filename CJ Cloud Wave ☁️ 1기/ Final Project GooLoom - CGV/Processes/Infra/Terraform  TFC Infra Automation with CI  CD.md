  

# Terraform Cloud / Github 을 활용한 CI/CD 를 통해 Infra 구축 자동화 Logic

  

## 1. Terraform Cloud 가입 및 Organization 참가 / 생성

1. Terraform Cloud 가입
2. Organization 참가 / 생성
3. VCS Workflow 옵션으로 Workspace 생성
4. Github Repository 연동

  

## 2. Workspace 설정 및 Provider Credential Variable 추가

## Workspace → Settings → Version Control 이동

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.29.39.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.29.52.png]]

  

  

## Terraform Working Directory

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.36.42.png]]

- TFC가 현재 Workspace와 연동된 VCS에서 WebHook 하여 수정/추가 된 Terraform Code를 수행할 경로를 지정.
    - VCS Repository 에서 테라폼 코드가 존재하는 경로를 명시 (Git)

  

## Variables 설정 (현재 Provider : AWS)

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.29.10.png]]

**AWS_ACCESS_KEY_ID 및 AWS_SECRET_ACCESS_KEY 추가**

- Git에 Push 한 Terraform Code - [main.tf](http://main.tf) 및 [variables.tf](http://variables.tf) 에 임의로 지정된 AWS_ACCESS_KEY, AWS_SECRET_KEY 변수를 TFC 에서 Hook 해오며 TFC에 위 사진처럼 저장되어 있는 Variable 로 Overwrite 해준 후 해당 AWS Credentials / TFC의 VM을 사용해 개발자의 AWS 계정에 접근, terraform code를 사용해 배포해준다.
- 주의 : Value 값은 String 이므로 “” 안에 포함해줄것.
- Variables 설정 옵션
    
    - Terraform Code에 Overwrite할 용도이기 때문에 `HCL` 선택.
    - Credential 에 관한 정보이므로 Sensitive 옵션 선택하여 암호화 보관.
    
      
    

  

  

## 3. [CI] Local 에서 Terraform Code 개발 및 Commit / Push

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.28.25.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.28.48.png]]

- Terraform 코드 개발 후 VCS(Git) 에 Push 하게 되면 위처럼 자동으로 TFC가 해당 Github repository를 web hook 하여 TFC의 Variable에 저장된 AWS Credential을 Overwrite하고, 계정에 접근하여 Terraform 코드에 기반해 자동 Infra 구축 및 배포를 해주게 된다.

  

## 4. Terraform Destroy

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.27.29.png]]

- 위 사진처럼 Local 에서 terraform destory 는 먹히지 않는다.
    - VCS Workflow 로서만 추가 / 수정 / 삭제 가 이루어져야 각 작업에 대해 혼란이 생기지 않으므로 막아둔 것으로 보인다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.27.47.png]]

- TFC의 Workspace → Settings 에서 Terraform Destroy가 가능함. (유저 권한에 따라서)

  

  

# CGV 실제 프로젝트 인프라 구축

## 1. Local Terraform 작업 폴더에 폴더 생성

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.14.12.png]]

  

## 2. Terraform 파일 작성

```
# main.tfterraform {  required_providers {    aws = {      source  = "hashicorp/aws"      version = "=3.42.0"    }  }}provider "aws" {  region  = var.region  access_key = var.AWS_ACCESS_KEY_ID  secret_key = var.AWS_SECRET_ACCESS_KEY}resource "aws_vpc" "vpc" {  cidr_block           = var.vpc_cidr  enable_dns_hostnames = true  tags = {    name = "${var.prefix}-vpc-${var.region}"    environment = "Production"  }}
```

  

시연 영상: [https://youtu.be/PIq4U9E6PfM](https://youtu.be/PIq4U9E6PfM)