- 절차적 언어
    - Ansible - 순차적으로 실행
        - 생성 및 작업 정보를 보관하고있는 파일이 없음
- 선언형 언어
    - Terraform
        - 생성 및 작업 정보를 보관하고있는 파일 존재
            
            → 삭제 명령 시 해당 정보를 기반으로 한번에 깔끔하게 삭제 가능
            
    - CloudFormation
        
        - 고객의 요구사항에 맞추어 미리 Yaml 파일을 작성 해놓은 후 해당 메일 전송.
        - 짧은 시간내에 비즈니스 요구사항을 맞추어 구축 및 테스팅 가능
        
          
        
          
        
          
        
          
        
          
        

Terraform

- tfenv
    - Terraform version 관리 (버젼 맞춰주기)

  

  

  

  

  

  

  

  

## Terraform 실습

  

- Terrform 워크 스페이스(폴더) 내부에 Dev / Stg / Prd 워크스페이스(폴더)를 생성하여 분리.
- 실습을 위해 EC2 VM의 용량 증가 시키기
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.26.07.png]]
    
      
    
      
    
      
    
    1. AWS EC2에서 볼륨을 증가 시켜도 Cloud9에 바로 attach되지 않는다.
    2. Cloud9에도 OS가 존재하므로 해당 OS에 attach 시켜줘야 볼륨이 인식됨.
        
        ```
        $ sudo vi resize.sh
        ```
        
        ```
        #!/bin/bash# Specify the desired volume size in GiB as a command line argument. If not specified, default to 20 GiB.SIZE=${1:-20}# Get the ID of the environment host Amazon EC2 instance.INSTANCEID=$(curl http://169.254.169.254/latest/meta-data/instance-id)# Get the ID of the Amazon EBS volume associated with the instance.VOLUMEID=$(aws ec2 describe-instances \  --instance-id $INSTANCEID \  --query "Reservations[0].Instances[0].BlockDeviceMappings[0].Ebs.VolumeId" \  --output text)# Resize the EBS volume.aws ec2 modify-volume --volume-id $VOLUMEID --size $SIZE# Wait for the resize to finish.while [ \  "$(aws ec2 describe-volumes-modifications \    --volume-id $VOLUMEID \    --filters Name=modification-state,Values="optimizing","completed" \    --query "length(VolumesModifications)"\    --output text)" != "1" ]; dosleep 1done\#Check if we're on an NVMe filesystemif [ $(readlink -f /dev/xvda) = "/dev/xvda" ]then  # Rewrite the partition table so that the partition takes up all the space that it can.  sudo growpart /dev/xvda 1  # Expand the size of the file system.  # Check if we are on AL2  STR=$(cat /etc/os-release)  SUB="VERSION_ID=\"2\""  if [[ "$STR" == *"$SUB"* ]]  then    sudo xfs_growfs -d /  else    sudo resize2fs /dev/xvda1  fielse  # Rewrite the partition table so that the partition takes up all the space that it can.  sudo growpart /dev/nvme0n1 1  # Expand the size of the file system.  # Check if we're on AL2  STR=$(cat /etc/os-release)  SUB="VERSION_ID=\"2\""  if [[ "$STR" == *"$SUB"* ]]  then    sudo xfs_growfs -d /  else    sudo resize2fs /dev/nvme0n1p1  fifi
        ```
        
          
        
        ```
        $ sudo chmod 755 resize.sh$ sudo ./resize.sh
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.36.45.png]]
        
          
        
        ```
        $ df -h
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.37.28.png]]
        
          
        
          
        
        ## 늘린 볼륨을 다시 줄이기 힘든 이유
        
        - 리눅스 단에서 사용하지 않는 볼륨을 잘라줘야함(어려움).
        - **Snapshot**
            - 사용한 공간에 대한 백업(6.3G)
            - 백업 으로 끝남
            - Daily 로 Incremental 증가된 부분에 대해서만
            - 백업을 위한 기능
        - **AMI**
            
            - 최대 용량에 대한 백업(40G)
            - 부팅이 되는 OS 이미지 파일
            - Weekly Backup (최신 OS 적용하기 위해)
            - EC2 장애 시 AMI를 불러서 해결?
            
              
            
              
            
              
            

