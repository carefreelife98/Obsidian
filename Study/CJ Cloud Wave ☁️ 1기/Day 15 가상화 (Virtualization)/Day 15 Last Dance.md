  

## 파일 공유 : FTP

FTP

- TCP
- Port
    - 21 (session)
    - 20 (data) - 서버의 active mode 에서 사용하는 data 전송 포트
- Mode
    - Active
        - Client(임의의 포트) — 세션연결시도 / 연결 → Server (21번 포트)
            - 클라이언트가 서버에게 통신을 위한 data 포트를 알려준다.
            - 서버가 클라이언트에게 서버의 20번 포트에서 클라이언트가 알려준 포트로 data를 전송.
        - 문제점
            - 서버 쪽 방화벽의 21번 포트는 항상 열려있음.
            - 하지만 클라이언트 쪽 포트는 초기에 임의의 포트에서 요청을 했으므로 클라이언트 방화벽 쪽에서 해당 포트가 열려 있지 않을 수도 있음.
    - Passive
        - Client(임의의 포트) — 세션연결시도 / 연결 → Server (21번 포트)
            
            - 클라이언트가 서버에게 통신을 위한 data 포트를 알려준다.
                - 포트 번호 지정 가능
                - 서버쪽 방화벽에서 지정된 포트번호에 대해서만 방화벽을 열어주면 된다.
            - 서버가 클라이언트에게 서버의 20번 포트에서 클라이언트가 알려준 포트로 data를 전송.
            
              
            
              
            

## DHCP (Dynamic Host Configuration Protocol)

- 클라이언트에게 IP주소, 서브넷마스크, 게이트웨이 주소, DNS 주소 등 IP관련 파라미터를 자동으로 할당하는 서비스
    - UDP 67(서버) , 68(클라이언트) 사용
    - 전송방식 : 브로드캐스트(Default), 유니캐스트(1대1 통신)
    - DHCP 메시지 (DORA) : ACK 전송까지 완료되어야 ~ 연결됨?
        1. DHCP DISCOVER : 클라이언트 → 서버
        2. DHCP OFFER : 서버 → 클라이언트
        3. DHCP REQUEST : 클라이언트 → 서버
        4. DHCP ACK : 서버 → 클라이언트
    - DHCP 서버로부터 IP 주소를 할당받지 못하면 169.254.x.x 주소가 할당됨.

  

## Router 0

- GigabitEthernet0/0 : 10.0.0.0/24 네트워크1
    - server1
        - DHCP
            - DISCOVER
            - OFFER
            - REQUEST
            - ACK
    - server2
    - DHCP 1 : 브로드캐스트
    - DHCP 2 : 브로드캐스트
        - DHCP 1, 2 중 살아있는 DHCP 서버 / 먼저 도착한 DBCP 서버에서 실행됨.
        - 10.0.0.0/24 네트워크 내의 서버에게만 브로드캐스팅 가능.
            - Cisco : helper -address 와 같은 추가적인 설정을 라우터에 해주면 다른 네트워크의 서버에게도 브로드캐스팅이 가능해짐.
- GigabitEthernet 0/1 10.0.1.0/24 네트워크2
    
    - server3
    - server4
    - DHCP 3
        - server 3, 4에게 브로드 캐스팅 가능.
        - server 1, 2에게 브로드 캐스팅 불가능.
            - 추가적인 설정을 해주면 (Cisco : help -address) 다른 네트워크의 서버에게도 브로드캐스팅 가능.
    
      
    

  

  

  

## (추가)

  

(추가)

AWS - VPN

- 한 쪽은 on premise 라 생각하고
    - VPC 간에도 VPN 연결이 가능하다.

  

계정 다시 만듦 → 키 페어 새로 생성?