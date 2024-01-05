  

  

vnc란?

  

## Podman Pod 사용하기

- 포드는 동일한 네트워크, pid 및 ipc 네임스페이스를 공유하는 하나 이상의 컨테이너 그룹이다. 아래 예제는  
    Podman 에서 Pod 를 사용하는 방법에 대한 설명이다.
    
    ![[Untitled 47.png|Untitled 47.png]]
    
      
    
- Container 생성하면 해당 컨테이너의 포트와 같은 포트를 사용하는 infra container도 같이 생성된다.
    
    - **Infra container** : ipc, net, pid, namespaces, cgroups
    
      
    

1. pod 생성하기
    
    ![[Untitled 1 4.png|Untitled 1 4.png]]
    
    ![[Untitled 2 4.png|Untitled 2 4.png]]
    
      
    
2. Pod, Container 동시 생성하기 (new: ~ 옵션)
    
    ```
    [root@servera ~]# podman run -dt --pod new:web-pod -p 80:80 -p 3306:3306 \> --name nginx docker.io/nginx[root@servera ~]# podman pod ls[root@servera ~]# podman ps -a[root@servera ~]# curl http://localhost:80
    ```
    
    ![[Untitled 3 4.png|Untitled 3 4.png]]
    
      
    
3. 기본 Pod 에 Container 추가하기
    
    ![[Untitled 4 4.png|Untitled 4 4.png]]
    
      
    

- 포트번호 지정 로직
    - 현재 pod 에는 두개의 포트가 지정되어있다. (:80, :3306)
    - nginx 는 기본 지정 포트가 :80 이다.
    - nginx 가 이미 :80을 사용중이기 때문에 mariadb는 :3306 포트를 자동으로 사용하게 된다.

  

- Podman / Docker → Container 사용이 중점 (기본단위 : Container)
- Kubernetes → Pod 사용이 중점 (기본단위 : Pod)

  

  

1. Kubernetes Yaml generate (Podman 이 제공하는 서비스)
    
    - podman 에서 작성한 yml 파일을 Kubernetes 에서 Pod 생성에 사용할 수 있다
    
      
    
      
    
2. podman-compose 설치하기
    
    - docker-compose 와는 다르게 podman-compose 는 패키지 설치가 바로 가능하다. 사용법은 docker-compose 와 동일하므로 그 설명을 생략한다
    
    ```
    dnf install epel-release -ydnf search compose
    ```
    
    - epel-release 패키지를 설치해주면 podman-compose가 설치된다.
        
        ![[Untitled 5 4.png|Untitled 5 4.png]]
        
          
        
          
        
          
        

## KVM

1. 기본 조건
    
    - 실습을 위해 설치하는 호스트에 대한 기본 사양은 다음과 같다. 실습 중 더 필요한 사양이 있는 경우 중간에 추가해도 된다.
        
          
        
    
    ![[Untitled 6 4.png|Untitled 6 4.png]]
    
    - 위 가상머신 설치시에 한가지 주의점은 VM 생성을 위해 Processors 메뉴에서 Virtualize Intel  
        VT or AMD 옵션이 반드시 활성화되어야 한다.

  

1.1 패키지 설치하기

- 가상화를 위한 조건 충족 단계
    - BIOS 조건 충족
    - HostOS 조건 충족
    - GuestOS 조건 충족 (KVM)

  

- 아래와 같이 설정 확인 시 같은 모습이 보여야 한다.

```
[root@admin ~]# cat /proc/cpuinfo | egrep "vmx|svm"
```

![[Untitled 7 4.png|Untitled 7 4.png]]

  

```
[root@admin ~]# lscpu | grep Virtualization
```

![[Untitled 8 4.png|Untitled 8 4.png]]