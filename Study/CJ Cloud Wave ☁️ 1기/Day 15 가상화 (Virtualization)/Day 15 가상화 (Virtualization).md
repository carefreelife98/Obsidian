  

정철 (tland12@naver.com)

  

1. **KVM 가상화 - 8시간**
2. **Container 가상화 - Docker - 32시간 (Podman)**
3. **VMWare 가상화 - 4시간**

  

## Linux

- Suse
- Ubuntu
- Redhat
- RHEL (AS 가능)
    - CentOS (AS 불가)
    - Fedora
    - Oracle
    - AMI

  

공통점

- 명령어 거의 비슷

  

차이점

- Ubuntu
    - 패키지 다운로드 : apt -get
- CentOS
    
    - 패키지 다운로드 : yum / dnf
    - 패키지 파일명 : .rpm 으로 끝남.
    
      
    

# 가상화

- 한 대의 시스템 하드웨어를 **논리적으로 분할**하여 가상의 시스템을 생성 및 활용하는 개념이다.
    - **논리적 : 얼마든지 생성 및 삭제가 가능하다.**
- 가상 시스템들은 **서로 독립적인 하나의 시스템으로 인지**되기 때문에 주어진 하드웨어 리소스를 효율적으로 사용할 수 있다
    - **가상화된 시스템 상호간에 터치를 하지 않는다**

  

mac OS(213.0.113.1) → VMWare(213.0.113.2) → CentOS Stream 8(213.0.113.128) → KVM(시뮬레이터) → CentOS 8

  

  

  

- HostOS → Hypervisor(HostOS 와 GuestOS를 이어주는 기능 : 추상화) → GuestOS (가상OS)
- 가상화의 개념에서 **하이퍼바이저(Hypervisor)**의 개념이 매우 중요.
    
    - 이는 가상 OS와 실제 하드웨어 자원 사이에 위치하여 둘 사이의 괴리를 조정해주는 역할을 하는 것으로 볼 수 있다.
    - 이 과정을 **'추상화'**라고 하며, 물리적인 하드웨어 자원을 소프트웨어적으로 (가상으로)나누고, 나누어진 가상의 하드웨어를 진짜 하드웨어처럼 인지시키는 과정을 의미한다.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.17.19.png]]
    
- GuestOS는 자신이 Hypervisor 위에서 동작한다는 것을 모른다. → 가상화의 독립성(Isolation : 고립, 격리)

  

## Protection Ring

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.19.30.png]]

- Kernel 의 가장 중요한 역할 ?
    - Hardware Control (CPU , RAM, HardDisk, Monitor, Mouse…)
        - Hardware Control을 위해 Driver가 있어야 함.
    - Kernel 이 모든 하드웨어들을 인식해야한다.
    - Kernel에 근접한 사용자일수록 가장 많은 권한을 가져야 함. (Kernel은 최고 권한)
    - 하지만 Kernel에 근접하지 않더라도 Application은 사용할 수 있어야 함.
        - 따라서 Ring을 나누어 권한을 설정 해주는것.
        - LINUX 에서 루트 사용자와 일반 사용자를 나누는 것과 같음
- **Ring : 접근 경로**

  

## (참고)Full Virtualization

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.33.43.png]]

- HostOS 가 존재하지 않는다.
- Hypervisor 만을 사용해서 가상화를 구현
    - 당연히
        - Hypervisor에는 Kernel이 존재하지 않음.
        - Guest OS 에는 Kernel이 존재
            - 이것이 **GuestOS가 Ring0 인 이유이다.**
                
                ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.36.19.png]]
                

  

  

## (참고) 에뮬레이션 VS 시뮬레이션

  

- 에뮬레이션
    - 에뮬레이션은 호스트 머신에 존재하지 않는 하드웨어 및 아키텍처를 가상머신에게 제공
    - QEMU는 대표적인 에뮬레이터 중 하나로써 매우 다양한 종류의 하드웨어를 소프트웨어로 구현해둔 Hypervisor이다.
        - 에뮬레이터 : 호스트에 없는 자원을 ~..

  

- Simulation

  

  

**KVM 은 기본적으로 Qemu와 같이 사용하며 Qemu 는 에뮬레이터 이다.**

**KVM이 CPU와 RAM의 가상화를 담당한다면 QEMU는 각종 디바이스의 가상화를 담당한다. 즉, QEMU는 '에뮬레이터' 이며, ‘에뮬레이트 한다’라는 표현은 ‘소프트웨어적으로 동작시킨다’라는 의미로 받아들일 수 있다.**

  

## 그래서?

- GuestOS에

  

  

  

  

- 미국의 NASA는 IP 주소의 클래스 중 E class를 사용. (인공위성에도 IP가 존재)
    - 인공위성은 LINUX(UNIX) 를 사용.

  

- NSA
    - SELinux 사용.
    - SELinux는 루트 사용자의 권한도 조정할 수 있다.
    - Redhat 계열 리눅스는 SELinux 도 사용한다.

  

  

## 해야할 것

1. Hostname setting
2. Static Ip Setting
3. Firewall Setting & SELinux Setting

  

ping -c 3 8.8.8.8

  

route -n

- gateway checking

  

cat /etc/resolv.conf

- DNS Checking

  

  

  

  

  

  

  

  

## Cloud OS - OpenStack

AWS : 백그라운드에서 cloud OS 가 동작함으로서 서비스 됨.

  

**OpenStack**

- Privat Cloud Service를 구축하기 위한 Cloud OS의 이름.
- Open Source 이므로 비용이 무료이다.

  

**OpenShift (상용)** - Redhat

- Kubernetes 의 상위버전.

**Kubernetes** (Open Source)

- 무료

  

신한은행

- Amazon AWS 에 클라우드 서비스를 사용, 데이터 센터를 가지고 있지 않아도 데이터 센터가 있는 효과를 냄.
- 미국의 고객들은 한국의 데이터 센터를 거치지 않고 미국의 Amazon 에 직접 접근함으로서 시간과 비용을 줄일 수 있다.