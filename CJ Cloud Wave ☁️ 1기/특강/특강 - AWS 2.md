AWS Direct Connect

  

  

Amazon Open Search Service

- 상품검색 서비스

  

Amazon Simple Email Service

- 환영 / 가입 / 쿠폰 이메일 발송

  

큐레이션 서비스

- 상품 구매 / 장바구니 등의 데이터를 기반으로 상품 추천

  

Elastic Beanstalk

- parse 형 서비스
- EC2 배포 후 확인해보면 웹 관련 프로그램 기본 설치 되어있음.
- 소스만 작성해주면 됨
- 템플릿 같은 느낌

  

Amazon Kinesis

- 데이터 수집 서비스

  

## AWS 구축 시 고려사항

  

## 1. Fault Tolerance / High Availability

1. Fault Tolerance
    1. AZ 이중 / 다중화
        1. NAT Gateway 는 무조건 Zone 마다 하나씩.
        2. 가용영역 전부 최소 이중화
2. Instance Backup & Restore
3. 장애 대응
4. 장애 대응을 위한 모니터링
    1. CloudWatch Dashboard
    2. CloudWatch - Slack Alarm
        1. CloudWatch
        2. Amazon SNS
        3. AWS Lambda
        4. Slack
    3. Data Dog (돈이 있으면 가장 좋은 툴)
    
5. Scalability
    1. Auto Scaling Group
    2. Read Replica

  

## 2. **RDS**

- Aurora → Cluster 식의 구성이 가능한 장점.
    - Reader instance 사용하여 트래픽을 상당히 줄일 수 있음

  

## 3. Security

1. Account or VPC Design
    
    1. account
        1. [권고] 서비스 별로 사용자 계정 분리 (서로 통신 가능)
        2. 개발 / 네트워크 / 구축 / 운영 계 단위로 계정 생성 하여 사용
    2. 개발 / 운영 간의 VPC는 무조건 구분하라.
    
      
    
2. AWS
    1. Service Encryption
        1. 암호화 관련 설정이 보이면 걸어라?
    2. Security Group
        1. 0.0.0.0/0 말고 최소한의 권한을 걸어 사용해볼것
        2. SG는 Stateful
            - inbound를 아무 포트나 원하는 포트로 잡게 되면 자동으로 Outbound는 랜덤으로 나가게 되어 있음.
                
                → 따로 Outbound 설정할 필요는 없지만 할 수 있으면 좋은 추세.
                
        3. Encryption 굉장히 중요. 그냥 넘어가면 이게 무엇인지 알 길이 없음
    3. NACL
        1. 기본적으로 필수 구성은 아님.
            1. 보안관심있는 경우 많이 적용해달라고 함.
        2. Stateless
            1. inbound, outbound 둘 다 필수로 정의 해주어야 함.
                1. 예) inbound 80 , outbound 1024 ~ 65535
3. IAM Policy & Role
    1. IAM 구성 요소
        1. 요청
        2. 인증과정
        3. Action
        4. 대상 Resource
    2. IAM 권한
        1. Amazon에서 제공하는 IAM Policy는 제한적.
            1. 그룹 별, 그룹의 구성원 별로 필요한 Policy를 적용해 주기 위해 JSON을 사용하여 직접 IAM Policy를 짤 수 있어야 한다.
        2. 대표 사례
            1. Root 계정은 정해진 사유가 아닌 경우 사용 불가 + Root 계정 접근 모니터링 권고
            2. Administrator Access 권한은 반드시 인프라 담당자 기준으로만 할당
            3. User Group이 많아서 문제될 것은 없다. 권한 세분활ㄹ 위해 여러 Group이 있는 것이 오히려 좋다.
            4. 인프라 담당자가 아닌 다른 담당자의 사용자 계정에는 최소 권한 할당을 목표
            5. User 내 AccessKey 발급은 지양 (Role Base 호출 권고)
            6. AWS Console 계정과 Programmatic 계정은 분리 권고
            7. 기타, 사용자 계정 MFA 활성화, 비밀번호 정책 적용 등