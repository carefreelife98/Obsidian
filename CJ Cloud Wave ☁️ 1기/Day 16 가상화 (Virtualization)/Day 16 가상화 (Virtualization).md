  

1. Root로 로그인 → Hostname setting, password 변경
    
    - passwd → 1234
    
      
    
2. Hostname setting
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.05.33.png]]
    
    - vim /etc/hostname
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.05.12.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.06.30.png]]
        
          
        
3. Static IP setting
    
    - 213.0.113.11/24 로 변환
    
    1. 설정
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.08.11.png]]
        
    2. 네트워크 설정
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.09.01.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.10.24.png]]
        
          
        
4. Firewall Setting & SELinux Setting : 비활성화
    
    - Ubuntu와 달리 Redhat계열은 default로 방화벽이 활성화 되어 있다.
    - 접속이 차단될 수 있으므로 비활성화 해주자.
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.13.24.png]]
        
    
      
    
    - SELinux 비활성화
        
        - SELinux : Security
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.15.16.png]]
        
    
      
    
    - SELinux 를 사용하지 않겠다고 설정해주어야함
        - setenforce : 설정
        - getenforce : 확인절차
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.17.49.png]]
            
        - reboot하여 설정이 잘 적용되었는지 확인
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.19.50.png]]
            
              
            
    - hostname / ip setting 성공
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.21.31.png]]
        
    
      
    
    - google로 ping 동작 확인
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.22.36.png]]
        

- SELinux : Disabled 동작 확인
- Firewall : inactive 동작 확인
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.23.33.png]]
    
      
    

  

## MacOS 와 CentOS 와 통신 확인

  

  

## 가상머신 생성에 필요한 기본 패키지 설치

  

```
[root@admin ~]# cat /proc/cpuinfo | egrep "vmx|svm"[root@admin ~]# lscpu | grep VirtualizationVirtualization: VT-xVirtualization type: full[root@admin ~]# dnf -y install qemu-kvm libvirt virt-install \> virt-viewer libguestfs-tools[root@admin ~]# lsmod | grep kvm[root@admin ~]# systemctl enable --now libvirtd[root@admin ~]# systemctl status libvirtd
```

## ip 두개 생성 성공

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.13.57.png]]

- 192.168.64.6/24
- 192.168.64.7/24

  

## 네트워크 변경하기

1. **NIC 설정하기**
    
    - [root@admin ~]# nmcli connection add type bridge autoconnect yes con-name br0 ifname br0
    
      
    

  

  

  

- ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.50.44.png]]
    
      
    
      
    

  

  

## Docker

- hostname 변경
    
    ![[Images/Untitled.png|Untitled.png]]
    
    ![[Images/Untitled 1.png|Untitled 1.png]]
    

  

**기억해야할 용어**

- BUILD : 이미지 생성하기 (Download, Dockerfile)
- SHIP : 이미지 저장하기 (Docker Registry - public & private)
    - Docker Registry : 이미지가 저장된 곳
        - public : 누구나 접근가능
        - private : 인증 후 접근가능
- RUN : 이미지 실행 & Container 생성 (Application)

  

- Build / Ship : 주로 개발자가 진행
- Ship / Run : 주로 운영자가 진행

  

## Doker의 구조

![[Images/Untitled 2.png|Untitled 2.png]]

  

![[Images/Untitled 3.png|Untitled 3.png]]

- docker
    - 이미지 : 애플리케이션을 구동하기 위한 FILE.
    - build - 이미지 생성
    - pull - 이미지 다운로드
    - run - 이미지 실행

  

- docker 은 client와 server 사이를 REST API 를 사용하여 연결
- dockerd 라는 도커 데몬이 일을 함

  

![[Untitled 4.png]]

- 도커 명령어
    - 직관적인 명령어
    - 기억할만한 명령어
        - exec : 실행
        - run : 이미지를 이용하여 실행이 되는 컨테이너 생성
        - commit : container을 이용하여 이미지를 생성 (비선호 - 용량이 커짐)
            - container은 가능한 소형화 하여 여러 개의 컨테이너를 띄우는 것이 암묵적 룰 / 철학
            - 컨테이너가 소형화 될수록 성능이 증가
        - save / load : file 확장자는 무조건 .tar
        - create : stopped 상태의 컨테이너 생성(run과의 차이점)
        - export : 실행이 되고 있는 파일을 container로 변환
        - inspect : 상세정보 보기
    - pull 과 run의 연관성
        - container는 무조건 image가 있어야함.
            - 만약 run하려는데 image가 없으면 run 메소드에서 pull까지 한 후 run하게 됨.
            - 결국 pull 할 필요없이 매번 run만 하면 됨

  

## Docker 과 Podman

