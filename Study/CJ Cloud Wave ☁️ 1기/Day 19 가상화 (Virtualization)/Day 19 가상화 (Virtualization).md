  

## Docker Applications (WordPress)

- 도커를 이용하여 WordPress 애플리케이션 설치하는 방법을 설명한다.
- 아래 실습은 docker swarm 이 구성된 환경에서 진행한다.

  

1. WordPress 설치
    
    - 먼저 MariaDB 컨테이너를 생성하고 다음에 WordPress 컨테이너를 생성한다
    
    1. MariaDB 컨테이너 생성
        1. 데이터베이스 사용자 / 암호 생성하기
            
            ```
            openssl rand -base64 20 | docker secret create root_db_password -
            ```
            
            - 랜덤 문자 20개 생성
            - “ | ” 이전의 결과를 “ - ” 이후에 적용하겠다.
            
            ![[Untitled 46.png|Untitled 46.png]]
            
              
            
        2. MariaDB 컨테이너 생성하기
            
            ```
            [root@servera ~]# docker service create --name mariadb \> --replicas 1 --network wp --constraint=node.role==manager \# constraint: 제한 -> role이 manager, 즉 master 노드에만 서비스를 생성하겠다.> --secret source=root_db_password,target=root_db_password \> --secret source=wp_db_password,target=wp_db_password \> -e MYSQL_ROOT_PASSWORD_FILE=/run/secrets/root_db_password \# FILE : 직접 비밀번호 설정하지 않고 FILE을 불러옴 > -e MYSQL_PASSWORD_FILE=/run/secrets/wp_db_password \> -e MYSQL_USER=wp -e MYSQL_DATABASE=wp mariadb:10.1
            ```
            
            ![[Untitled 1 3.png|Untitled 1 3.png]]
            
              
            
        3. WordPress 컨테이너 생성
            
            ```
            [root@servera ~]# docker service create --name wp \> --constraint=node.role==worker --replicas 1 \> --secret source=wp_db_password,target=wp_db_password,mode=0400 \> --publish 80:80 --network wp \> -e WORDPRESS_DB_USER=wp \> -e WORDPRESS_DB_PASSWORD_FILE=/run/secrets/wp_db_password \> -e WORDPRESS_DB_HOST=mariadb \> -e WORDPRESS_DB_NAME=wp wordpress:4.7
            ```
            
            ![[Untitled 2 3.png|Untitled 2 3.png]]
            
              
            
    2. WordPress 설치
        
        ![[Untitled 3 3.png|Untitled 3 3.png]]
        
        - WordPress 설치가 완료된 모습

  

  

  

## 13. Docker Monitoring

![[Untitled 4 3.png|Untitled 4 3.png]]

  

**Monitoring Tools**

