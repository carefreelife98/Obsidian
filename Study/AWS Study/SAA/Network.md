  

- [Q4]  
    EC2 에서 돌아가는 Application은 S3에 저장된 Logs 를 처리.  
    EC2 Instance 가 인터넷 연결 없이 S3 에 Access 하는 법  
    [S3에 대한 Private Network 연결을 제공하는 솔루션]
    - VPC ↔ S3 간 인터넷을 통하지 않는 연결은 `S3 VPC Gateway Endpoint` 이다.
    - S3 버킷에 대한 게이트웨이 VPC 엔드포인트를 생성

  

- [Q29]  
    UDP 연결을 사용하는 VoIP (Voice over Internet Protocol) 서비스 제공.  
    해당 서비스는 Auto Scaling Group에서 실행되는 EC2 로 구성됨.  
    해당 서비스는 다중 Region에 배포 중.  
    지연 시간이 가장 짧은 Region 으로 사용자를 라우팅 해야함.  
    지역 간 자동 장애 조치 필요.  
    어떤 솔루션이 필요한가?
    
    - UDP 연결은 NLB.
    - 대기 시간이 가장 짧은 Region 라우팅 + UDP 사용 = AWS Global Accelerator
    
    **[Solution]**
    
    1. NLB(Network Load Balancer) 및 연결된 대상 그룹을 배포.
    2. 대상 그룹을 Auto Scaling Group과 연결.
    3. 각 Region에서 ALB를 AWS Global Accelerator Endpoint 로 사용.
    
      
    

  

- [Q68]  
    솔루션 설계자는 회사의 온프레미스 인프라를 AWS 로 확장하기 위해 새로운 하이브리드 아키텍처를 설계.  
    이 회사는 AWS 리전에 대해 일관되게 짧은 지연 시간과 고가용성 연결이 필요.  
    회사는 비용을 최소화해야 하며 기본 연결이 실패할 경우 더 느린 트래픽을 기꺼이 받아들입니다.  
    솔루션 설계자는 이러한 요구 사항을 충족하기 위해 무엇을 해야 합니까?
    - **[Solution]**
        1. 리전에 대한 AWS Direct Connect 연결을 프로비저닝.
        2. 기본 Direct Connect 연결이 실패하는 경우 백업으로 VPN 연결을 프로비저닝.
    - 어떤 경우에는 이 연결만으로는 충분하지 않다.
        - 항상 DX 의 백업으로 폴백 연결을 보장하는 것이 좋음.
    - `AWS Site-To-Site VPN` 으로 구현하는 것이 `비용 효율적`
    - `VPN 과 Direct Connect 는 같이 사용할 수 있음.`