## tfenv

  

1. tfenv clone 설치

```
$ git clone https://github.com/tfutils/tfenv.git ~/.tfenv
```

  

1. Terraform 특정 버전 설치
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.26.17.png]]
    

  

1. Terraform 특정 버전 설치 확인
    
    ```
    $ tfenv list
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.27.27.png]]
    

  

1. 특정 버전의 Terraform 사용하기
    
    ```
    $ tfenv use (버젼 명시)$ terraform --version
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.29.22.png]]
    

  

1. 특정 버전의 Terraform 삭제하기
    
    ```
    $ tfenv uninstall (버전 명시)
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.37.11.png]]
    
      
    
      
    

## tfswitch

- Terraform 코드내에 포함하면 해당 코드 실행시 자동으로 명시된 버전의 Terraform으로 실행된다.

  

## Terraform - Keypair

AWS Console (SSH Key gen을 통한 Public / Private key 생성)

- Pem 키(Private key) 다운로드 및 EC2 접속

  

위 과정을 Terraform Code로 진행해보자.

## 기본 Private Key 생성 방법

```
$ ssh-keygen -t rsa -b 2048 -C "" -f "$HOME/.ssh/<키페어이름>" -N "" && sudo cp /home/ec2-user/.ssh/<키페어이름> /home/ec2-user/.ssh/<키페어이름>.pem       옵션 설명       -t 생성할 키 타입       -b 생성할 키의 비트 수       -C 코멘트       -f keyfile 이름       -N 새 암호
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.47.06.png]]

  

## Local에 Private 키 생성 후 **AWS에 Public Key를 생성하여 Matching**

