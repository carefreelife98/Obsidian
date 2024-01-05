  

NAT - 사설 네트워크의 장치가 외부의 공중망과 통신하기 위해서 사용(출발지나 목적지를 바꿀 수 있음) 어제 한건 출발지의 사설 아이피를 공인 아이피로 바꿔준거

  

ACL(access control list) : 보안그룹, 통신이 되는 상태에서 트래픽을 제어(차단하거나 허용하거나)

트래픽 식별, 필터링을 주로 사용

  

QoS(quality of service)를 위해선 acl을 사용해야함, 정상적인 트래픽만 허용

  

  

- **ACL(Access Control List:접근제어목록)**

네트워크 상에서 ACL은 크게 두가지 목적으로 사용됨.  
첫번째 :

트래픽을 식별하기 위한 목적으로 사용됨. 식별된 트래픽을  
QoS, NAT, VPN 등과 같은 다른 기능에 사용함.

두번째 :

트래픽을 필터링하기 위한 목적으로 사용됨.  
트래픽을 식별하여 허용 또는 차단을 해서 네트워크를 보호하기 위해 사용함.

# R1 에 ACL 설정 및 적용

  

192.168.200.0/24 네트워크의 호스트는 외부에 있는 웹서버에만 접근 가능하고, 나머지 트래픽은 모두 차단.

  

## Step 1.

enable conf t

access-list 100 permit udp 192.168.200.0 0.0.0.255 host 100.0.0.100 eq 53

access-list 100 permit tcp 192.168.200.0 0.0.0.255 host 100.0.0.200 eq 21

access-list 100 permit tcp 192.168.200.0 0.0.0.255 any eq 80

access-list 100 deny ip any any

  

int g0/1

ip access-group 100 in

  

  

## Step 2.

no access-list 100

access-list 100 permit udp 192.168.200.0 0.0.0.255 host 100.0.0.100 eq 53

access-list 100 permit tcp 192.168.200.0 0.0.0.255 host 100.0.0.200 eq 21

access-list 100 permit tcp 192.168.200.0 0.0.0.255 any eq 80

access-list 100 deny ip any any

  

int g0/1

ip access-group 100 in

  

  

# R3에 SSH 서비스 설정

  

## Step 1.

우선 이전에 ACL 적용한 것 삭제

  

## Step 2.

R3 라우터에서 아래와 같이 실행

en

conf t

ip domain-name cloudwave.com

username admin password cisco

  

crypto key generate rsa general-keys modulus 1024

  

line vty 0 4

login local

  

- pc0 에서 ssh 접속(command prompt)
- ssh -1 admin 30.0.0.3

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.37.12.png]]

pc0 에서 정상적으로 작동이 되는 모습.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.39.14.png]]

pc1 에서도 정상적으로 접속이 된다.

  

## Step 3.

R3에 ssh 접속을 pc0로 제한하고 접속 IP 주소도 30.0.0.3 으로 제한하는 ACL 작성 및 설정

  

R3)

en

conf t

no access-list 103 (선택 - 삭제 시)

access-list 103 permit tcp host 20.0.0.1 any eq 22

→ 192.168.200.10 에서 30.0.0.3 으로 가는 트래픽 중에~ ssh 는 22

access-list 103 deny ip any any

→ 암묵적으로 모든 트래픽을 차단한다는 규칙이 들어감. 접근이 차단된 트래픽의 개수를 Count 해주는 기능.

line vty 0 4

→ line vty 0 4 == 0 ~ 4의 총 5가지 회선만 허용 하겠다.

access-class 103 in

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.48.35.png]]

위와 같이 적용이 완료 되면 PC0에서만 접속이 가능해진다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-28_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.26.03.png]]

타 IP에서는 접근이 차단 되는 모습.

  

- 참고)
- ACL 번호 :
- 1 ~ 99 == 표준 ACL(출발지 IP 주소만 조건으로 사용함)
- 100 ~ 199 == 확장 ACL

  

  

# Load Balancing (부하 분산)

- 서버에 들어오는 트래픽을 분산 시키는 것.

  

- 로드 밸런서 (L4, L4 Switch, 네트워크 로드 밸런서 → TCP / UDP / port number) : 트래픽의 서비스 종류(L4 → TCP / UDP / port number)까지만 식별하고, 분산처리를 하는 장비.
    
    → 분산 시 Scheduling은 RR(Round Robin) 으로 진행.
    
- Application Switch (L7) : 로드 밸런서와 같은 기능을 하나, L7까지의 정보(데이터)를 볼 수 있다.
    
    → 보안 장비, 애플리케이션 로드 밸런서로 사용됨. (모든 정보를 볼 수 있으므로)
    
    → 트래픽의 모든 정보를 분산화 하므로 부하가 더 적어지게 한다.(img, text, script 까지도 분산 시키기 때문에)
    

  

  

sudo yum install httpd  
sudo systemctl start httpd  
sudo su -  
cd /var/www/html  
echo "WebSVR1" > index.html

보안그룹 인바운드 규칙에 TCP 80 추가

로드밸런서 DNS 주소로 접속.