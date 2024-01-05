  

  

Container Quota 사용하기  
컨테이너에게 CPU, MEMORY 할당하는 방법을 설명한다.

  

1. 일반적인 메모리 할당법
    - [root@servera ~]# docker run --name nginx -d -p 8080:80 --memory="100m" nginx
        
        - swap : Harddisk → Memory
            - 하드디스크 공간을 마치 메모리처럼 사용하는 것.
            - Linux 탄생 시 메모리 가격이 하드디스크 가격보다 월등히 비쌌음.
            - 따라서 swap 기능이 개발됨.
        
        ![[Untitled 35.png|Untitled 35.png]]
        
2. 메모리 업데이트 하기
    
    ![[Untitled 1 2.png|Untitled 1 2.png]]
    

  

컨테이너 CPU 할당법

![[Untitled 2 2.png|Untitled 2 2.png]]

  

**확인이 끝난 후 컨테이너 전부 삭제.**

```
docker rm -f nginxdocker rm -f nginx2docker rm -f nginx3
```

  

## Docker Volume

![[Untitled 3 2.png|Untitled 3 2.png]]

- 컨테이너가 삭제되어도 컨테이너 내부의 데이터는 삭제되지 않도록 하는 기능.
    
    ![[Untitled 4 2.png|Untitled 4 2.png]]
    
- 볼륨의 종류는 세가지.
    - Bind Mount
    - Volumes
    - tmpfs → 사용하지 않는다.
- 볼륨의 특성
    
     스토리지 볼륨은 호스트 운영체제의 파일 시스템을 사용한다. AUFS 와 Overlay 파일시스템 같은  
    Union Filesystem 은 Native 파일시스템 위에 올라가기 때문에 성능이 떨어진다. 스토리지  
    볼륨으로 이런 단점을 극복할 수 있다.  
     스토리지 볼륨은 재사용이 가능하며, 컨테이너들 간에 공유도 가능하다.  
     스토리지 볼륨은 호스트에서 직접 접근할 수 있다.  
     스토리지 볼륨은 컨테이너가 삭제되어도 계속 유지된다. 기본적으로 컨테이너와 독립적으로  
    운영되기 때문이다
    
- Union File System
    - container 안의 파일 변경 사항을 UnionFS을 통해 관리
    - UnionFS은 이미지 layer와 write layer를 합쳐 container의 데이터 관리하는 데, container 삭제 시 write layer도 삭제
        - 데이터의 휘발성
    - write layer에는 이미지 layer의 데이터에서 변경된 사항 저장하므로 write layer 삭제 시 데이터 사라짐(데이터 휘발성)
    - container의 데이터 휘발성 때문에 데이터를 container가 아닌 호스트에 저장
    - 또는 container끼리 데이터 공유할 때 Volume 사용

  

- 도커 볼륨 사용방법

1. docker volume 종류 파악
    
    ![[Untitled 5 2.png|Untitled 5 2.png]]
    
2. docker volume 생성 및 할당하기
    
    ![[Untitled 6 2.png|Untitled 6 2.png]]
    
    - 컨테이너에 저장된 볼륨의 데이터와 호스트에 저장된 볼륨의 데이터가 같다.
    - 컨테이너를 삭제해도 호스트에 저장된 볼륨은 삭제되지 않는가?
        
        ![[Untitled 7 2.png|Untitled 7 2.png]]
        
