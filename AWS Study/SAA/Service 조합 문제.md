  

- [Q18]  
    큰 이미지를 더 작은 압축 이미지로 변환하는 마이크로서비스를 설계.  
    사용자가 웹 인터페이스를 통해 `이미지를 업로드`하면 마이크로 서비스는 이미지를 `Amazon S3 에 저장`하고, `AWS Lambda 함수로 이미지를 처리 및   압축`하고, `다른 S3 에 압축된 형태로 이미지를 저장`.  
    내구성이 있는 `상태 비저장` 구성 요소를 사용하여 이미지를 자동으로  
    처리하는 솔루션을 설계.
    - `S3 Events → SQS Queue → Lambda`
        1. Amazon SQS 대기열을 생성.  
            이미지가 S3 에 업로드될 때 SQS 대기열에 알림을 보내도록 S3 구성.
        2. Amazon SQS 대기열을 호출 소스로 사용하도록 Lambda 함수를 구성.
        3. SQS 메시지가 성공적으로 처리되면 대기열에서 메시지를 삭제.
    - SQS 대기열을 생성하고 이미지가 S3 에 업로드될 때 SQS 대기열에 알림을 보내도록 S3 버킷을 구성하면 Lambda 함수가 상태 비저장 및  
        내구성 방식으로 트리거 됨.
    - SQS 대기열을 호출 소스로 사용하도록 Lambda 함수를 구성하고 성공적으로 처리된 후 대기열에서 메시지를 삭제하면 Lambda 함수가 상태 비저장 및 내구성 방식으로 이미지를 처리

  

- [Q21]  
    전자 상거래 회사는 AWS 에서 하루 1 회 웹 사이트를 시작.  
    피크 시간 동안 밀리초 지연 시간으로 시간당 수백만 개의 요청을 처리.  
      
    A. Amazon S3 를 사용하여 다른 S3 버킷에 전체 웹 사이트를 호스팅합니다. Amazon CloudFront 배포를 추가합니다. S3 버킷을 배포의 오리진으로 설정합니다. Amazon S3 에 주문 데이터를 저장합니다.  
      
    B. 여러 가용 영역의 Auto Scaling 그룹에서 실행되는 Amazon EC2 인스턴스에 전체 웹 사이트를 배포합니다. ALB(Application Load Balancer)를 추가하여 웹 사이트 트래픽을 분산합니다. 백엔드 API 에 대해 다른 ALB 를 추가하십시오. MySQL 용 Amazon RDS 에 데이터를 저장합니다.  
      
    C. 컨테이너에서 실행되도록 전체 애플리케이션을 마이그레이션합니다. Amazon Elastic Kubernetes Service(Amazon EKS)에서 컨테이너를 호스팅합니다. Kubernetes 클러스터 자동 확장 처리를 사용하여 트래픽 버스트를 처리할 포드 수를 늘리거나 줄입니다. MySQL 용 Amazon RDS 에 데이터를 저장합니다.  
      
    D. Amazon S3 버킷을 사용하여 웹 사이트의 정적 콘텐츠를 호스팅합니다. Amazon CloudFront 배포를 배포합니다. S3 버킷을 오리진으로 설정합니다. 백엔드 API 에 Amazon API Gateway 및 AWS Lambda 함수를 사용합니다. Amazon DynamoDB 에 데이터를 저장합니다.  
    - Amazon S3 를 사용하여 다른 S3 버킷에 전체 웹 사이트를 호스팅 (X)
        - S3+CloudFront 조합은 정적 웹 사이트 호스팅 만을 위한 것.
    - RDS 는 기본적으로 Auto Scaling 을 사용하지 않음.  
        따로 켜야하는데 해당 선택지엔 Auto Scaling 을 사용한단 언급이 없음.
        - RDS 사용 시 Auto Scaling 기능을 사용한다는 언급이 있어야 함.
    - [정답]  
        Amazon S3 버킷을 사용하여 웹 사이트의 정적 콘텐츠를 호스팅.  
        Amazon CloudFront 배포를 배포. S3 를 오리진으로 설정.  
        백엔드 API 에 Amazon API Gateway 및 AWS Lambda 함수를 사용. Amazon DynamoDB 에 데이터를 저장.
        - 정적인 웹사이트 요소들은 S3 + CloudFront 로 빠르게 제공하고, API Gateway 에서 Lambda 함수를 호출해 DynamoDB 에 데이터 저장 가능.
        - DynamoDB 는 확장성이 뛰어나고 밀리초 단위 액세스를 지원하는 데이터베이스 유형.