```
$ ssh-keygen -t rsa -b 2048 -C "" -f "$HOME/.ssh/csm-tf-keypair" -N "" && sudo cp /home/ec2-user/.ssh/csm-tf-keypair /home/ec2-user/.ssh/csm-tf-keypair.pem$ cd $HOME/.ssh$ aws ec2 import-key-pair --key-name "csm-tf-keypair" --public-key-material fileb://~/.ssh/csm-tf-keypair.pub
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.48.11.png]]

  

- 아래와 같이 AWS의 Keypair 에 해당 키의 Public key가 등록이 되어 있는 것을 볼 수 있다.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.49.19.png]]
    
      
    
      
    

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.00.03.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.00.38.png]]

- 방식 1

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.01.52.png]]

```
provider "aws" {  region = "ap-northeast-2"}resource "aws_key_pair" "key-pair" {  key_name = "terraform-key"  public_key = file("/home/ec2-user/.ssh/csm-tf-keypair.pub")}
```

- 방식 2
    
    - 실제 키 정보 대신 키의 경로를 입력하여 보안 및 코드의 단순화 향상
    
      
    

## Terraform init

```
$ terraform init
```

- Terraform 파일이 있는 경로에서 Terraform init 실행
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.06.11.png]]
    
      
    

## Terraform plan

```
$ terraform plan
```

- Terraform 파일의 예상 실행 과정을 보여줌.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.10.00.png]]
    

  

## Terraform apply

```
$ terraform apply
```

- terraform 실행
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.10.26.png]]
    
      
    

## Terraform Destroy

```
$ terraform destroy
```

- Terraform은 생성 시 기록된 정보를 기반으로 한번에 깔끔한 삭제가 가능함.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.31.50.png]]
    

  

## Terraform Code

- Provider 지정 필수
    
    - AWS의 경우 Region 지정 필수
    
      
    

# Terraform 소개

- Terraform은 오픈 소스 프로비저닝 도구
- Go로 작성된 단일 바이너리로 제공
- Terraform은 크로스 플랫폼이며 Linux, Windows 또는 MacOS에서 실행 가능

  

## AWS에 Terraform을 쓰는 이유

- 다중 클라우드 및 하이브리드 인프라 지원
- 다른 클라우드 제공 업체로부터 마이그레이션
- 프로비저닝 속도 향상
- 효율성 향상
- 위험 최소화

  

  

## Terraform Workspaces

- Terraform Workspaces는 Terraform 코드가 포함된 폴더 또는 디렉토리
- Terraform 파일은 항상* .tf 또는* .tfvars 확장자로 끝남
- Terraform Workspaces에는 일반적으로 3개정도의 파일을 저장 (정해진 것은 아님)
- main.tf : 대부분의 기능 코드는 main.tf에 있음
- variables.tf : variables.tf는 변수를 저장하기 위한 것
- outputs.tf : Terraform 실행 끝에 표시되는 내용을 지정

  

## Terraform Data Sources

- Terraform Data Sources는 공급자가 기존 리소스를 반환하도록 쿼리하는 방법
- 자체적으로 사용할 매개 변수에 액세스 할 수 있음
- **Terraform에서 AWS로 API를 보냄.**
- **자동으로 AWS에서 최신 AMI로 업데이트하여 실행됨. (최신 버전 자동 유지)**

```
$ data "aws_ami" "ubuntu" { most_recent = truefilter {name = "name"values = ["ubuntu/images/hvm-ssd/ubuntu-trusty-14.04-amd64-server-*"] }filter {name = "virtualization-type"values = ["hvm"]}owners = ["099720109477"] # Canonical }
```

  

## [Variables.tf](http://Variables.tf) 파일 구성

- dev, stg, prd 별로 이름을 구분해주어 생성

  

## Remote Exec Provisioner

- Remote Exec Provisioner를 사용하면 대상 호스트에서 스크립트 또는 기타 프로그램을 실행할 수 있음
- 자동으로 실행할 수 있는 경우(예 : 소프트웨어 설치 프로그램) Remote Exec Provisioner로 실행 가능
- 아래 예시에서는 일부 권한 및 소유권을 변경하고 일부 환경 변수가 있는 스크립트를 실행하기 위해 몇 가지 명령을 실행

```
provisioner "remote-exec" {inline = ["sudo chown -R ${var.admin_username}:${var.admin_username} /var/www/html","chmod +x *.sh","PLACEHOLDER=${var.placeholder} WIDTH=${var.width} HEIGHT=${var.height} PREFIX=${var.prefix} ./deploy_app.sh", ]...}
```

  

  

## Terraform Cloud

- 멀티 클라우드 환경 제공

  

## Terraform Remote State

- AWS 의 S3처럼 외부 Repository에 저장
- 기본적으로 Terraform은 랩톱 또는 워크스테이션의 작업 공간 디렉토리에 상태 파일을 저장
- 개발 및 실험에는 괜찮지만 프로덕션 환경에서는 상태 파일을 안전하게 보호하고 저장해야 함
    - Terraform에는 상태 파일을 원격으로 저장하고 보호하는 옵션이 있음
    - 모든 상태 파일은 암호화되어 Terraform Cloud 계정에 안전하게 저장 (HashiCorp Vault 사용)
- 상태 파일을 다시 잃어 버리거나 삭제하는 것에 대해 걱정할 필요가 없음

  

  

## Terraform 을 통한 EC2 생성하기

1. Terraform 작성
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.12.30.png]]
    
2. terraform init
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.12.48.png]]
    
3. terraform plan
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.13.00.png]]
    
4. terraform apply
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.13.11.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.13.23.png]]
    
      
    
      
    

## Terraform Registry

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.33.28.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.38.16.png]]

- Provider 별 (AWS, Azure, GCP..) Terraform 작성 예시 Docs를 볼 수 있다.

  

  

## Terraform Resource “ ” “ ”

```
resource "aws_instance" "example" {  ami           = "ami-0b7f251f110d30ada" # current-ami-id.sh 를 실행하여 나온 AMI ID를 입력합니다.  instance_type = "t2.nano"  tags = {    Name = "Carefree Terraform"  }}
```

- Resource 옆의 두 개의 String 역할
    - 왼쪽은 Fixed된 이름.
    - 오른쪽은 사용자 지정이름.
- 해당 String을 바탕으로 Terraform configuration이 되어 Terraform 생성 정보가 되며 해당 정보를 바탕으로 Terraform destroy 등을 실행하게 된다.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.48.36.png]]
    
      
    

  

## Terraform with Variables

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.51.50.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.53.19.png]]

- 위와 같이 함수를 넣어 사용도 가능하다.

  

  

## Terraform VPC ID & Tag Name

```
aws ec2 describe-vpcs --output text | grep VPCS | awk '{print "ID: " $8, "CIDR: " $2}' && aws ec2 describe-vpcs --output text | grep TAGS | awk '{print "NAME: " $3}'
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.24.04.png]]

  