1. Prometheus ([prometheus.io](http://prometheus.io/))
    
    - 데이터 수집하는 중간 매개체 역할
    - 여러 곳으로부터 들어오는 데이터의 흐름 Prometheus 서버 하나로 보내주면 거기 데이터가 쌓이고, 관리자는 그 곳에서 데이터 가공하고 분석.
    - 여러 곳으로부터 데이터 받아 수집할 수 있으며, 이를 분석할 수 있는 쿼리 제공
    - Kubernetes와 Containerd가 속한 CNCF에 Prometheus는 등록되어 있는 툴
    
      
    
2. Grafana ([grafana.com](http://grafana.com/))
    
    - 데이터를 시각화해주는 매우 편리한 툴
    - Prometheus, Graphite 등의 데이터 수집 서버로부터 데이터 제공받아 이를 사용자 환경에 맞게 시각화
    - 웹 브라우저 기반
    
      
    
3. Cadvisors ([github.com/google/cadvisor](http://github.com/google/cadvisor)) → 도커 관련 데이터 수집
    - 도커 호스트에 컨테이너로서 실행되는 모니터링 툴
    - 도커 엔진 및 컨테이너, 이미지 등에 대한 데이터 수집
    - ... /metrics endpoint로 데이터 수집
4. Node Exporter ([prometheus.io](http://prometheus.io/)) → 도커를 실행하는 호스트 관련 데이터 수집
    
    - CPU, Memory, Disk 사용량과 같은 호스트 관련 metric 수집하여API로 노출
    - 해당 노드, 즉 호스트 자체 모니터링하기 위한 툴
    - 컨테이너로서 실행, CAdvisor와 다른 점은 도커 엔진이 아닌 호스트 자체에 대한 데이터 주로 제공
    - 이 또한 /metrics endpoint로 데이터 수집
    
      
    
      
    

Cadvisor 설정하기 (servera, serverc)

- 설정은 Cadvisor -> node-exporter -> Prometheus -> Grafana 순서로 이루어진다.
- 여기서 Prometheus, Grafana 는 serverb 에서 그리고 Cadvisor, node-exporter 는 servera, serverc 에서 생성한다.
- 혹시 swarm이 진행되었다면 해제해주도록 하자.
    - Monitoring은 각각의 데이터를 관측해야하기 때문에 swarm을 필요로 하지 않는다.

![[Untitled 5 3.png|Untitled 5 3.png]]

![[Untitled 6 3.png|Untitled 6 3.png]]

  

  

**node-exporter 설정하기(servera, serverc)**

- 두번째는 node-exporter 를 생성해야 한다. 이 작업을 완료하면 터미널이 종료되지 않으므로 아래 작업을 위해 다른 터미널을 사용해야 한다.
- serverc 의 설정도 동일하다.
    
    ![[Untitled 7 3.png|Untitled 7 3.png]]
    

  

  

**Prometheus 설정하기(serverb)**

- 세번째는 Promethesus 컨테이너를 생성하는 것이다

1. vim 파일작성
    
    ```
    scrape_configs: - job_name: 'prometheus' static_configs: - targets: ['localhost:9090'] - job_name: cadvisor scrape_interval: 5s# Prometheus 가 아래 정보를 사용해서 cadvisor / node_exporter를 찾을 수 있게 된다. static_configs: - targets: ['213.0.113.4:8080','213.0.113.5:8080'] - job_name: 'node_exporter' scrape_interval: 5s static_configs: - targets: ['213.0.113.4:9100','213.0.113.5:9100']
    ```
    
    ![[Untitled 8 3.png|Untitled 8 3.png]]
    
      
    
2. Prometheus 설치 및 생성
    
    ```
    [root@servera ~]# docker run -d --name prometheus \# -h : 호스트 네임으로 통신. /etc/hosts에 prometheus 라는 이름이 저장됨docker run -d --name prometheus \-h prometheus -p 9090:9090 \-v $(pwd)/prometheus-cadvisor.yml:/etc/prometheus/prometheus.yml prom/prometheus:v1.7.0 \--config.file "/etc/prometheus/prometheus.yml"
    ```
    
      
    
3. [http://213.0.113.4:9090](http://213.0.113.4:9090) 접속하여 prometheus 확인
    
    ![[Untitled 9 3.png|Untitled 9 3.png]]
    

  

**Grafana 설정**

  

1. Grafana 설정하기
    
    ![[Untitled 10 3.png|Untitled 10 3.png]]
    
      
    
2. Grafana 접속 및 설정
    
    ![[Untitled 11 3.png|Untitled 11 3.png]]
    
    - grafana가 이전에 생성한 prometheus를 찾은 모습.
3. 893 숫자는 Grafana 에서 Prometheus 환경을 제공하기 위한 대시보드의 숫자이다
    
    ![[Untitled 12 3.png|Untitled 12 3.png]]
    
      
    
4. prometheus를 선택해주고 Import.
    
    ![[Untitled 13 3.png|Untitled 13 3.png]]
    
      
    
5. 결과
    
    ![[Untitled 14 3.png|Untitled 14 3.png]]
    

  

1. 컨테이너 별로 CPU / 메모리 사용량 등을 Monitoring 할 수 있다.
    
    ![[Untitled 15 3.png|Untitled 15 3.png]]
    
      
    
      
    
      
    

## Docker & Podman

![[Untitled 16 3.png|Untitled 16 3.png]]

**Docker vs Podman** **차이점**

- Docker와 다르게 커널에 직접 이미지 올라가고 위에 컨테이너 동작 구조
- **컨테이너 독립적 실행 가능, 시스템 데몬 등록 통해 컨테이너별 실행/중단 가능**
- 개별적인 컨테이너 운영 가능, Container Engine에 영향을 받지 않음
- 패키지 관리자 통해서 바로 설치 가능, ubuntu 사용자도 간단히 설치 사용
- Docker hub 직접 이미지 가져와 실행 가능
- Dockerfile 작성 통해 직접 실행 가능
- Podman과 Docker 저장 공간 다름: /var/lib/docker, /var/lib/containers
- Podman 과 Docker images 호환 가능

  

**Pod**

![[Untitled 17 3.png|Untitled 17 3.png]]

- 컨테이너 여러 개를 그룹화 한것
- Infra Container
    - 네트워크와 볼륨 정보 제공
- Container
    - 애플리케이션 제공
- conmon
    - Container Monitor
        
        - 컨테이너의 기본 프로세스 감시하고 컨테이너가 죽으면 종료 코드 저장
        - podman이 분리 모드(백그라운드)에서 실행되도록 하여 podman은 종료할 수 있지만 conmon은 계속 실행
        - 각 컨테이너에는 고유한 conmon 인스턴스 존재
        
          
        

  

## Why Podman?

- 도커 명령어 그대로 사용가능
- 실행하지 않을 파일을 위해 대기 중인 데몬이 존재하지 않으므로 도커보다 가벼움
- 돈

  

## Podman Practices

1. Podman 설치 및 사용하기
    
    ```
    [root@servera ~]# dnf install podman –y[root@servera ~]# podman version
    ```
    
    ![[Untitled 18 3.png|Untitled 18 3.png]]
    
      
    
      
    
2. Podman은 기본 레지스트리가 3개 이다.
    
    ![[Untitled 19 3.png|Untitled 19 3.png]]
    
    - 위 주소에서 컨테이너를 다운로드 한다.

  

1. Podman은 다운받을 대상 레지스트리를 지정해서 Pull 해주어야 한다.
    
    ```
    podman pull docker.io/nginx
    ```
    
    ![[Untitled 20 3.png|Untitled 20 3.png]]
    
      
    
2. /etc/containers/registries.conf 에서 가장 자주 사용할 레지스트리를 앞으로 옮겨준다.
    
    ![[Untitled 21 3.png|Untitled 21 3.png]]
    
      
    
3. Pull명령어 이후 레지스트리 선택창에서 가장 앞에 지정한 레지스트리가 나타난다.
    
    ![[Untitled 22 3.png|Untitled 22 3.png]]
    
      
    
      
    

Podman Volume 사용하기

1. Volume 생성 및 사용
    
    ![[Untitled 23 3.png|Untitled 23 3.png]]
    
      
    
2. Volume 사용 확인
    
    ![[Untitled 24 3.png|Untitled 24 3.png]]
    
      
    
3. Volume 공유 확인
    
    ![[Untitled 25 3.png|Untitled 25 3.png]]
    
      
    
      
    
4. Volume 삭제하기
    
    ![[Untitled 26 3.png|Untitled 26 3.png]]