  

## 파일공유 서비스는 각 설치 버전에 따라 실행 방법에 있어 약간의 차이가 있을 수 있다.

  

- 445번 포트란?
    - Server Message Block(=Common Internet File System)은 다른 시스템 간의 파일 및 프린터 공유에 사용하는 프로토콜이며 445 포트를 사용한다.
    - TCP/IP 기반의 NetBIOS 프로토콜을 사용한다.
    - SMB에는 SMB1, SMB2.0, SMB2.1, SMB3.0, SMB3.0.2, SMB3.1.1 버전이 있다.
        - 위와 같은 SMB 네트워킹 프로토콜을 다시 구현한 자유 소프트웨어를 Samba 라고 한다.

  

- 사용자를 강제하는 방법
    
    sudo mount -t cifs //10.10.10.20/ubuntu /home/ec2-user/smb_dir -o username=ubuntu,password=1234,uid=1000,gid=1000,forceuid,forcegid  
      
    공유하는 공간을 mount 하여 사용.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.22.17.png]]
    
    - mount란?
        
        우리가 많이 사용하는 Windows에서는 물리적 장치(USB, 외장하드디스크 등등 ) 컴퓨터와 연결하면 자동으로 연결이 됩니다
        
    - 하지만 리눅스에서는 직접 연결한 물리적 장치와 리눅스에 특정 디렉토리를 연결하여 사용해야 되는데 이때 외부 장치와 리눅스에 특정 디렉토리를 연결하는 것을 mount(마운트) 라고 합니다.
        
        [![](https://blog.kakaocdn.net/dn/MD0W7/btrq8IlbIH9/VBKjJXvk8ptskpnfALdmG1/img.png)](https://blog.kakaocdn.net/dn/MD0W7/btrq8IlbIH9/VBKjJXvk8ptskpnfALdmG1/img.png)
        

  

**Window O/S (= AWS → FSx service)**

- 방화벽 → :445 차단, :22가능(예)
    
    - AWS
        - VPC : sg(보안그룹) → :22 open
            - EC2
                - samba
    
    → 위 모습에서 :445 포트는 방화벽 차원에서 차단되어 있지만, :22포트를 열어 외부에서 22포트를 터널로 사용, ec2내의 samba에 접근할 수 있도록 해준다. 마치 VPN 처럼.  
      
    위 방식을 적용하기 위하여 포트번호를 바꿔주어야 한다.
    
      
    

(window 명령어)

netsh interface portproxy add v4tov4 listenaddress=10.0.0.3 listenport=445 connectaddress=10.0.0.3 connectport=5445

- 10.0.0.3:445 로 접속을 시도하면, 10.0.0.3:5445 로 변경해주는 설정
- 윈도우가 445포트를 시스템에서 사용중이기 때문에 445번을 사용하는 Samba 서비스를 사용할 수가 없기 때문.
    - 네트워크 연결 보기 → ncpa.cpl

  

**파일 서버란?**

- 파일 서버는 OS따라서 Windows File Server, Unix File Server, Linux File Server가 있다.
- 윈도우 파일서버 경우는 CIFS(Common Internet File System)을 사용하여 클라이언트에 스토리지를 공유하며
- Unix, Linux는 NFS(Network File System)을 사용함
- 파일 서버 구축은 OS 중요도가 가장 높음
    - 윈도우 ⇨ 윈도우 파일서버 구축: CIFS 사용
        - **CIFS 란?**
            
            CIFS(Common Internet File System)은 네트워크를 위한 SMB파일 공유 프로토콜의 확장된 버전. 윈도우와 유닉스 환경을 동시에 지원하는 인터넷의 표준 파일 프로토콜 이다.
            
    - 리눅스 ⇨ 리눅스 파일서버 구축: 리눅스 NFS-Untils 이용하여 NFS 사용
    - 윈도우 ⇨ 리눅스 파일서버 구축: Samba를 이용하여 SMB/CIFS 사용
- SMB(Server Message Block)는 윈도우 시스템이 다른 시스템의 디스크나 프린터와 같은 자원을 공유할 수 있도록 개발된 프로토콜
- **Samba 란?**
    
    Samba는 SMB/CIFS 네트워킹 프로토콜을 다시 구현한 소프트웨어.
    
    Windows 운영체제를 사용하는 PC에서 Linux 또는 UNIX 서버에 접속하여 파일이나 프린터를 공유하여 사용할 수 있도록 해준다.
    
      
    

## NFS (Network File System) → AWS(EFS service)  
= 서버의 디렉토리를 클라이언트가 공유할 수 있도록 해주는 기능

  

NFS란 네트워크를 통해 다른 호스트에 있는 파일시스템의 일부를 자신의 디렉토리처럼 사용할 수 있도록 해 주는 것.

하나의 서버에 디스크를 집중관리하고 그것을 공유하여 나머지 시스템들이 사용할 수 있게 해준다.

- sun microsystem 사에서 개발  
    - 분산처리환경의 RPC(Remote Procedure Call)를 사용하는 파일 및 자원 공유 시스템  
    - TCP/IP 네트워크를 통한 서버/ 클라이언트 구조  
    - UNIX 운영체제간의 파일 공유  
    - 보안에 취약하므로 주의해서 사용

  

- 아마존 linux에 nfs-dir 생성 및 nano 설정
    - no_root_squash : root로 취급 하겠다.
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.51.17.png]]
        