```
resource "aws_security_group" "web" {  name        = "web"  description = "Allow web inbound traffic"  vpc_id      = "vpc-0fcc9ed532b1c80b9" # 사용하고자하는 VPC ID 입력  ingress {    description = "Web from VPC"    from_port   = 0    to_port     = 0    protocol    = "tcp"    cidr_blocks = ["0.0.0.0/0"]  }  egress {    from_port   = 0    to_port     = 0    protocol    = "-1"    cidr_blocks = ["0.0.0.0/0"]  }  tags = {    Name = "allow_web"  }}
```

  

## [이중화 구성] VPC, 보안그룹 및 Subnet 직접 설정 후 Terraform apply

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.33.07.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.38.10.png]]

  

  

  

## Terraform graph

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.43.14.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.43.43.png]]

1. Terraform graph 명령어 실행
    - 반환값 복사
2. [Graphbiz](https://dreampuf.github.io/GraphvizOnline/#digraph%20G%20%7B%0A%0A%20%20subgraph%20cluster_0%20%7B%0A%20%20%20%20style%3Dfilled%3B%0A%20%20%20%20color%3Dlightgrey%3B%0A%20%20%20%20node%20%5Bstyle%3Dfilled%2Ccolor%3Dwhite%5D%3B%0A%20%20%20%20a0%20-%3E%20a1%20-%3E%20a2%20-%3E%20a3%3B%0A%20%20%20%20label%20%3D%20%22process%20%231%22%3B%0A%20%20%7D%0A%0A%20%20subgraph%20cluster_1%20%7B%0A%20%20%20%20node%20%5Bstyle%3Dfilled%5D%3B%0A%20%20%20%20b0%20-%3E%20b1%20-%3E%20b2%20-%3E%20b3%3B%0A%20%20%20%20label%20%3D%20%22process%20%232%22%3B%0A%20%20%20%20color%3Dblue%0A%20%20%7D%0A%20%20start%20-%3E%20a0%3B%0A%20%20start%20-%3E%20b0%3B%0A%20%20a1%20-%3E%20b3%3B%0A%20%20b2%20-%3E%20a3%3B%0A%20%20a3%20-%3E%20a0%3B%0A%20%20a3%20-%3E%20end%3B%0A%20%20b3%20-%3E%20end%3B%0A%0A%20%20start%20%5Bshape%3DMdiamond%5D%3B%0A%20%20end%20%5Bshape%3DMsquare%5D%3B%0A%7D) 접속 후 붙혀넣기
    
    → 연관성 확인이 가능하다.
    
      
    
      
    

## Pluralith

  

1. [https://github.com/Pluralith/pluralith-cli/releases](https://github.com/Pluralith/pluralith-cli/releases)
    - 위 링크 접속 후 적절한 버전의 Pluralith cli 다운로드
2. /usr/local/bin 으로 해당 파일 이동 후 pluralith 로 이름 변경.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.30.52.png]]
    

  

1. pluralith install 및 API key를 이용하여 접속
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.31.53.png]]
    

  

1. 해당 URL로 이동하면 Terraform Graph 확인 가능.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.23.44.png]]
    

  

  

  

  

## CloudCraft

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.36.27.png]]

  

## Cacoo

  

## Draw.io

- VSCode 확장프로그램 사용 가능

  

  

  

  

