  

  

## Test

  

**Regions**

- AWS Service가 제공되는 물리적인 위치
- Region 별 제공되는 서비스 다름

  

  

**Availability Zone**

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.10.53.png]]

- 1개의 Region에는 최소 2개 이상의 AZ 로 구성.
- 1개의 AZ에는 1개 이상의 데이터 센터로 구성
- 각 가용영역은 상호 100KM 내외의 거리를 이격하여 구성
    - 가용영역 간 네트워크는 전용선으로.

  

  

**route 53 policy**

1. Simple
    - 단순한 방식으로 도메인에 대한 트래픽을 리소스로 전달하는 방식.
    - 1개의 레코드에 Multi Value (2개 이상의 IP 주소) 설정가능.
        - Multi Value 값 설정 시 다중 값을 반환하고 Client에서 1개를 선택하여 사용
        - Alias(별칭) 레코드 값을 적용한 경우 Multi Value 적용 제한
    - Health Check 기능 사용 제한

  

1. Weighted
    - 도메인에 등록된 리소스 마다 가중치를 적용해 트래픽 양을 조절하는 방식
        - 레코드의 유형과 레코드의 이름을 동일하게 구성
        - 각 레코드에 가중치를 설정
        - 모든 레코드의 가중치 값이 0인 경우 모든 레코드에 균등한 가중치 적용
    - Health Check 기능 사용 가능

  

1. Latency
    - Latency (응답 지연 시간) 이 가장 적은 리소스의 정보를 반환하는 방식
        - 일정 기간 측정된 결과 값을 기준으로 라우팅
        - 시간이 지남에 따라 최초 응답 받은 라우팅 값과 다른 값을 반환 할 수 있음.
    - 다중 Region 환경에서는 클라이언트와 가장 근접한 리소스로 연결될 확률이 높음
    - Private Hosted Zone 에 등록된 레코드에도 적용 가능

  

1. Failover
    - Route 53에서 제공하는 상태 검사기를 이용해 정상 리소스 값을 반환하는 방식
    - 하나의 기본 레코드와 하나의 보조 레코드로 구성

  

1. Geolocation
    - Client의 위치 정보를 기반으로 가장 가까운 Region의 리소스 정보를 반환하는 방식
        - 위치 정보와 레코드 반환 값을 Mapping에 구성
        - 사용자 위치에 일치하는 레코드 정보가 없을 경우를 대비해 Default 레코드 생성 필요
    - EDNS 기능을 지원하지 않는 경우 예상과 다르게 동작할 수 있음.
        - Geolocation레코드는 DNS서버의 IP주소를 기반으로 동작.
        - EDNS 기능을 지원할 경우 확장 헤더에 Client의 IP를 추가하여 전달
        - EDNS 기능 지원 DNS 서버
            - Google DNS server
            - Open DNS Server
            - Amazon Route 53

  

  

Amazon Route 53 개요

- AWS에서 제공하는 완전 관리형 DNS 서비스
    - Domain 구입 및 호스팅 관리 지원
    - 다른 도메인 호스팅업체에서 구매한 Domain을 Hosted Zone에 등록하여 사용 가능.
        - Public Hosted Zone
            - 모든 Domain name Query 서비스를 수용하는 호스팅 존
        - Private Hosted Zone
            - VPC 내부 리소스만 사용할 수 있는 Domain Name Query 서비스 제공
- Route. 53에서 생성한 Domain에 대한 ‘상태검사 기능’ 제공
- AWS에서 제공하는 서비스 중 유일하게 100% SLA 제공하는 서비스.

  

  

  

  

**Network 세가지 설계 포인트**

1. **Segmentation**
    1. CIDR-Subnetting
    2. Public & Private
    3. Internet & Internal boundary
    4. Environment (prod, stg, dev)
2. **Connectivity**
    1. Internet Gateway
    2. NAT Gateway
    3. Elastic IP
    4. Site to Site VPN
    5. SSL VPN
    6. Direct Connect
    7. Transit Gateway
    8. Peering
    9. VPC Sharing
    10. Private-Link
3. **Security**
    1. Security Group
    2. Network-ACL
    3. VPC End-Point
    4. WAF
    5. GWLB
    6. Shield
    7. ELB-ACM
    8. VPN

  

  

**VPC**

- 다른 네트워크 환경과 격리된 사용자가 정의한 **가상의 사설 네트워크**
    - 사용자가 IP대역을 **CIDR Block 단위로 할당**하여 네트워크 구성
    - CIDR Block은 **최소 /28 ~ 최대 /16** 범위 내 지정
    - **Secondary CIDR Block은 최대 5개** 추가 가능
