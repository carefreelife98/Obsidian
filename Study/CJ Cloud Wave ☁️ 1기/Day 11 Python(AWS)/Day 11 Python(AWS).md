  

- AWS : 굳이 언어 사용필요 x
    - 많은 리소스를 사용할 때에 코드를 사용해서 쉽고 빠르게 작업 가능

## AWS CLI 2 다운로드

  

## AWS CLI 명령어

- 명령어는 AWS CLI Document 사이트에 찾아 사용할 수 있다.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.41.34.png]]
    
- [링크]
    
    [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html)
    

  

## CLI 에서 ec2 instance 실행하기

1. instance ID check
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.31.47.png]]
    
2. aws ec2 start-instances —instance-ids (인스턴스 ID)
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.31.00.png]]
    
3. 실행된 모습
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.30.06.png]]
    

  

## CLI 에서 ec2 instance 중지하기

1. instance ID check
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.31.47.png]]
    
2. aws ec2 terminate-instances --instance-ids (인스턴스 ID)
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.36.38.png]]
    
3. 중지된 모습
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.35.14.png]]
    
      
    
      
    

## CLI 에서 ec2 instance 생성하기

  

1. 형태
    - aws ec2 run-instances --image-id AMI코드 --instance-type t2.micro --key-name 키 페어 이름 --subnet-id 서브넷 ID
2. AMI 찾기
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.49.32.png]]
    
3. Subnet ID 찾기
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.33.03.png]]
    
4. AWS CLI 명령어 적용
    - aws ec2 run-instances --image-id ami-0c9c942bd7bf113a2 --instance-type t2.micro --key-name csm-keypair --subnet-id subnet-0b76e94cbb884989c
5. 인스턴스 생성 결과
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.47.02.png]]
    

  

  

## Python 을 활용하여 AWS 다루기

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.08.33.png]]

- [링크]
    
    [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2.html](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ec2.html)
    

  

## Python 코드로 AWS 인프라 구축하기

- [링크] : [https://hwasurr.io/AWS/infrastructure-as-code/](https://hwasurr.io/AWS/infrastructure-as-code/)

  

- 프로비저닝: 사용자가 바로 사용 할 수 있는 상태로 만들어(세팅) 준다.

  

# AWS 인프라 구축 전 준비

### 1. aws 접속

```
aws configure
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.16.13.png]]

  

### 2. openpyxl 설치

```
pip install openpyxl
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.18.11.png]]

  

### 3. boto3 설치

```
pip install boto3
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.18.55.png]]

```
import boto3
```

- **boto3 라이브러리**를 다운받은 후 import 하여 사용가능.

  

# 테스트 - EC2의 Instance ID 가져오기

### ec2_describe.py 생성

  

### ec2 인스턴스 탐색

```
import boto3client = boto3.client('ec2')
```

- 접속 시의 지역 기준 내 모든 ec2 인스턴스를 가져온다.
    - 예제 기준 : ap-northeast-2 (서울)
- client() 함수는 서비스 항목(ec2..) 을 제외한 나머지 파라미터는 입력이 없을 시 default 값으로 설정된다.
    
    ```
    # client() 함수의 모습def client(        self,        service_name,        region_name=None,        api_version=None,        use_ssl=True,        verify=None,        endpoint_url=None,        aws_access_key_id=None,        aws_secret_access_key=None,        aws_session_token=None,        config=None,    ):		return self._session.create_client(            service_name,            region_name=region_name,            api_version=api_version,            use_ssl=use_ssl,            verify=verify,            endpoint_url=endpoint_url,            aws_access_key_id=aws_access_key_id,            aws_secret_access_key=aws_secret_access_key,            aws_session_token=aws_session_token,            config=config,        )
    ```
    

  

### ec2 인스턴스 필터링

```
instance_tags = client.describe_instances(    Filters=[        {            'Name': 'tag:Name',            'Values': ['csm-svr-py'],        }    ])
```

- Dict 자료형을 사용하여 Value 값(인스턴스의 Tag 값)을 통해 자신의 인스턴스만 필터링 하여 가져온다.

  

```
ids = [    instance['InstanceId']    for reservation in instance_tags['Reservations']    for instance in reservation['Instances']       ]print(ids)
```

- 이전 단계에서 꺼낸 instance_tag 내부의 Reservations 내부의 Instances 가 가지고 있는 instance에서 InstanceId 를 찾아 ids에 저장 후 출력한다.
    - 이미 필터링 과정에서 `csm-svr-py` 를 걸러서 가져왔기 때문에 현재 코드에서는 사용되지 않는다.
- 실행 결과
    - csm-svr-py 인스턴스의 ID가 출력되는 것을 볼 수 있다.
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.08.19.png]]