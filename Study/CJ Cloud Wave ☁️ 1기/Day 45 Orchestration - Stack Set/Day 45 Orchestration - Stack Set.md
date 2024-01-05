mapping_stackset_iam.yaml

```
AWSTemplateFormatVersion: '2010-09-09'Description: This CloudFormation StackSet deploys two AWS provided CloudFormation templates that add Administrator and Execution Roles required to use AWSCloudFormationStackSetAdministrationRoleResources:  AWSCloudFormationStackSetAdministrationRole:    Type: AWS::CloudFormation::Stack    Properties:      TemplateURL: https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetAdministrationRole.yml       TimeoutInMinutes: '3'  AWSCloudFormationStackSetExecutionRole:    Type: AWS::CloudFormation::Stack    Properties:      TemplateURL: https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetExecutionRole.yml       TimeoutInMinutes: '3'      Parameters:        AdministratorAccountId : !Ref 'AccountID'Parameters:  AccountID:      Type: String      Description: Your AWS Account ID      MaxLength: 12      MinLength: 12
```

  

시험 : 프린트 20문제

아침 9시

객관식 + 단답형

# Test

## Ansible 소개

  

  

# SQS

  

# CDK

워크스테이션에서 AWS Cloud Development Kit(CDK)를 설정하고 구성하는 방법과 코드형 인프라를 통해 당신의 첫 AWS 리소스를 만드는 방법을 설명합니다.

AWS CDK는 익숙한 프로그래밍 언어(JavaScript, TypeScript, Python, Java, C#, Go 등)를 사용하여 클라우드 애플리케이션 리소스를 정의할 수 있는 오픈 소스 소프트웨어 개발 프레임워크입니다. 작성하는 코드는 CloudFormation(CFN) 템플릿에 트랜스파일(transpile)되고 [[AWS CloudFormation](https://aws.amazon.com/cloudformation/)]을 사용하여 인프라를 생성합니다.

## AWS CDK CLI 설치

```
# Global (전역) 으로 n 설치npm install -g aws-cdk# cdk가 정상적으로 설치되었는지 확인cdk --version
```

  

## AWS 계정 Boot Strapping

```
# Get the account IDaws sts get-caller-identity# Bootstrap the accountcdk bootstrap aws://ACCOUNT-NUMBER/REGION
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.17.50.png]]

  

## 처음으로 CDK 프로젝트 생성

- AWS Cloud Development Kit(CDK)를 사용하여 인프라를 생성합니다.
- python 을 사용하여 새 인프라 프로젝트를 생성하기 위해 사용됩니다. 또한 간단한 리소스를 작성하는 방법과, CDK 코드를 합성 및 배포하는 방법에 대해 알아봅니다. 동기화는 CDK가 인프라 코드를 AWS CloudFormation 템플릿으로 전환하는 방식입니다.

  

## 새 CDK 프로젝트 만들기

- CDK CLI 사용
- 시스템에 빈 디렉토리를 생성하고 해당 디렉토리로 이동.

```
# 새 TypeScript CDK 프로젝트 생성mkdir (디렉토리 이름)cd (생성한 디렉토리)cdk init --language python
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.27.31.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.45.46.png]]

```
cdk init
```

- cdk init 을 통해 생성된 cdk 프로젝트의 구조를 살펴보자.

  

## 인프라 생성

- 프로젝트 구축을 시작하려면 일반적으로 사용자가 정의하는 논리적으로 분리된 가상 네트워크를 생성하는 것부터 시작하는 게 좋습니다.
- 이를 [Amazon Virtual Private Cloud(VPC)](https://aws.amazon.com/vpc)라고 합니다. 첫 VPC를 생성하기에 앞서 cdk init 명령이 생성한 파일에 대해 이해해야 합니다.

  

## 애플리케이션에서 몇 가지 정의하기

- [app.py](http://app.py) 파일의 accout 및 region 수정 후 주석 해제.

```
vi app.py
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.39.41.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.41.19.png]]

  

## VPC용 코드

- 두 개의 퍼블릭 서브넷이 있는 VPC를 두 개의 가용성 영역에 걸쳐 분산되도록 생성할 것
- 코드 작성을 본격적으로 수행하기에 앞서 구축 라이브러리 모듈에 대해 설명하고 이를 설치해야 합니다.
- 프로비저닝 중인 인프라에 필요한 종속성만 추가할 수 있도록 여러 서비스가 모듈로 패키지되어 있습니다.
- 모듈은 단일 서비스에 대한 것(예: AWS Amplify) 또는 여러 서비스에 대한 것(예: Amazon EC2)이 있습니다.
- 이 안내서에서는 AWS VPC에 대한 지원도 포함하는 Amazon EC2 모듈이 필요합니다. 모듈을 설치하기 위해 npm을 사용합니다. 프로젝트 디렉터리에 있는 동안 다음 명령을 실행합니다

```
npm install @aws-cdk/aws-ec2
```

이렇게 하면 작업에 필요한 모든 모듈이 설치됩니다. package.json 내부를 확인하면, 거기에도 설치되어 있음을 볼 수 있습니다.

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.47.34.png]]

  

## cdk_python_stack.py 코드 작성

```
from aws_cdk import Stackfrom constructs import Constructimport aws_cdk.aws_ec2 as ec2class CdkPythonStack(Stack):    def __init__(self, scope: Construct, construct_id: str, **kwargs) -> None:        super().__init__(scope, construct_id, **kwargs)        # The code that defines your stack goes here        vpc = ec2.Vpc(self, "VPC", subnet_configuration=[            ec2.SubnetConfiguration(                name='PublicSubnet',                subnet_type=ec2.SubnetType.PUBLIC,                cidr_mask=24            ),            ec2.SubnetConfiguration(                name='PrivateSubnet',                subnet_type=ec2.SubnetType.PRIVATE_WITH_NAT,                cidr_mask=24            )        ])
```

  

## 배포 하기

```
cdk deploy
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.26.10.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.30.49.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.36.13.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.37.21.png]]

- cdk_python_stack.py 코드에서 VPC 생성 과정에서 Subnet 옵션 중 Availability Zones 옵션 or maxAzs 옵션을 주지 않으면 Default로 3개의 AZ에 서브넷이 생성된다.
- Public 과 Private 각각 3개의 AZ(a, b, c)에 Subnet이 생성된 것을 볼 수 있다.