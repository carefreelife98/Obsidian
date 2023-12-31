  

  

## 상태 검사

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.41.09.png]]

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.42.09.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.42.19.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.42.32.png]]

  

  

## Geolocation

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.18.16.png]]

- nameserver 의 IP를 미국에 존재하는 IP로 변환.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.17.57.png]]

- dig 명령어 사용 시 ANSWER SECTION 에서 미국의 IP인 54.162.7.72로 응답이 오는 것을 볼 수 있다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.19.54.png]]

- 해당 IP 접속시 미국의 IP인 것을 볼 수 있다.

  

## Geolocation 주의 사항

- 디폴트 값은 무조건 지정.
    
    - 사용자가 지정한 위치가 아닌 다른 곳에서 트래픽 보낼 시 디폴트로 보내주기 위함
    - 예) 미국 / 서울 설정 했으나 베트남에서 트래픽이 왔을 때 Default로 보낼 곳이 없으면 안된다.
    
      
    
      
    
      
    
      
    

# VPC Peering

- 서로 격리 되어 있는 2개 이상의 VPC의 네트워크를 연결하는 서비스
    - 동일계정에 구성된 서로 다른 VPC 연결 가능.
    - 다른 계정에 구성된 서로다른 VPC 연결 가능.
    - 서로 다른 Region에 구성된 VPC 연결 가능.

  

- VPC Peering 연결 리소스 비용은 무료.
    - 동일 Region, Available Zone 에서 발생시키는 데이터 트래픽은 무료.
    - Region, Available Zone 을 교차하는 데이트 트래픽은 과금.

  

- VPC Peering 제약사항
    - 전이적 피어링 구성 불가능.
    - CIDR Block이 겹치는 경우 VPC Peering 구성 불가능.

  

  

  

  

  

## Transit Gateway

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.40.47.png]]

- 하나의 라우터라고 생각하면 된다.
    - IP에 따라 연결 대상을 바꾸어 줄 수 있다.
        
        - 10.0.0.0/16 → VPC-A 로 가라.
        - 10.10.0.0/16 → VPC-B 로 가라.
        - 10.20.0.0/16 → VPC-C 로 가라.
        - Direct Connect(전용선) 도 연결 가능.
        
          
        
- VPC 간에 전이적 피어링이 불가능한 것을 가능케 해준다.
- Transit Gateway = 클라우드의 꽃

  

- Transit Gateway가 생성된 region이 다르면 각 Region 마다 새로 생성해주어야 하며, Transit Gateway Peering을 통해 연결 할 수 있다.

  

  

  

VPC 에 생성된 인스턴스에 접속(ec2 pem key)

  

해당 인스턴스의

  

  

  

  

virginia 18.212.31.28

  

## 고객 게이트 웨이

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.28.56.png]]

- 온 프레미스에 존재하는 VPN 장비의 정보를 담음

  

## Site-to-Site VPN 연결

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.30.43.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.31.37.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.32.47.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.32.56.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.34.27.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.35.42.png]]