- Docker
    - Daemon 이 있음.
    - Daemon이 죽으면 해당 컨테이너도 중지됨.
- Podman
    - Daemon 이 없음
    - Daemon 과 상관없이 해당 운영체제/ 컨테이너 가 종료될 때까지 종료되지 않음.
- Docker의 상용화 (유료화)
    - 점점 podman을 사용하는 추세가 보임 (대표적인 예로 KT)

  

  

## Docker 설치 (Repository 생성)

1. [root@servera ~]# yum-config-manager --add-repo \
    
    > https://download.docker.com/linux/centos/docker-ce.repo
    

- CentOS Stream 8 은 Docker 설치를 기본적으로 지원하지 않으므로 그 레파지토리를 먼저 설치해야 한다.
- 저장소의 URL이 저장되어 있는 파일(=repository)을 만들어주어야 한다.

```
[root@servera ~]# yum install yum-utils –y[root@servera ~]# yum remove runc -y[root@servera ~]# yum-config-manager --add-repo \> https://download.docker.com/linux/centos/docker-ce.repo[root@servera ~]# yum install docker-ce -y[root@servera ~]# rpm -qa | grep dockerdocker-ce-23.0.1-1.el8.x86_64docker-buildx-plugin-0.10.2-1.el8.x86_64docker-scan-plugin-0.23.0-3.el8.x86_64docker-ce-cli-23.0.1-1.el8.x86_64docker-ce-rootless-extras-23.0.1-1.el8.x86_64docker-compose-plugin-2.16.0-1.el8.x86_64[root@servera ~]# systemctl enable –-now dockerCreated symlink from /etc/systemd/system/multi-user.target.wants/docker.service to/usr/lib/systemd/system/docker.service.[root@servera ~]# systemctl status docker>
```

  

  

  

![[Untitled 5.png]]

- Servera - Docker 설치 및 실행된 모습.

  

![[Untitled 6.png]]

- Serberb - Docker 설치 및 실행된 모습.

  

  

## Docker 실행 과정 예시 및 장점

  

- 문제점
    - Docker는 현재 상용화 (유료화) 중
    - IP별로 하루에 PULL 할 수 있는 이미지 개수가 제한됨.

  

- [root@servera ~]# docker pull mariadb
    
    ![[Untitled 7.png]]
    

  

- [root@servera ~]# docker images
    
    ![[Untitled 8.png]]
    

  

- 데이터베이스는 생성 시 환경 변수가 필요함.
    
    - [root@servera ~]# docker run --name mariadb-basic \
        
        > -e MYSQL_USER=user1 \  
        > -e MYSQL_PASSWORD=mypassword \  
        > -e MYSQL_DATABASE=product \  
        > -e MYSQL_ROOT_PASSWORD=r00tpassword \  
        > -d mariadb:latest
        
        - -d : 백그라운드에서 실행하라.
    - 위에서 지정해준 환경 변수는 컨테이너에 저장됨.
    
    ![[Untitled 9.png]]
    
      
    
- [root@servera ~]# docker ps
    
    ![[Untitled 10.png]]
    

  

- [root@servera ~]# docker exec -it mariadb-basic bash
    
    - mariadb-basic 을
    - -it : 터미널을 사용해서
    - bash :
    - exec : 실행하라.
    
    ![[Untitled 11.png]]
    

  

- root@9ec2cf06718b:/# mariadb -uroot –pr00tpassword
    
    ![[Untitled 12.png]]
    

  

- MariaDB [(none)]> show databases
    
    ![[Untitled 13.png]]
    

  

- exit / docker stop mariadb-basic / ps / ps -a (종료된 컨테이너까지 출력해줌)
    
    ![[Untitled 14.png]]
    

  

![[Untitled 15.png]]

- CentOS 8 → containers 100개
    
    커널 → containers 공유해서 사용 (개수 상관 없이)
    

  

  

## Container 개념 탄생 배경

- 기존 리눅스 환경
    - cgroup (Control Group) : 자원(cpu, ram, disk) 을 통제
    - chroot : 특정 디렉토리 제한 격리 환경
- 변경 사항 레이어 형태로 저장하는 파일 시스템 (Union File System)
- 간단하게 말하자면, 격리된 공간에서 프로세스가 동작하게 해주는 가상화 기술.

  

![[Untitled 16.png]]

- Hypervisor( +openstack) : VSWare
    - 결과물이 Virtual Machine이다.
    - 속도가 느리고 무겁다.
    - 용량이 크다.
    - INTEGRATION이 어렵다
    - 상대적으로 보안성이 좋다.
- Container Runtime : Docker
    - 결과물이 Container이다.
    - 속도가 빠르다.
    - 용량이 작다.
    - Container를 사용함으로서 INTEGRATION이 쉽다.
    - 상대적으로 보안성이 좋지 않다.

  

