  

Development : 기본 개발 테스팅 환경만 구축 (이중화 굳이 필요없다)

  

Staging : 실제 운영환경과 똑같이 생성

  

Production : 실제 운영 환경. STG와 다른 점은 리소스의 크기가 더욱 커진 것 뿐.

  

  

- Image 는 S3에. (Cloud Front)
- Dynamo DB → RDS 의 부담을 줄이기 위해 사용
- Elastic Cache → RDS 의 부담을 줄이기 위해 사용

  

  

질문

1. prometheous grafana / aws - prometheous grafana

  

1. SQS SNS / Event Bridge 차이점