- ubuntu 20.04 nfs client 설치
    - sudo apt install nfs-common
- ubuntu 20.04 nfs-server 설치
    - sudo apt update
    - sudo apt install nfs-kernel-server
    - sudo nano /etc/exports → 공유할 디렉토리 호스트(rw,no_root_squash) 후 저장.
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.39.25.png]]
        
        - no_root_squash 옵션 → root로 mount 시 root권한 부여.
    - sudo exportfs -ar : 공유 시작
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.38.31.png]]
        
    - client 에서 mount.
        - mount -t nfs 10.10.10.x:/root/nfs_dir nfs_dir
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.37.06.png]]
            
    - 보안그룹에 허용할 포트 → 방화벽 개방 : tcp 2049, 111, 20048
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.53.23.png]]
        
        - AWS에서 EFS 서비스 사용시에는 2049, 111 포트만 열어주면 된다.
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.33.01.png]]
            

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.31.43.png]]

- NFS 를 사용해서 파일 공유가 된 모습.

  

Ubuntu 20.04 서버시간 동기화

1. apt install chrony
2. nano /etc/chrony/chrony.conf
    - 기존 pool 정보 주석처리
    - server [time.bora.net](http://time.bora.net) iburst
3. timedatectl 명령으로 확인시 timezone 이 Asia/Seoul 이 아닌 경우
    - timedatectl set-timezone Asia/Seoul
4. systemctl restart chrony
5. timedatectl 로 서버시간 동기화가 되었는지 재확인

  

  

  

  

## DNS (Domain Name System)

: 이름주소 ↔ IP 주소

- 초창기 : /etc/hosts
    - /etc/ 에 IP주소 등록,
    - /hosts 에 이름 주소를 등록하여 직접 도메인 서버 운용이 가능. (현재도)
- 현재 : DNS 서비스를 사용

  

- FQDN (Fully Qualified Domain Name)
    - URL
        - http:// www. → 호스트명
        - [naver.com](http://naver.com)( . 생략) → 도메인 네임
        - /index.php
- DNS 서버 : Caching Name Server (캐시를 사용하여 빠르게 정보 교환)

  

Domain Name

- Internet 상에서 Host 구별 방법
- 기억하기 까다로운 IP Address의 대체 수단
    
    → 숫자로 이루어진 IP Address를 수 많은 Host 마다 기억하기는 어려움
    
    → IP Address를 사람이 기억하기 쉬운 문자와 숫자의 조합인 Domain Name과 매핑
    
- 현재 Internet Service의 기본 구성 요소
- Domain Name 등록 원칙
    
    영문자(a-z) 26개, 숫자(0-9) 10개, 특수 기호(-) 1개를 합쳐 총 37개 글자 조합  
    영문자의 대·소문자 구분은 없으며, 특수 기호(-)는 처음과 끝에는 올 수 없음  
    분산 관리 원칙에 따라 각 단계별로 독자적인 관리를 하며 각 단계별 구분은 마침표(.) 사용  
    전 세계적으로 중복되지 않아야 하며, 선착순 원칙(First Come, First Served)에 따라 부여
    
    - 최근에는 한글 도메인도 사용 가능해짐.
    
      
    
    1. DNS 구성 요소  
    - Domain Name Space  
    인터넷에서 사용되고 있는 도메인 네임의 계층적 구조 공간을 의미  
    Root Domain(.) 이하로 Tree(트리) 형태의 계층적인 구조로 구성  
      
    “. → kr. → co.kr. → google.co.kr.”  
    Root Domain은 최상위 도메인 정보를 관리하고 최상위 도메인은 그 하위 도메인에 대한 정보를 관리  
    → 이러한 계층 구조로 인하여 정보는 각 도메인의 네임 서버로 분산, 관리  
      
    - Resource Record → 정보의 종류  
    Domain Name Space에서 지정된 Domain Name에 대해 필요한 인터넷 자원 정보를 매핑하는 수단을 제공  
    → Domain Name과 IP Address를 연결하여 DNS Database 구성  
    하나의 도메인 네임이 갖는 속성 정보(예: www, ftp, mail 등 == 호스트 네임)를 지정하는 수단  
    리소스 레코드는 확장이 가능
    
    → 예) IPv4: A type의 리소스 레코드 사용 (A name), IPv6: AAAA Type의 레코드 (AAAA name) 추가 정의
    
    C name: AWS 등에서 주로 사용
    
      
    - Name Server
    
    Domain Zone(예: netcollege.co.kr)의 정보를 소유하고 이에 대한 질의에 대해 응답하는 역할을 수행
    
    보통 DNS Server라 칭하며 특정 질의에 대해서 자신이 소유한 Domain Zone의 정보만 응답
    
    - Resolver  
        Name Server에 의해 구성된 도메인 데이터 베이스를 검색하는 역할  
        요청된 리소스 레코드가 존재하는 위치를 DNS에서 찾고, 찾은 정보를 최종 응답으로 되돌려 주는 기능을 담당  
        전체 도메인 데이터 베이스를 검색할 수 있도록 DNS 검색의 시작점이 되는 Root Name Server의 IP Address를 가짐.  
        짧은 시간동안 동일한 Domain Name에 대한 질의 반복을 방지하기 위해 Cache 사용
    
    [그림 15.1] DNS 체계
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.20.05.png]]
    
      
    
    DNS 동작 과정
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.35.00.png]]
    
    - 매번 위와 같은 과정을 거치게 되면 많은 시간을 소요하기 때문에 Cache를 사용한다.

