  

- [Q12]  
    ALB 뒤의 EC2 에서 Web Application 호스팅 중.  
    Web Application의 정적 데이터는 S3에 저장.  
    Route 53 에 등록된 자체 도메인 이름 사용중.  
    정적/동적 데이터의 성능 개선 및 대기 시간의 감소 솔루션은?
    - `S3 및 ALB 를 오리진으로 포함하는 Amazon CloudFront 배포`를 생성.
    - CloudFront 배포로 트래픽을 라우팅하도록 Route 53 을 구성
    - 정적 콘텐츠는 S3의 클라우드 프런트 엣지 위치와 ALB 뒤의 동적 콘텐츠 EC2에서 캐싱 가능