# Cloud9 에서 Terraform - ALB 생성하기

1. ALB 생성
    1. aws_alb.tf 생성
        
        ```
        resource "aws_lb" "test" {  name               = "carefree-test-lb-tf"  internal           = false  load_balancer_type = "application"  security_groups    = [aws_security_group.carefree-sg-alb.id]  subnets            = ["subnet-04d3d0bbcf0f9ef43","subnet-0357233c743d06d66","subnet-052b1c9f648d64603","subnet-0d58fc5216e87a0c4"]#  subnets            = aws_subnet.public.*.id  enable_deletion_protection = false/*  access_logs {    bucket  = aws_s3_bucket.lb_logs.bucket    prefix  = "test-lb"    enabled = true  }*/  tags = {    Name = "carefree_aws_lb"  }}resource "aws_security_group" "carefree-sg-alb" {  name        = "carefree-sg-alb"  description = "Allow web inbound traffic"  vpc_id      = "vpc-0fcc9ed532b1c80b9" # 사용하고자하는 VPC ID 입력  ingress {    description = "Web from VPC"    from_port   = 0    to_port     = 0    protocol    = "-1" # 모든 접속 허용    cidr_blocks = ["0.0.0.0/0"]  }  egress {    from_port   = 0    to_port     = 0    protocol    = "-1"     cidr_blocks = ["0.0.0.0/0"]  }  tags = {    Name = "carefree_allow_web"  }}
        ```
        
    2. [provider.tf](http://provider.tf) 생성
        
        ```
        provider "aws"{ # aws 라는 provider 선언입니다.#  access_key = "자신의 Key 를 입력" # Cloud9 을 사용하면 IAM Role 을 활용하게 됩니다. 만일 Mac 사용자는 IAM 에서 발급해야 합니다.#  secret_key = "자신의 Key 를 입력"  region = "ap-northeast-2" # 사용할 AWS Region 을 선언합니다.}
        ```
        
    3. terraform init / apply
        
        ```
        terraform initterraform apply
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.32.14.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.32.55.png]]
        
          
        
2. Target Group 생성
    1. instance tg / ip tg 선택
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.37.39.png]]
        
    2. terraform_target_group 작성
        
        ```
        resource "aws_lb_target_group" "carefree_tg" {  name     = "tf-example-lb-tg"  port     = 80  protocol = "HTTP"  vpc_id   = "vpc-0fcc9ed532b1c80b9"  health_check {    interval            = 30    path                = "/index.html"    port                = 80    protocol            = "HTTP"    timeout             = 5    unhealthy_threshold = 2    matcher             = 200  }}
        ```
        
    3. terraform init / apply
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.44.32.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.45.11.png]]
        
          
        
3. Listener.tf 생성
    
    ```
    resource "aws_lb_listener" "front_end" {  load_balancer_arn = aws_lb.test.arn  port              = "80"  protocol          = "HTTP"#  ssl_policy        = "ELBSecurityPolicy-2016-08"#  certificate_arn   = "arn:aws:iam::187416307283:server-certificate/test_cert_rab3wuqwgja25ct3n4jdj2tzu4"  default_action {    type             = "forward"    target_group_arn = aws_lb_target_group.carefree_tg_alb.arn  }}
    ```
    
4. Listener Rule.tf 생성
    
    ```
    resource "aws_lb_listener_rule" "static" {  listener_arn = aws_lb_listener.front_end.arn  priority     = 100  action {    type             = "forward"    target_group_arn = aws_lb_target_group.carefree_tg_alb.arn  }  condition {    host_header {      values = ["example.com"]    }  }}
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.03.40.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.06.55.png]]
    
5. Target Group Attachment 생성
6. Instance 실행 시 기본 프로그램 깔기
    1. instance terraform 파일에 userdata 항목 추가
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.47.10.png]]
        
          
        
    2. 실행할 [userdata.sh](http://userdata.sh) 쉘 파일 작성
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.47.42.png]]
        
    3. terraform apply
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-07_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.55.37.png]]