## bind9 프로그램 → 도메인 등록/사용

- **BIND9**은 **DNS 네임 서버를 구축하고 레코드를 관리할 수 있도록 도와주는 패키지**
- 도메인의 정보를 가지고 있는 zone 파일을 생성
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.58.47.png]]
    
    - _TTL_: Time to live. 캐시에 zone 정보가 남아있는 데이터 유효 기간
    - _@_: root 도메인
    - _SOA Record_: zone에 대한 필수적인 정보를 가지고 있는 레코드
    - _NS Record_: 해당 도메인의 IP 주소를 찾기 위해 가야 할 네임 서버 정보를 담고 있는 레코드
    - _A Record_: 호스트 네임에 주어진 도메인에 매핑되는 IPv4 형식의 IP 주소를 저장하는 레코드
    - _AAAA Record_: 호스트 네임에 주어진 도메인에 매핑되는 IPv6 형식의 IP 주소를 저장하는 레코드

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.51.41.png]]

- root@ip-10-10-10-20:/etc/bind 위치에서
    - vi naver.zone을 통해 도메인 정보 설정을 해준다.
        - zone : 도메인의 정보

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.32.54.png]]

- Amazon Linux 에서 DNS 사용 모습

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.49.01.png]]

- Ubuntu 에서 DNS 사용 모습(좌)
- Amazon Linux에서 Ubuntu의 IP를 통한 DNS 사용 모습 (우)

  

## Ubuntu server : Master

## Amazon : slave

## 관계로 설정해보기

  

- Master server에 먼저 접근.
    - 만약 Master server가 작동하지 않으면 slaves 폴더에 있는 slave server가 동작하게 됨.
    - master-server 관계는 그것이 전부.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.04.55.png]]

- slave server인 Amazon Linux에 Target 도메인 [www.naver.com](http://www.naver.com) 의 zone파일을 생성
    - 해당 zone파일은 slave의 속성을 가짐
    - master server의 ip 저장
    - slave server의 slaves 폴더에 [naver.zone](http://naver.zone) 파일을 master server 으로부터 가져올 것.

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.08.55.png]]

- Master server인 Ubuntu server의 zone.conf 에는 자신이 Target 도메인인 [www.naver.com](http://www.naver.com) 에 대한 상태 정보를 생성
    - 해당 zone파일은 Master의 속성을 가짐.
    - allow-transfer 에 설정된 IP에만 file에 설정된 파일을 제공해주겠다 설정.
- 위 Master-Slave zone 파일 설정을 통해 양 DNS서버(Master, Slave) 에 같은 IP를 가리키는 zone파일이 생성된다.
- 만약 Master server에 요청을 했으나 UDP 프로토콜의 특성상 약 2초간 응답이 없으면 Slave server의 zone 파일이 가리키는 IP를 응답하게 된다.
- 이전 과정에서 Master server의 zone 파일이 Slave server에게 복제 되었고, 결과적으로 같은 IP를 가리키게 되며 두 DNS server의 Master-Slave 관계가 성립하게 된다.

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.11.12.png]]

- named-checkconf named.conf
    - 위 명령어를 통해 bind된 zone 파일이 같은지 검사해 볼 수 있다(?)

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.13.01.png]]

- slave server의 slaves 폴더에 master에게서 받아온 zone 파일이 저장되어 있는 것을 볼 수 있다.

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-03_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.14.04.png]]

- slave server (10.10.10.180)에서 target 도메인(www.naver.com) 에 요청을 해도 master와 같은 IP인 10.10.10.20 을 응답해주는 것을 볼 수 있다.