3. Docker Volume 공유하기
    
    - 두번째 컨테이너 생성 및 볼륨 할당하기
        
        ![[Untitled 8 2.png|Untitled 8 2.png]]
        
    - 컨테이너 삭제 후 볼륨 확인
        
        ![[Untitled 9 2.png|Untitled 9 2.png]]
        
    - Bind-mount 사용하기
        
        - Bind-mount 란 호스트의 특정 디렉토리(or 파일)을 container 와 매핑해서 사용하는 방법을 의미한다.  
            이를 위해 미리 마운트시킬 디렉토리를 준비해야 한다. Volume 의 위치를 사용자가 정할 수 있으므로  
            데이터를 찾기 쉬운 장점이 있다
        - 볼륨 마운트와 차이점
            - bind mount는 직접 경로를 설정한다?
        
        1. 호스트 디렉토리 생성 및 컨테이너에 할당
            
            ![[Untitled 10 2.png|Untitled 10 2.png]]
            
            - z : SELinux 기능 무력화 (3번째 행)
        2. 볼륨 사용하기
            
            ![[Untitled 11 2.png|Untitled 11 2.png]]
            
        
        - 컨테이너 mysql 삭제 후에도 호스트 상의 볼륨에는 데이터가 존재한다.
            
            ![[Untitled 12 2.png|Untitled 12 2.png]]
            
    
      
    
    - bind-mount 공유하기
        
        - 공유의 뜻? → 실시간 최신화를 지원하는가?
            - 호스트 디렉토리에 파일을 생성하면 컨테이너에도 적용이 되는가?
            - 컨테이너가 이미 생성된 후에도 그 내부의 파일이 실시간으로 공유가 되는가?
                - index2.html을 만든 후 이를 nginx-1 /usr/share/nginx/html 에 복사한 후에 해당 파일을 nginx-2에서 확인
                    
                    ![[Untitled 13 2.png|Untitled 13 2.png]]
                    
        
        1. 첫번째 컨테이너에 공유 디렉토리 마운트
            
            ![[Untitled 14 2.png|Untitled 14 2.png]]
            
        2. 두번째 컨테이너에게 디렉토리 공유 확인하기
            
            ![[Untitled 15 2.png|Untitled 15 2.png]]
            
            ![[Untitled 16 2.png|Untitled 16 2.png]]
            
        
          
        

  

  

  

## Docker Image 관리 (중요)

  

Docker Image 란?

- Read-Only 템플릿.
- Image는 Container 를 생성하기 위해 존재.
- Image 는 다른 image를 기반으로 하여 존재.
- Image는 누군가에게서 만들어질 수 있으며, repository에 저장됨.
    - repository : 이미지 저장소
- 이미지를 만들기 위해서 **Dockerfile**을 만들어야 한다.
    - 약간의 문법이 존재
- Dockerfile 은 image 내부에 layer를 생성한다.
- Dockerfile 은 image 생성시 **Lightweight, small, and fast** 라는 철학을 바탕으로 생성된다.

  

- Docker 공식 문서에서 설명하고 있는 Docker Images
    
    **Docker Images**
    
     A read-only template with instructions for creating a  
    Docker container  
     An image is based on another image, with some  
    additional customization  
     You might create your own images or you might only use  
    those created by others and published in a registry  
     To build your own image, you create a Dockerfile with a  
    simple syntax for defining the steps needed to create the  
    image and run it  
     Each instruction in a Dockerfile creates a layer in the  
    image. When you change the Dockerfile and rebuild the  
    image, only those layers which have changed are rebuilt  
     This is part of what makes images so lightweight, small,  
    and fast, compared to other virtualization technologies
    
      
    

Docker 와 Podman 에서의 차이점

- Docker
    - Dockerfile
- Podman
    - Containerfile

  

Docker Image Build

- Build
    - Dockerfile을 Images 로 만들기
- Tag
    - image의 버전(?)
    - private으로 생성 시 포트번호는 기본으로 :5000으로 설정되어있음.
- base image는 일반적으로 컨테이너의 OS라고 보면 됨,
    - 하지만 용량이 작아야 하므로, OS의 기능이 조금씩 빠져있어 호스트 OS와 다를 수 있음.

  

**Docker는 UnionFS (Union File System)를 사용하여 컨테이너를 실시간으로 생성 및 삭제 할 수 있게 되는 것.**

  

  

  

## Docker Driver

