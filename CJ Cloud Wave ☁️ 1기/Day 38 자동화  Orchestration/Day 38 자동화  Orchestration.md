  

**CI / CD 를 위한 Tool 3가지**

- Multi Cloud (다양한 환경에서 지원 및 사용 가능)
    - Ansible
    - Terraform
- Single Cloud (AWS 에서만 사용 가능)
    - CloudFormation & AWS CLI

  

  

**Ansible**

- Stateless

  

**TerraForm & CloudFormation**

- Stateful
- IaC

  

**Ansible**

- EC2 100대에 설정된 UTC 시간대를 KST 시간으로 바꿀때
- 이미 만들어진 기존 리소스에 간단한 설정을 추가하는 것에 사용

  

**Terraform**

- 백지 상태에서 새로 쌓아올리는 작업시에 사용

  

  

# Ansible

- Ansible 프로젝트는 Red Hat 이 후원하는 오픈 소스 커뮤니티
- Ansible Playbook에서 IT 애플리케이션 환경을 완벽하게 설명하는 간단한 자동화 언어
- Ansible Engine은 Ansible 커뮤니티 프로젝트에서 구축된 지원 제품
- Ansible Tower는 UI 및 RESTful API 로 Ansible 자동화 (커뮤니티 또는 엔진)를 제어, 보안, 관리 및 확장하기 위한 엔터프라이즈 프레임워크
- 자체 Agent가 필요없는 Agentless 프로그램
    - Agent 대신 Open SSH / SSH Daemon 를 사용하여 통신.
    - 자체 Agent가 존재하면 특정 Port번호가 존재하지만, SSH 를 사용하기 때문에 :22 만 열어주면 된다.
- 크로스 플랫폼 - Linux, Windows, UNIX
    - 물리적, 가상, 클라우드 및 네트워크의 모든 주요 OS 변형에 대한 에이전트 없는 지원
- 가독성이 좋은 YAML 사용
- 애플리케이션에 대한 완벽한 설명
- 버전 제어
    - Playbook은 일반 텍스트 이며, 기존 버전제어의 코드처럼 취급
- 동적 인벤토리
    - Hostfile과 같은 서버의 목록을 관리
    - 인프라, 위치 등에 상관없이 모든 서버를 100% 캡쳐
- 다른 제품과의 연동성이 좋은 오케스트레이션 - HP SA, Puppet, Jenkins, RHNSS

  

## Ansible Main Topic 5

- SSH
- Inventory
    - 도메인으로 관리하는 것을 권장하나 쉽지 않다.
- ansible.cfg
- hosts

  

## Ansible : DevOps의 언어

- DevOps 의 핵심 : 커뮤니케이션
- Ansible은 IT 전반에서 읽고 쓸 수 있는 최초의 자동화 언어
- Ansible 은 전체 애플리케이션 라이프 사이클과 지속적인 전달 파이프 라인을 자동화 할 수 있는 자동화 엔진

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2016-12-24_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.24.48.png]]

## Hosts File

1. Hosts file
2. DNS
    
    1. A record
    2. CName Record
    
      
    
      
    

# Ad-Hoc Commands

- Ad-Hoc Commands 는 신속하게 수행할 수 있는 Ansible 작업이지만 나중에 저장하지 않음
- One-Liner 명령어
    - 여러 줄에 명령어를 작성하지 않고 (쉘 스크립트) 한 줄로 쭉 작성하는 방식
- Ad-Hoc Commands 일반 옵션
    
    ```
    # ad-hoc$ -m MODULE_NAME, —module-name=MODULE_NAME
    ```
    
      
    
      
    
      
    

# Playbook

## Roles

- Role은 단독으로 Play하는 것보다 더 쉽게 공유할 수 있는 밀접하게 관련된 Ansible Contents Package
    - 복잡한 ~

  

## Ansible Galaxy

- Ansible Galaxy 는 Ansible 콘텐프를 찾고 재사용하고 공유하기 위한 Hub.
- AWS Market Place와 비슷함.

  

  

## [개발] Local vs Cloud9

