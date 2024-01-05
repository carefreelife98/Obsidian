  

# Terraform

- IaC(Infrastructure as Code)는 수동 작업 대비 일관성 유지, 재사용성, 자동화 측면을 개선.
    - 테라폼은 cloudformation, pulumi와 같은 IaC 방법론을 실현하는 도구 중 하나일 뿐.
    - IaC 이전의 (클라우드 상의) 인프라 관리는 웹 콘솔이 주류로 사용.
    - 콘솔로도 작업은 가능하지만 대규모 / 반복적 작업을 수행하는 경우는 어떨까?
- 가령 필요에 따라 dev 환경을 생성하고 제거한다는 일이 반복적으로 필요하다고 가정.
    - 해당 dev VPC에는 각 3개의 퍼블릭/프라이빗 서브넷을 갖고, 퍼블릭엔 서비스 확인을 위한 접근용 ALB / 프라이빗엔 web서버 및 RDS가 구성되어야 하고, 각 ALB와 EC2에 대하여 쉬운 접근을 위해 Route53 도메인을 붙이는 것이 필요 조건.
    - 해당 과정을 수동으로 진행한다면 리소스 tag 규칙등의 과정에서 입력 실수로 인해 누락/오입력이 발생하고, 일관성이 훼손될 수 있다.
    - 생성한 뒤 qa가 끝나면 삭제하고, 다시 필요할때 재생성할 때, 이를 일일히 클릭-대기로 진행하면 시간도 오래 걸리고, 비효율적이다. 그러나 코드로 구성되어, 필요할때마다 복제하고 재생성 한다면 매우 효율적.

# Terraform Command

```
# 현재 디렉토리 초기화terraform init# Terraform 이 어떤 작업을 하게 될 지 Dry-Runterraform plan# 해당 코드를 apply하여 적용하고 Build.terraform apply# Terraform 으로 구축된 것 삭제terraform destroy# state file 새로고침terraform refresh# Terraform output 보기terraform output# Dot-formatted graph 생성terraform graph
```

  

  

## Terraform Cloud

- **UI integration**
- **버전 관리 시스템 (VCS)**
- **API Driven Workflows**
- **중앙 관리형**
- **Private Module Registry 제공**
- **Sentinel Policy Ensforcement**
    - IAM Policy?
- **Single Sign-on**
    - 사용자가 여러 웹 애플리케이션 또는 서비스에 대해 단일 인증을 사용하여 한 번의 로그인으로 여러 서비스에 액세스할 수 있는 인증 메커니즘
    - 이는 사용자 경험을 향상시키고 비밀번호 관리의 어려움을 줄이는 것에 도움.
    - SSO 시스템은 일반적으로 중앙 인증 서비스를 사용하여 여러 애플리케이션 간의 인증을 처리.
- **Secure API credentials**

  

## [Terraform Cloud] Workspace

**디렉토리별로 복잡하게 구분된 테라폼 구성 파일들을, 각 디렉토리(워크스페이스) / 프로젝트별로 시각화 해줘서 관리를 용이하게 한다.**

- 테라폼 사용이 고도화 될수록 디렉토리 구조가 복잡해지고, 가시성이 떨어짐
    - 처음 사용할때는 하나의 디렉토리에 main.tf, variable.tf 를 두는 식으로 간단하게 구성.
    - 하지만 필요에따라 VPC / IAM / EC2 / RDS 등 각 리소스별, 또는 자체 Module 활용시 모듈별 디렉토리 구성등으로 세분화 되어 관리되는 순간, 각 디렉토리별 연관 관계는 복잡해진다.
- 테라폼 명령어를 사용할때, 테라폼은 현재 디렉토리(working directory)에 있는 구성파일(*.tf), state파일(*.tfstate 또는 remote state), 변수파일(variables.tf)들의 값을 가져와 변경이전과(state) 변경이후(구성+변수)를 생성한 뒤 차이점을 비교한다
    - 이 실행되는 개별 디렉토리를 테클에서는 workspace라고 부른다(CLI의 workspace랑은 조금 다른 느낌인듯함).

  

## [Terraform Cloud] Getting Started

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.52.25.png]]

- Terraform Cloud 에서 제공하는 Sample 을 이용해서 시작해보자.

  