- Docker에서 기본적으로 설치되는 드라이버
    - graphdriver : container image 관리
        - graphdriver는 Storage Driver
        - /var/lib/docker 내에 저장되어 있는 container image 관련 정보들  
            이용 사용자에게 layered filesystem으로 제공하는 driver
        - built-in graphdriver 로는 btrfs, vfs, auts, devmapper, overlay2 등
        - Pluggable 한 구조 가지고 있기 때문에, 다른 graphdriver 로 변경하여 사용 용이
    - networkdriver : 가상 bridge 등 container network 관리
    - execdriver : container 생성 관리

  

### Dockerfile

![[Untitled 17 2.png|Untitled 17 2.png]]

- Dockerfile이 어디 있는가?
    - docker buil -t web_img .
    - 가장 끝의 ‘ . ’ == 현재 디렉토리에 Dockerfile이 있다.
        
        - 가장 끝에 Dockerfile이 있는 경로를 지정해주어야 한다.
        
          
        

### Docker Image 사용하기

1. Docker image 다운
    
    ![[Untitled 18 2.png|Untitled 18 2.png]]
    
2. 컨테이너 생성 및 확인
    
    ![[Untitled 19 2.png|Untitled 19 2.png]]
    
3. Docker commit
    
    - commit : 현재 실행되고 있는 컨테이너를 이미지로 저장
    - template처럼 commit하여 기본 세팅이 되어 있는 컨테이너를 집단내에서 공유하여 사용할 수 있다. (추천하지는 않는 방법)
    
    ![[Untitled 20 2.png|Untitled 20 2.png]]
    