1. **Local (VSCode, Intellij, Eclipse)**
    
    - 인터넷을 통해 AWS에 접근 (불확실성 증가)
    
      
    
2. Cloud 9
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.42.22.png]]
    
    - IAM의 Access / Secret key (API 키)를 통해 인증하여 사용할 수 있음.
        - Access / Secret key 만 있으면 어디서든 AWS CLI를 통해 리소스 생성 가능.
        - 따라서 보관상 주의하지 않으면 보안사고 발생
    - 다른 곳을 통하지 않고 AWS 내에서 개발.
    - EC2 비용 발생

  

[참고] Cloud 9은 시간 제한을 통해 불필요한 요금 청구를 방지 할 수 있으므로 굳이 삭제하지 않아도 된다.

  

# [IAM]

![[KakaoTalk_Photo_2023-08-03-14-46-23.jpeg]]

Group (권한→ dev / ops)

- User
    - Policy
- Policy (Json)
    - 다른 Entity 에 적용되는 순간 효과를 발휘

  

Role

- 마찬가지로 Policy를 통해 AWS 리소스에게 권한을 부여하여 접근할 수 있게함.
    - 해당 리소스와 연결되거나 연결되어야 할 또 다른 리소스에 접근할 수 있는 권한도 주어야 한다.
    - 예) EC2 접근권한 → EC2에서 접근해야할 S3접근권한… 등

  

```
$ aws configure list
```

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.42.44.png]]

- iam-role 확인
- access / secret key 자동 발급

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.43.36.png]]

- 사용 인스턴스의 IAM Role 확인

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.43.14.png]]

- AWS IAM 에서 해당 role을 확인 할 수 있다.

  

  

  

# Ansible 소개

1. Configureation Management System
    - Provision
    - Orchestration
    - System 설정을 code로 관리
    - **멱등성**

  

# Ansible 실습

  

**AWSCLI**

- ansible test를 위해 세 개의 인스턴스 생성

```
aws ec2 run-instances \  --image-id resolve:ssm:/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 \  --count 3 \  --instance-type t2.micro \  --key-name csm-keypair \  --security-group-ids sg-075d131ed182177f8 \  --subnet-id subnet-0d58fc5216e87a0c4 \
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.24.37.png]]

  

Ansible 설치

```
sudo amazon-linux-extras install ansible2 -yyum install ansible -ycd /etc/ansible
```

  

Ansible 확인

```
ansible --version
```

  

## EC**2 as Bastion**

> Bastion 개념을 사용

> Local의 인터넷 망에서 바로 접속할 수 없는 private EC2에 접속하는 방법

> Public EC2 를 거쳐 private EC2 로 접속하기

![[KakaoTalk_Photo_2023-08-03-15-09-34.jpeg]]

- Public EC2에는 Public IP가 있으므로 인터넷 망에서 접근가능.
- 또한 Pulic EC2는 Private IP도 있으므로 해당 사설망을 이용하여 Private EC2에 접근하게 된다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.31.37.png]]

- 접속 대상 Private EC2

  

```
chmod 400 csm-keypair.pem
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.21.08.png]]

- Cloud 9 EC2 (Public)을 Bastion으로 삼아 Private EC2에 접속한 모습.

  

## Ansible node

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.07.41.png]]

- 위처럼 확인해 볼 수 있는 내가 가진 private 키와 AWS에 존재하는 Public 키와 조합이 되면 통신이 되는 것.

  

  

```
# targethost: 설치 대상 호스트의 IP 주소 정보 목록[targethost]xxx.xxx.xxx.xxx## targethost_vars: Ansible 실행시 이용할 변수[targethost:vars]ansible_ssh_port=22ansible_ssh_user=ec2-useransible_ssh_private_key_file=/etc/ansible/pemfile/xxx.pemansible_become=yeshost_key_checking=False
```

## **localhost ping**

```
ansible localhost -m pingansible all -m ping
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.33.01.png]]

  

  

## Ansible은 Python 기반 제작 → Python 버전 중요

  

```
$ ansible all -m shell -a "hostname"$ ansible all -m shell -a "whoami"
```

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.42.55.png]]