- [Q25]  
    현재 애플리케이션은 AWS Lambda 함수를 사용하여 Amazon API Gateway 를 통해 정보를 수신하고 Amazon Aurora PostgreSQL 데이터베이스에 정보를 저장.  
    데이터베이스에 로드해야 하는 대용량 데이터를 처리하기 위해 Lambda 할당량을 크게 늘려야 합니다.  
    확장성을 개선하고 구성 노력을 최소화하기 위해 새로운 설계 권장 사항은?  
      
    A. Lambda 함수 코드를 Amazon EC2 인스턴스에서 실행되는 Apache Tomcat 코드로 리팩터링합니다. 네이티브 JDBC(Java Database Connectivity) 드라이버를 사용하여 데이터베이스를 연결합니다.  
      
    B. 플랫폼을 Aurora 에서 Amazon DynamoDProvision a DynamoDB Accelerator(DAX) 클러스터로 변경합니다. DAX 클라이언트 SDK 를 사용하여 DAX 클러스터에서 기존 DynamoDB API 호출을 가리킵니다.  
      
    C. 두 개의 Lambda 함수를 설정합니다. 정보를 수신할 하나의 기능을 구성하십시오. 정보를 데이터베이스에 로드하도록 다른 기능을 구성하십시오. Amazon Simple Notification Service(Amazon SNS)를 사용하여 Lambda 함수를 통합합니다.  
      
    D. 두 개의 Lambda 함수를 설정합니다. 정보를 수신할 하나의 기능을 구성하십시오.  
    정보를 데이터베이스에 로드하도록 다른 기능을 구성하십시오. Amazon Simple Queue Service(Amazon SQS) 대기열을 사용하여 Lambda 함수를 통합합니다.
    
    - 정답 : D
        
        - 대기열(SQS)로 병목 현상을 방지할 수 있습니다.  
            대량의 데이터 처리 + 확장성 개선 = SQS queue + Lambda 조합.
        
          
        
    
      
    

  

- [Q36]  
    AWS 클라우드에서 애플리케이션을 구축.  
    애플리케이션은 두 AWS 리전의 Amazon S3 버킷에 데이터를 저장.  
    AWS Key Management Service(AWS KMS) 고객 관리형 키를 사용하여 S3 버킷에 저장된 모든 데이터를 암호화 해야함.  
    두 S3 버킷의 데이터는 동일한 KMS 키로 암호화 및 복호화 해야함.  
    데이터와 키는 두 지역 각각에 저장되어야 함.  
    최소한의 운영 오버헤드를 가진 솔루션은?
    - **[Solution]**
        1. 고객 관리형 다중 지역 KMS 키를 생성.
        2. 각 리전에서 S3 버킷을 생성.
        3. S3 버킷 간의 복제를 구성.
        4. 클라이언트 측 암호화와 함께 KMS 키를 사용하도록 애플리케이션을 구성.
    - AWS KMS 키로 암호화된 개체를 업로드하려면 키와 S3 버킷이 동일한 AWS 리전에 있어야 함.

  

- [Q51]  
    REST API 로 검색하기 위해 주문 배송 통계를 제공하는 애플리케이션을 개발 중.  
    배송 통계를 추출하고 데이터를 읽기 쉬운 HTML 형식으로 구성하고 매일 아침 여러 이메일 주소로 보고서를 보내려고 함.  
    이러한 요구 사항을 충족하기 위해 솔루션 설계자는 어떤 단계 조합을 취해야 합니까?  
    1. AWS Lambda 함수를 호출하여 데이터에 대한 애플리케이션의 API 를 쿼리하는 Amazon EventBridge(Amazon CloudWatch Events) 예약 이벤트를 생성
    2. Amazon Simple Email Service(Amazon SES)를 사용하여 데이터 형식을 지정하고 보고서를 이메일로 보냅니다.
- [Q52]  
    회사에서 온프레미스 애플리케이션을 AWS 로 마이그레이션.  
    애플리케이션은 수십 기가바이트에서 수백 테라바이트까지 다양한 크기의 출력 파일을 생성.  
    애플리케이션 데이터는 표준 파일 시스템 구조로 저장되어야 함.  
    고가용성이며 최소한의 운영 오버헤드를 가진 자동으로 확장되는 솔루션?
    - 다중 AZ Auto Scaling 그룹의 Amazon EC2 인스턴스로 애플리케이션을 마이그레이션.
        - 고가용성, 최소 운영 오버헤드이므로 Auto Scaling
    - 스토리지에 Amazon Elastic File System(Amazon EFS)을 사용.
        - EFS 는 표준 파일 시스템으로 자동 확장되며 가용성이 높다.
        - EFS vs EBS 를 비교해보면 보통은 EFS 가 정답인 경우가 많다
            - EBS 는 여러 EC2 인스턴스에서 동시 접속할 수 없다는 단점이 치명적.

  

- [Q75]  
    한 회사에서 애플리케이션의 성능을 개선하기 위해 다계층 애플리케이션을 온프레미스에서 AWS 클라우드로 이동하려고 함.  
    애플리케이션은 RESTful 서비스를 통해 서로 통신하는 애플리케이션 계층으로 구성.  
    한 계층이 오버로드되면 트랜잭션이 삭제되는 오류 발생.  
    이러한 문제를 해결하고 애플리케이션을 현대화하는 솔루션은?
    - **[Solution]**
        1. Amazon API Gateway 를 사용하고 애플리케이션 계층으로 AWS Lambda 함수에 트랜잭션을 전달.
        2. Amazon Simple Queue Service(Amazon SQS)를 애플리케이션 서비스 간의 통신 계층으로 사용.
    - `RESTful API = API Gateway 사용.`
    - `트랜잭션 삭제되는 문제 = SQS.`