
**고가용성 저장소**
- S3

**AWS DataSync 에이전트**
- 에이전트는 온프레미스 스토리지에서 AWS 로 데이터를 복사할 때 일반적으로 사용

**AWS Glue 작업 북마크**
- AWS Glue 가 유지 관리하는 데 도움이 됩니다. 상태 정보를 제공하고 오래된 데이터의 재처리를 방지

**Snowball Edge Storage Optimized** 
- 수십 테라바이트(TB)~페타바이트(PB)의 고용량 데이터를 안전하고 신속하게 AWS 로 전송해야 할 때 선택할 수 있는 가장 적합한 옵션

**AWS Glue** 
- 70 개 이상의 다양한 데이터 소스를 검색하여 연결하고 중앙 집중식 데이터 카탈로그에서 데이터를 관리할 수 있습니다. 추출, 변환, 로드(ETL) 파이프라인을 시각적으로 생성, 실행, 모니터링하여 데이터 레이크에 데이터를 로드할 수 있습니다.

**Kinesis Data Firehose** 
- 스트리밍 ETL 솔루션입니다. 스트리밍 데이터를 캡처하고 변환한 후 Amazon S3, Amazon Redshift, Amazon OpenSearch Service 및 Splunk 로 로드하여 이미 사용하고 있는 기존 비즈니스 인텔리전스 도구 및 대시보드를 통해 거의 실시간으로 분석할 수 있습니다.

**CloudWatch Logs** 
- 구독을 통해 실시간에 가깝게 Amazon OpenSearch Service 클러스터로 수신한 데이터를 스트리밍하도록 CloudWatch Logs 로그 그룹을 구성할 수 있습니다.

**AWS Firewall Manager**
- [여러 계정]에서 SQL 주입, XSS 공격, DDoS 등을 방어

**상태비저장, 시작 및 중지 가능**
- 스팟 인스턴스

**OAC 또는 OAI**
- S3 + CloudFront 를 사용하는 상황에서 S3 에 직접 액세스하는 것을 막기
- CloudFront 는 Amazon S3 오리진에 인증된 요청을 전송하는 두 가지 방법으로 오리진 액세스 제어(OAC)와 오리진 액세스 ID(OAI)를 제공합니다. OAC 는 다음을 지원하므로 OAC 를 사용하는 것이 좋습니다.

**기본 운영 체제에 대한 액세스 유지**
- Amazon RDS Custom.

**PrivateLInk**
- VPC 내의 특정 애플리케이션이나 서비스에 연결

**온프레미스 데이터베이스를 Amazon Aurora 로 마이그레이션**
- 지속적인 복제 작업 생성
- AWS Database Migration Service(AWS DMS) 복제 서버를 생성
	- DMS 는 대상이 소스와 동기화된 상태를 유지하도록 지속적 복제를 지원
	- 서로 다른 데이터베이스 플랫폼 간의 이기종 마이그레이션을 지원

**EC2 Compute Savings Plans**
-  최대 66%까지 비용을 절감할 수 있는 가장 유연한 요금 모델

**Global Accelerator**
- TCP/UDP+엔드포인트에 입력할 수 있는 고정 IP 주소를 가져야 한다

**예약 인스턴스**
- 기본 사용량 수준 예상 가능 시

**Amazon S3 Inventory**
- 비즈니스, 규정 준수 및 규제 요건에 대한 객체의 복제 및 암호화 상태를 감사하고 보고할 수 있습니다. 
- 또한 Amazon S3 동기식 List API 작업의 대안으로 Amazon S3 인벤토리를 사용하면 비즈니스 워크플로 및 빅 데이터 업무를 단순화하고 속도를 높일 수 있습니다.

**S3 객체 수정 및 삭제 방지 - S3 ObjectLock.** 
- **규정 준수 모드**에서는 AWS 계정의 루트 사용자를 포함하여 어떤 사용자도 보호 객체 버전을 덮어쓰거나 삭제할 수 없습니다
- **거버넌스 모드**를 사용하면 대부분의 사용자가 개체를 삭제하지 못하도록 보호하지만 필요한 경우 일부 사용자에게 보존 설정을 변경하거나 개체를 삭제할 수 있는 권한을 계속 부여할 수 있습니다.

**CloudFront vs AWS Global Accelerator**
- CloudFront 는 로컬 캐시를 사용하여 응답을 제공하고, AWS Global Accelerator 는 요청을 프록시하고 응답을 위해 항상 애플리케이션에 연결

**Athena - KPI & AWS LakeFormation** 
- KPI 로 표시하기 위해선 Athena 가 필요하고, Athena 는 S3 에 쿼리
- 형식이 다른 데이터를 한 곳에 모을 때 적절한 AWS LakeFormation 필요
	- 분석 목적으로 모든 데이터를 보관하는 중앙 위치로 사용
	- AWS Glue 는 Amazon S3 데이터 레이크의 필수 구성 요소이며 최신 데이터 분석을 위한 데이터 카탈로그 및 변환 서비스 제공

**Athena vs Kinesis Data Analytics**
- Amazon Athena 는 스트리밍 데이터에 대한 일회성 쿼리를 실행하기 위한 최상의 선택입니다.  
- Amazon Kinesis Data Analytics 는 스트리밍 데이터를 실시간으로 분석할 수 있는 쉽고 친숙한 표준 SQL 언어를 제공하지만 **일회성 쿼리가 아닌 지속적인 쿼리**를 위해 설계

**HPC** 
- Amazon FSx for Lustre

**메시지 처리에 실패하면**
- SQS Dead Letter Queue.

CloudFront 는 Amazon S3 오리진에 인증된 요청을 전송하는 두 가지 방법으로 오리진 액세스 제어(OAC)와 오리진 액세스 ID(OAI)를 제공

**특정 국가 또는 지리적 위치로부터의 요청을 허용하거나 차단**
- AWS WAF 를 사용

**Amazon CloudFront - 필드 레벨 암호화**
- CloudFront
	- HTTPS 를 통해 오리진 서버에 대한 종단 간 보안 연결을 적용할 수 있습니다.
- 필드 레벨 암호화
	- 추가 보안 레이어를 추가하여 시스템 처리 전체에서 특정 데이터를 보호하고 특정 애플리케이션만 이를 볼 수 있도록 합니다.

**RDS / Aurora 연결 수가 많음**
- RDS Proxy 사용
	- RDS 프록시를 사용하여 예기치 않은 데이터베이스 트래픽 급증을 처리할 수 있습니다.

**DynamoDB 와 DAX** 
- 가 결합되면 성능을 한 단계 업그레이드하여 읽기 중심의 워크로드에서 초당 수백만 개의 요청에도 마이크로초의 응답 시간을 지원
- DAX 는 장애 탐지, 장애 복구, 소프트웨어 패치와 같은 일반적인 관리 작업 상당 부분을 자동화