- 최대 5개의 VPC 생성 가능
- 생성 후 Region 내 Availability Zone을 자유롭게 지정하여 사용
- 계정 생성 시 172.31.0.0/16 CIDR이 할당된 Default VPC 제공

  

  

  

**security group - acl**

- 상태 저장(Stateful) 방식 방화벽
    - In-Bound Rule: 외부에서 내부로 들어오는 트래픽 정의
    - Out-Bound Rule : 내부에서 외부로 나가는 트래픽 정의
- 상태 비저장 (Stateless) 방식 방화벽
    - In-Bound , Out Bount 모두 동일한 정책 적용 필요
    - 단방향 정책 적용 시 응답 값 반환 제한
- 서브넷 단위에 적용되어 동작
- Network ACL Quotas
    - 최대 규칙 수 : 20개 ~ 40개

  

  

  

**ec2 → instance type 관련**

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-02_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_8.58.36.png]]

- EC2 Instance Type - Naming Rule
    - Instance Type, Instance generation, Attributes, Size 로 구성
    - Instance Type을 통해 Application 또는 서비스 요구사항에 적합한 커퓨팅 리소스 성능 (CPU , Memory, Process..) 선택 가능
- EC2 Instance Type - Family & Type
- EC2 Instance Type - Generation
    - 주기적으로 최신 기술을 반영한 신규 인스턴스 세대 출시
    - 이전 세대 인스턴스 대비 비용은 낮아지고 성능은 향상
    - Region, AZ 별로 지원되는 인스턴스 세대가 다름
        - 신규 배포 리전의 경우 Stable version 인스턴스 세대 위주로 서비스 출시
            - 이전 인스턴스 세대 중 미지원 되는 인스턴스 세대가 있음
- EC2 Instance Type - Attribute & Size

  

  

  

AWS ELB loadbalancer

- 클라이언트의 서비스 요청 트래픽을 다수의 서버로 분산 시켜주는 서비스
    - 클라이언트의 요청을 Listener 로 수신
    - Target Grouop에 등록된 서버의 상태 검사를 통해 정상적인 대상에만 트래픽 라우팅
- 완전 관리형 서비스로 트래픽 요텅에 따라 자동으로 서비스 확장
    - LB 시스템에 대한 Upgrade, Management, Available 에 대한 관리는 AWS 에서 지원
- 다른 AWS 서비스와 연계하여 사용 및 확장 가능
    - AWS ACM 인증서와 연동하여 HTTPS 암호화 통신 지원
    - EC2, Auto-Scaling, Lambdas, ECS, EKS등 컴퓨팅 시스템 지원
    - Route 53, Global Accelerator, Cloud Front 같은 네트워크 시스템과 연동
- Health Check
    - Target Group에 등록된 시스테믕ㄹ 대상으로 Health Check를 통해 정상 응답(상태 코드:200)을 받은 경우에만 해당 시스템으로 트래픽 라우팅
- 종류 네 가지
    - Application Load Balancer
        - OSI 7 계층을 지원하는 로드밸런서
        - HTTP / HTTPS / WebSocket 지원
    - Network Load Balancer
        - OSL L4 계층을 지원하는 로드 밸런서
        - TCP / TLS / UDP 지원
        - Security Group 미지원
        - Elastic IP 설정 가능
    - Gateway Load Balancer
        - OSI L3 계층을 지원하는 로드밸런서
        - internet protocol 지원
        - VPC 내부 모든 트래픽이 GWLB를 거쳐 단일 네트워크 진입점 구성
    - Classic Load Balancer

  

  

**AWS Storage EBS**

- 인스턴스에 연결하여 사용할 수 있는 네트워크 드라이브
- 한번에 하나의 인스턴스에만 연결 가능
    - 하나의 인스턴스에 2개 이상의 EBS 볼륨 연결 가능
    - Volume Type io 1,2는 동일 가용영역의 여러 인스턴스에 연결 가능
    - 인스턴스에 연결되어 있던 EBS 분리 후 다른 EBS 연결 가능
- 특정 가용영역에서만 생성되기 때문에 가용영역간 EBS 볼륨 공유 불가
    - 스냅샷을 이용해 다른 가용영역으로 복제본을 만들어 사용 가능
    - 가용영역 내에서 데이터를 자동으로 복제하여 99.99999% 가용성 보장
- 디스크 용량을 희망하는 만큼 프로비저닝 하여 사용

S3 Storage Class

RDS Aurora

  

Security

- IAM - Policy
- Logging system 종류
- AWS Secret Manager

  

Management

- Metric?