4. Docker tag (Version of Image)
    
    - 위에서 생성한 도커 이미지에게 이름 및 버전 번호를 부여하는 작업이다. 참고로 아래 작업을 위해  
        미리 [quay.io](http://quay.io/) 의 계정을 생성해야 한다.
    - 계정 생성 후 로그인 시 에러가 발생한다.
        
        - [quay.io](http://quay.io) 계정의 비밀번호를 다시 설정한 후 로그인 하면 로그인이 될 것이다.
        
        ![[Untitled 21 2.png|Untitled 21 2.png]]
        
    
      
    
    - [quay.io](http://quay.io) 에 Image push하기.
        
        ![[Untitled 22 2.png|Untitled 22 2.png]]
        
          
        
    - 업로드가 된 모습
        
        ![[Untitled 23 2.png|Untitled 23 2.png]]
        
5. Docker Save & Load
    
    - 도커 이미지를 파일로 저장하거나 이를 이미지로 불러올 때 사용하는 방법이다
    
    ![[Untitled 24 2.png|Untitled 24 2.png]]
    
      
    
6. Docker Image 삭제(rmi)
    - 위의 과정에서 다루었으므로 pass

  

  

### 5. Dockerfile (Containerfile) 사용법

1. Dockerfile 정의
    - 도커는 기본적으로 이미지가 있어야 컨테이너를 생성하고 동작시킬 수 있다.
    - Dockerfile 은 필요한 최소한의 패키지를 설치하고 동작하기 위한 자신만의 설정을 담은 파일이며 이 파일로 이미지를 생성(빌드)할 수 있다. (참고로 Podman 은 파일 이름이 Containerfile 이다)
    - 패키지 설치, 환경 변수 변경, 설정 파일 변경 등 다양한 작업을 하나하나 컨테이너를 만들고 설정을 적용할 필요 없이 Dockerfile 을 사용하여 적용할 수 있고, 사용자의 실수로 인한 설정 누락 예방 등 다양한 장점이 있다.
2. Dockerfile 이용 이미지 만드는 과정
    - 이를 위해 세가지 즉 파일 저장 디렉토리, Dockerfile 그리고 명령어가 필요하다.
3. Dockerfile 문법 정리
    1. 주석(Comments)
        
        ```
        # 주석 시작# 해당 문장은 제외됨# 파이썬과 같은 방식의 주석
        ```
        
          
        
    2. FROM (base image 실행)
        - 새 컨테이너 이미지는 기존의 Base 이미지에서 시작한다는 의미로 어떠한 시스템, 또는 프로그램을 실행하기 위해 처음부터 운영체제를 설치할 필요가 없다는 의미이다. 이를 일반적으로 ‘Base Image’ 라고 부른다.
        - 문법
            
            ```
            FROM : <image>:<tag>
            ```
            
              
            
    3. RUN
        - RUN 은 Dockerfile 이 이미지를 생성하는 과정에서 실행될 명령어를 지칭한다. 즉 현재 이미지 맨 위의 새 계층에서 명령을 실행하고 그 결과를 저장한다. 저장된 그 결과는 Dockerfile 다음 단계에서 사용된다.
        - RUN 으로 시작하는 명령어는 각각의 새 레이어를 생성하므로 **레이어 수를 최소화(=용량의 최소화)하기 위해 && 접속사를 사용**한다. (RUN 3개 → layer 3개, RUN 1개 → layer 1개)
            - && 접속사 → a && b : a 를 실행한 후 b 를 실행 해라.
            - && \ → 엔터를 통해 각 행을 보기 좋게 만든다.
    4. CMD (↔ ENTRYPOINT)
        - CMD 는 Dockerfile 이 이미지로 생성된 이후, 컨테이너로 만들어지며 **실행되는 ‘첫 명령어’의 기본 인수를 제공**하기 위해 사용된다. Dockerfile 내부에 단 하나의 CMD 명령어만 존재해야 한다.
        - CMD ["httpd", "-D", "FOREGROUND"]
    5. ENTRYPOINT (명령어 실행 지정) (↔ CMD)
        - ENTRYPOINT 는 **컨테이너를 생성할 때 실행되는 기본 명령어를 지정**한다. 이는 마치 ubuntu 라는 리눅스 운영체제 안에서, 즉 리눅스라는 프로그램 자체 안에서 여러 개의 프로그램을 실행시킬 수 있듯이, 그것을 조정할 수 있다.
        - ENTRYPOINT [“httpd”]  
            CMD ["-D", "FOREGROUND"]
    6. WORKDIR
        - WORKDIR 는 현재 경로를 나타낸다. 더 정확히 말하면, 생성할 컨테이너 내의 현재 경로를 나타낸다. 대개 컨테이너가 실행되기 이전에 수행할 어떠한 작업들을 하기 이전에, base directory 같은 개념이 되는 경로를 지칭해준다.
    7. COPY & ADD
        - COPY 는 이미지 생성 시, 호스트의 파일을 이미지 내부로 복사할 수 있는 명령어이다. 대개 설정 파일 등을 복사할 때에 사용된다. 그러나 원격지의 파일 복사는 지원하지 않는다.
        - 정확한 문자열 대신에 간단한 정규식도 지원하므로 여러 개의 파일을 복사하는 것도 가능하다.
        - 주의할 점은, **호스트에 있는 파일을 복사할 때, Dockerfile 파일이 존재하는 경로보다 하부 경로에 위치하는 파일만 복사 가능**하다.
        - 이에 반해 ADD 는 로컬 또는 원격지의 파일을 복사하여 컨테이너의 파일 시스템에 추가할 수 있다.
    8. USER
        - USER 는 컨테이너 내에서 명령들이 실행될 때에 이를 실행하기에 필요한 사용자를 설정해주는 명령이다.
    9. VOLUME
        - 컨테이너 안에 있는 데이터는 컨테이너를 삭제하면 모든 데이터가 같이 삭제(휘발성 데이터) 되기 때문에 데이터를 보존하기 위해 VOLUME 을 사용한다.
        - VOLUME 명령은 설정한 컨테이너의 데이터를 호스트 OS 에 저장하거나, 컨테이너들간에 데이터를 공유가 가능하게 할 수 있다.
        - Dockerfile 에서 아래와 같이 생성한 볼륨은 호스트 OS 의 /var/lib/docker/volumes 에 생성되며, Docker 에서 자동 생성한 hash 값으로 디렉토리가 생성된다.
    10. EXPOSE
        
        - EXPOSE 는 컨테이너가 런타임에 지정된 네트워크 포트에서 수신한다는 의미이다.
        
        ```
        EXPOSE 80
        ```
        
        - 참고로 docker run 의 -p 옵션은 호스트의 포트를 노출하며 여기서의 포트는 EXPOSE 명령에 나열할 필요가 없다.
    11. ENV (image) & ARG (container)
        - ENV 는 컨테이너에서 사용할 수 있는 환경변수를 정의한다.
        - 이를 이용해서 Dockerfile 내부에 다양한 ENV 명령을 사용할 수 있다.
        - ENV 는 Dockerfile 또는 컨테이너 안에서 환경 변수로 사용이 가능하고, ARG 는 Dockerfile 에서만 사용이 가능하다.
        - ARG 의 경우 Dockerfile 작성하기 위해 필요한 변수를 선언하여 Dockerfile 을 좀 더 편하게 작성가능.
        - ENV 는 변수 선언은 물론, 컨테이너 안에서 사용할 환경 변수(작업 디렉토리 지정) 등 선언하여 사용할 수 있다.
    12. LABEL
        
        - LABEL 은 이미지에 일반적인 메타데이터를 추가하기 위해 사용된다. 이는 단순히 키:값 쌍으로 구성된다.
        
        ```
        LABEL version="3.0" \description="This is an sample container image" \creationDate="01-10-2020"
        ```
        
    13. ONBUILD
        
        - ONBUILD 는 빌드 완료 후 생성된 이미지가 다른 Dockerfile 에서 FROM 으로 불러질 때 실행되는  
            명령이다.
        
          
        
4. Dockerfile 작성하기
    1. Dockerfile 작성하기
        
        ![[Untitled 25 2.png|Untitled 25 2.png]]
        
    2. 스트립트 파일 작성하기
        
        ![[Untitled 26 2.png|Untitled 26 2.png]]
        
        ![[Untitled 27 2.png|Untitled 27 2.png]]
        
    3. 이미지 생성하기
        
        ![[Untitled 28 2.png|Untitled 28 2.png]]
        
    4. 이미지 생성 확인하기
        
        ![[Untitled 29 2.png|Untitled 29 2.png]]
        
    5. 컨테이너 생성 및 테스트
        
        ![[Untitled 30 2.png|Untitled 30 2.png]]
        
          
        
        ![[Untitled 31 2.png|Untitled 31 2.png]]
        
          
        
          
        

## Docker GUI 사용법

- Docker는 GUI 지원이 잘 되지 않는다.
    - 따라서 GUI 프로그램(Portainer)을 사용.
- Portainer 는 도커 및 쿠버네티스를 관리하기위한 GUI 이미지이다.
    - Portainer
        - Docker 컨테이너 관리
        - 컨테이너 내부에서 명령어, 컨테이너 로그들을 쉽게 확인할 수 있기 때문에 사용하기 매우 편리하다.
- Portainer 의 경우 도커 관리툴 중에서 오픈소스이고 가장 많이 사용되는 툴이라 할 수 있다.

  

1. Potainer 설치하기
    
    ![[Untitled 32 2.png|Untitled 32 2.png]]
    
    - Portainer 설치에 앞서 먼저 사용할 볼륨을 생성한다. Portainer 는 컨테이너 이미지 형태로 제공된다.
    - 도커 환경은 컨테이너 생성시 필요한 이미지가 없을 때 자동으로 다운로드를 하며 따라서 다음과 같은 명령을 실행하여 다운로드와 컨테이너를 동시에 실행할 수 있다.
        - --restart=always
            - 도커를 재시작 했을 때 Portainer 컨테이너가 자동으로 시작
        - -p 9000:9000
            - 9000 번 포트로 Portainer 관리 페이지 접근
2. Potainer 접근하기 ([http://서버](http://xn--hk3b17f/) IP:9000)
    
    ![[Untitled 33 2.png|Untitled 33 2.png]]
    
    - 원하는 비밀번호를 입력하고 접속하면 된다.
    
      
    
    ![[Untitled 34 2.png|Untitled 34 2.png]]
    
      
    
3. Container 생성 정보 입력
    
    ![[Untitled 35 2.png|Untitled 35 2.png]]
    
      
    

## 7. Docker Image Deep Dive

- **Docker Image는 사실 변경이 가능하다.**
    - **하지만 매우 많고 deep한 내용들을 알아야 변경 할 수 있게 된다.**
    - 변경 할 필요성도 존재하지 않는다.
    - 또한 우리가 필요로 하는 기능을 가진 이미지는 이미 [https://hub.docker.com/](https://hub.docker.com/) 에 매우 많이 존재한다.
        - 또한, 각 이미지 별로 Dockerfile도 존재한다.
        - 해당 Dockerfile을 사용하여 얼마든지 변경, 삭제 등의 수정 작업이 가능하다.

  

## 8. Docker Registry (Private) 사용법

- **Docker 이미지 저장소로 사용되는 Docker registry 생성 및 사용**하는 방법을 설명한다.
- 여기선 servera 와 serverb 두 개의 노드를 사용하여 테스트를 진행한다. (server a : client / server b : server)

  

1. 원격 Docker Registry 설정 및 사용하기
    
    - serverb 에서 Registry 를 설정하고 servera 에서 이를 테스트하는 순서로 진행
    
      
    
    1. Servera node 에서 Docker Registry 시작
        
          
        
        - 볼륨을 mount하는 이유
            - 실수로 registry가 삭제되면 해당 registry 내의 모든 image가 사라지기 때문에 volume을 mount하여 데이터의 안전성을 보장할 수 있다.
    2. Servera node 에서 업로드 및 다운로드 테스트
        
        ```
        # 서버 a에서 서버b 접속시 어떠한 인증이나 권한이 필요 없도록 한다.{ "insecure-registries":["serverb.example.com:5000"] }
        ```
        
        - /etc/hosts 에는 server ip 와 name을 이어주는 정보가 존재. (DNS와 같은 기능을 하며, DNS 보다 우선순위가 높다.)
        
        ![[Untitled 36.png]]
        
        - 이후 scp 를 이용하여 serverb 의 hosts 파일도 변경해준다.
        
          
        
        ![[Untitled 37.png]]
        
        - server a의 이미지를 server b에 push한 모습.
        
          
        
        ![[Untitled 38.png]]
        
        - server a의 이미지를 삭제하고, server b에 push했던 이미지를 다시 pull하여 가져오는 모습.
    3. Serverb 노드에서 업로드 이미지 확인
        
        ![[Untitled 39.png]]
        
        - serverb에 이미지가 정상적으로 push되어 있는 것을 볼 수 있다.
        
          
        
2. Docker Registry 에서 SSL(Secret Socket Layer) 사용하기 (암호화 적용)
    
    - https = http + SSL
    
      
    
    1. Private key 생성
        
        - Private / Public key 구분
        - serverb 의 private key를 생성.
        
        ![[Untitled 40.png]]
        
    2. Private key 설정 / 인증서 (certs) 생성
        
        ![[Untitled 41.png]]
        
    3. Doker의 경로로 복사 - key, csr, crt (Doker가 사용할 수 있도록)
        
        ![[Untitled 42.png]]
        
    4. SSL 레지스트리 생성
        
        - 443 : https 가 사용하는 포트.
        
        ![[Untitled 43.png]]
        
    5. servera 에서 테스트
        
        ![[Untitled 44.png]]
        
        - curl ~ /_catalog : 목록 보기
        
          
        
    6. SSL 테스트 (servera)
        
        - servera의 브라우저(현재 가상머신)에서 https://[serverb.example.com](https://serverb.example.com:5000/) 에 접속하여 인증서의 정보를 확인 할 수 있다.
        
        ![[Untitled 45.png]]
        
    
      
    
3. Docker Registry 에서 인증 사용하기
    
    - Docker registry 사용시 인증을 처리하는 방법을 아래에서 설명한다.
    - 아래의 실습을 위해 위에서 생성한 registry 컨테이너를 먼저 삭제해야 한다.