1. Terrform login
    
    ```
    $ terraform login
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.55.47.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.56.47.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.58.55.png]]
    
    - `terraform login` 명령어를 통해 API token을 생성하고 CLI가 사용하도록 설정해주자.

  

1. Terraform Sample repository 클론
    
    ```
    $ git clone https://github.com/hashicorp/tfc-getting-started.git
    ```
    
    - 인프라 자원을 Provisionning 하기 위한 Configuration 파일.
    - infra 자원은 `fake web service` 사용 (AWS-ec2 와 비슷한 가짜 자원을 생성)
        - [https://registry.terraform.io/providers/hashicorp/fakewebservices/latest](https://registry.terraform.io/providers/hashicorp/fakewebservices/latest)

  

1. setup script 실행
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.06.34.png]]
    
    - 작업 시작 전 Local PC에 `jq` 가 설치되어 있는 지 확인 할 것. (없다면 설치)
    
      
    
    ```
    $ cd tfc-getting-started$ ./scripts/setup.sh
    ```
    
    - configuration 초기화

  

## [Terraform Cloud] Example Configuration

1. Workspace
    - Name: getting-started
    - URL : [https://app.terraform.io/app/example-org-1b7e9c/workspaces/getting-started](https://app.terraform.io/app/example-org-1b7e9c/workspaces/getting-started)

  

1. Terraform init
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.10.50.png]]
    
      
    
2. Terraform Plan
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.11.28.png]]
    
    - Terraform Plan 은 Local의 내 맥북에서 init 되었지만, Terraform Cloud에서 실행되고 있음.
    
      
    
    > Terraform Cloud runs Terraform on disposable virtual machines in its own cloud infrastructure.
    
    - 테라폼 클라우드 내 클라우드 환경의 VM에서 테라폼이 실행됨.
    
      
    
    > This 'remote execution' helps provide consistency  
    > and visibility for critical provisioning operations.
    
    - 위와 같은 Cloud 환경에서의 원격 실행은 Provisioning 작업에 관해 일관성과 가시성을 제공.
    
      
    
    > It also enables notifications, version control integration, and powerful features like Sentinel policy enforcement and cost estimation (shown in the output above).
    
    - 또한 원격 실행은 알림 및 버전 컨트롤 (VCS) 기능, 보안 및 비용 관리를 수월하게 해줌.

  

1. Terraform Apply
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.19.56.png]]
    
    - Apply 하게 되면 Team&Governance 기능을 사용할 수 있는 30일 무료 체험 organization이 생성됨. → 무료체험 기간이 끝나면 자동으로 Free tier로 변경되니 걱정 ㄴㄴ
    
      
    
    **생성 된 것들**
    
    - Workspaces
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.23.44.png]]
        
        - Terraform Cloud 는 여러 Infrastructure 들을 directory 대신 workspace로 관리함.
        - 링크 : [https://app.terraform.io/app/example-org-1b7e9c/workspaces/getting-started](https://app.terraform.io/app/example-org-1b7e9c/workspaces/getting-started)

- **Remote State Management**
    - workspace 간의 output 공유 가능함.
    - 다른 workspace 에서 'terraform_remote_state' data source 를 참조 가능
    - 생성된 example configuration 에 관해 더 알고 싶다면 :
        - [https://app.terraform.io/fake-web-services](https://app.terraform.io/fake-web-services)

  

1. Next Steps:
    - [main.tf](http://main.tf) 파일 수정을 통한 Server 추가
        
        ```
        resource "fakewebservices_server" "server-3" {  name = "Server 3"  type = "t2.macro"}resource "fakewebservices_server" "server-4" {  name = "Server 4"  type = "t2.macro"}
        ```
        
        - 위 코드를 [main.tf](http://main.tf) 파일에 추가하여 추가적인 서버 생성 가능.
        - `terraform apply` 를 통해 적용한 후 확인하기.

- 실제 Cloud 환경에서 실행하기
    - Workspace 의 variables 메뉴에서 해당 Cloud Service provider 에 관한 Credentials 로 변경.
    - `terraform apply` 하여 적용.

  

  

  

## [Terraform Cloud] GIT 연동하기

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.44.18.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.51.31.png]]

- Version Control 탭에서 설정.
    - Terraform working directory 를 설정하여 API를 날릴 git repository를 정할 수 있다.
        - 설정하지 않으면 Terraform Cloud 의 default working directory 로 설정됨.
        - 여러 Infra 환경이 존재 할 수 있기 때문에. 따로 설정해주는 것이 좋아 보임.
    - GIT에서 코드 변동이 생기면 Trigger를 통해 자동으로 Terraform Cloud에서 실행할 수 있다.
        - Trigger의 조건
            
            - 항상 실행
            - GIT repo 내 특정 directory 변경 (ex. /modules) 되면 실행
            - GIT tag 반영 시 실행
            
              
            

## [Terraform Cloud] GIT push 테스트 해보기

  

> GIT과 연동된 Local Terraform 폴더에 존재하는 [main.tf](http://main.tf) 파일을 수정 후 Push.

  

1. [main.tf](http://main.tf) 에 서버 추가.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.06.23.png]]
    
    - server 3, 4 추가.
    
      
    
2. [backend.tf](http://backend.tf) 파일 변경
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.04.55.png]]
    
    - workspace 이름을 변경했기 때문에 최신화 시켜준다.
    
      
    
      
    

# [TFC] HashiCat-Workshop (AWS)

https://github.com/hashicorp/hashicat-aws