## IaC(Infrastructure as Code)

- Ansible
- Terraform

  

## Podman (무료) VS Docker (유료)

1. Layer (Code)
    1. docker으로 생성 가능.
2. Image
    1. docker으로 생성 가능.
3. Container
    1. docker으로 생성 가능.
4. Pod (group of containers - 컨테이너 그룹)
    
    1. docker으로 생성 불가.
    2. podman은 pod까지 생성 가능.
    
    - Pod 관리 프로그램
        - Kubernetes (opensource - 무료)
            - podman 을 이용하여 많이 사용.
        - Openshift (Redhat 상용 - 유료)
            - podman 을 이용하여 많이 사용.

  

# Kubernetes vs Openshift

- 가장 큰 차이점
    - GUI (Web console)
        - kubernetes : 저수준
            - Linux 명령어 필수
        - OpenShift : 고수준
            - Linux 몰라도 GUI 통해 사용 가능
            - 취업하게 되면 OpenShift 강의 꼭 들어봐라? → expensive

  

  

## Docker Container

![[Untitled 17.png]]

- **컨테이너 개념에서 매우 중요한 것**
    - **여러가지 container의 lmage들은 전부 read - only 속성.**
    - **하지만 container들은 writable**
    - **어떻게?**
        - **image에 writable이 가능한 layer를 추가한 것이 곧 컨테이너인 것!!!**

  

- **Base Image 는 OS(debian은 ubuntu와 비슷)**

  

- linux
    - bootfs : kernel을 가진다.
    - rootfs : 명령어를 가진다.

  

## Container 생성

![[Untitled 18.png]]

- Docker Client가 $docker container run … 하게 되면
    - Docker server의 Docker Daemon이 요청을 받게 된다
    - Docker Daemon :
        - ~/docker.service; (=dockerd)
            
            ![[Untitled 19.png]]
            

  

컨테이너 생성 및 시작 - create 사용

![[Untitled 20.png]]

![[Untitled 21.png]]

![[Untitled 22.png]]

  

  

**컨테이너 직접 실행 - run 사용  
[root@servera ~]# docker container run --name nginx -d -p 8180:80 nginx**

![[Untitled 23.png]]

1. -p 8180:80
    - Port Forwarding
        
        `_Port Forwarding : Host의 8180 포트와 Container 의 80 포트를 연결._`
        
2. -d : Background에서 돌아가도록 함. 해당 터미널은 실행 되지 않음?

  

![[Untitled 24.png]]

  

## 컨테이너 실행, 중지 및 재실행

![[Untitled 25.png]]

  

![[Untitled 26.png]]

  

## 컨테이너 일시 중지

![[Untitled 27.png]]

  

  

## 컨테이너 접속 및 그 내부 정보 변경하기

![[Untitled 28.png]]

- **/bin/bash 명령어는 어디에서 실행되는 것인가? (container / host)**
    - **container 에서 실행되는 명령어.**

  

- 0.0.0.0:
    - 서버a 내의 모든 사용자가 접속 가능.
- 8180 (host) ——> 80 (container)
    - host 포트는 65535 개의 범위중에 단 하나만 사용이 가능하다.
- IP가 모두 접속 가능한 IP(0.0.0.0)이므로 local / 해당 네트워크에 속한 사용자의 브라우저로 직접 해당 URL에 들어가 내부 정보 변경 확인을 할 수 있다.
    
    ![[Untitled 29.png]]
    

  

![[Untitled 30.png]]

- cp
    - host 상에 있는 경로를 container으로 복사
    - container의 경로를 host로 복사
        - 위와 같이 직접 컨테이너 내부에 들어가지 않고 내부 정보 변경을 할 수도 있다**.**

  

**컨테이너 생성 , 컨테이너 내부 정보 변경, 컨테이너 내부 정보 확인의 세 단계가 모두 일치해야 한다.**

  

  

**컨테이너 프로세스 및 포트 확인하기(top, port)**

![[Untitled 31.png]]

  

**컨테이너 이름 변경하기(rename)**

![[Untitled 32.png]]

  

**컨테이너 변경 정보 확인(diff)**

![[Untitled 33.png]]

- A : 추가된 파일
- B : 삭제된 파일
- C : 수정된 파일

  

  

**컨테이너 삭제하기(rm, prune)**

![[Untitled 34.png]]

  

  

**전부 삭제 → 가능하나 사용하지 말 것.**

- **prune**
- docker rm -f $(docker ps -aq)

  

  

  

## 질문

- docker image파일 내에 OS기능이 들어가 있으므로
    
    Base OS가 linux이든, window이든 상관 없이 작동 된다.
    

  

  

  

- Docker / RedHat registry : public registry
- Private registry