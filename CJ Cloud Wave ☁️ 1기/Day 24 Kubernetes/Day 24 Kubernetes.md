  

## Review

  

- kubectx = K8s Context

  

- 기존 노드에 이미 올라간 POD의 label을 edit로 바꾸게 되면 해당 POD가 죽은 뒤 다시 살리는 과정에서 스케줄러가 변경사항을 인지하고 수정된 label에 맞는 노드로 재배정해준다.

  

- etcd
    
    etcd는 분산 시스템에서 사용되는 오픈 소스 분산 키-값 저장소입니다. CoreOS에서 개발되었으며, Kubernetes와 같은 컨테이너 오케스트레이션 시스템 및 다른 분산 시스템에서 구성 정보, 설정 데이터, 중요한 메타데이터 등을 안전하게 저장하기 위해 사용됩니다.
    
    etcd는 간단하고 효율적인 API를 제공하여 분산 환경에서 강력한 일관성과 가용성을 제공합니다. 이는 Raft 합의 알고리즘을 기반으로 하며, 여러 노드에 데이터를 분산 저장하여 장애 대비와 가용성을 보장합니다. 따라서 etcd는 클러스터 환경에서 중요한 설정 데이터를 관리하고 공유하는 데 매우 유용합니다.
    
    etcd의 핵심 기능은 다음과 같습니다:
    
    1. 분산 키-값 저장소: 각 키는 해당하는 값과 연결되며, 클러스터의 모든 노드에 데이터가 분산 저장됩니다.
    2. 일관성과 내구성: Raft 합의 알고리즘을 사용하여 데이터의 일관성과 내구성을 보장하며, 클러스터 노드 중 일부가 고장나도 데이터 손실이 없습니다.
    3. Watch 기능: 클라이언트는 특정 키를 모니터링하고, 해당 키에 변경이 발생하면 이벤트를 실시간으로 받아볼 수 있습니다.
    4. TTL(생존 시간) 설정: 데이터에 TTL을 설정하여 자동으로 만료되도록 할 수 있습니다.
    5. 롤백 기능: 이전 상태로 클러스터를 롤백하는 기능을 제공하여 문제 발생 시 쉽게 복구할 수 있습니다.
    
    이러한 기능들로 인해 etcd는 많은 분산 시스템에서 구성 관리, 서비스 디스커버리, 분산 락 관리 등에 활용되고 있으며, 특히 Kubernetes 클러스터의 중요한 구성 요소 중 하나로 사용되고 있습니다.
    

  

- CAP 이론?

  

## 이론 수업

## Annotation 생성 / 삭제

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.36.42.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.37.34.png]]

  

## Namespace (K8s)

1. 네임스페이스를 사용하는 이유
    
    - 격리
    - 한번에 여러 리소스를 다룰 수 있다. (한 namespace 내의 리소스 한번에 삭제)
    - 권한을 줄 수 있다.
    
      
    
2. Namespace vs Cluster ?
    
    - 개발 환경과 운영 환경을 나누는 상황에서는 클러스터로 구분하는 것이 좋다. (namespace 보다)
    
      
    
3. 라벨 셀렉터에서 다중 조건 사용
    - 쿠버네티스의 객체이름의 범위를 제공하는것
    - 동일한 리소스 이름을 여러 namespace에서 여러 번 사용가능(개발/검증/운영 에서 동일한 리소스 명 사용)
    - 멀티 - 테넌트 환경으로 분리하는 효과

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.38.20.png]]

- Namespace 확인하기

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.43.10.png]]

  

## namespace 연습문제

- bitnami 라는 네임스페이스를 생성하세요
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.13.32.png]]
    
- 이후 리소스는 bitnami 네임스페이스에 생성하세요
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.13.46.png]]
    
- bitnami/apache 이미지로 pod를 만들고 tier=FronEnd, app=apache 라벨 정보를 포함하세요
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.24.02.png]]
    
- Pod 정보를 출력 할 때 라벨을 함께 출력 하세요
- app = apache 라벨을 가진 Pod 만 조회하세요
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.25.32.png]]
    
- 만들어진 Pod에 env=dev 라는 라벨 정보를 추가하세요
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.26.30.png]]
    
- created_by=kevin 이라는 Annotation을 추가 하세요
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.28.34.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.28.45.png]]
    
- apache Pod를 삭제 하세요
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.29.17.png]]
    

  

  

## RD / CRD

  

- RD (쿠버네티스 내에 존재하는 것)
    
    1. **Liveness Probe (감시)**
        - K8s는 Liveness Probe를 통해 컨테이너의 Health Check 를 수행.
        - Health Check 가 실패할 경우 POD 를 다시 시작.
        - 설정 값
            - **공통 설정**
                - initialDelaySeconds
                    - pod가 시작되고 최초의 liveness probe가 수행 되기까지의 대기시간
                        - POD가 뜨는 시간이 존재.
                        - 해당 시간에 Health check가 들어가게 되면 POD가 죽은 것으로 판단, 같은 POD 하나가 또 생성 → 반복
                        - 따라서 자신의 프로그램이 실행 되기까지의 최대 시간을 측정하여 설정해주어야함.
                - priodSeconds
                - timeoutSeconds
                - successThreshold
                - failureThreshold
            - **HTTP 추가 설정**
                - host
                - scheme
                - path
                - httpHeaders
                - port
    2. Replication Controller
        
          
        
    3. ReplicaSet - Replication Controller 를 대체
        - Replication Controller와 동일하지만, 더 풍부하고 상세한 표현(조건)을 사용할 수 있다.
            - 값을 사용 (Value)
                - In
                - NotIn
            - 키를 사용 (Key)
                - Exists
                - DoesNotExists
    
      
    

  

## Liveness 실습

1. yaml 파일 생성
    
    ```
    apiVersion: v1kind: Podmetadata:  name: liveness-pod # pod 이름  labels:    name: liveness-http # label 이름spec:  containers:  - name: liveness-container # container 이름    image: k8s.gcr.io/liveness    args:     - /server    livenessProbe:      httpGet:        Path: /healthz        port: 8080        httpHeaders:        - name: Custom-Header          value: Awesome      initialDelaySeconds: 5      periodSeconds: 3    resources:      limits:        memory: "128Mi"        cpu: "500m"    ports:      - containerPort: 8080
    ```
    
2. POD 생성
    
    ```
    $ kubectl apply -f liveness-prob-pod.yaml
    ```
    

  

## Replication Controller 실습

1. rc.yaml 생성
    
    ```
    apiVersion: v1kind: ReplicationControllermetadata:  name: goapp-rc # replication controller 이름spec:  replicas: 3 # 몇 개 띄울지  selector:    app: goapp  template: # POD    metadata:      name: goapp-pod      labels:        app: goapp    spec:      containers:        - name: goapp-container          image: dangtong/goapp          ports:            - containerPort: 8080
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.09.01.png]]
    
      
    
2. 3개의 POD 중 임의의 1개 POD Label 삭제
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.10.09.png]]
    
    - Replicas=3 의 옵션을 유지하기 위해 Replication Controller 가 Label이 붙어있는 POD 하나를 더 생성한다.
    - Label이 사라진 POD은 사라지지 않고 Running 상태만 유지한다.
3. 기존에 Label을 삭제했던 POD에 다시 Label을 붙여준다.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.12.32.png]]
    
    - 이전에 설정된 개수(3)를 맞춰주기 위해 가장 최근에 생성된 POD가 사라짐으로서 Replicas=3 을 유지해주는 것을 볼 수 있다.
4. Replication Controller는 Spring의 Connection Pool과 맥락적으로 비슷한 기능을 한다고 생각할 수 있다.
    - Label 로만 다룰 수 있다.
    

  

## ReplicaSet / Replication Controller 연습문제

1. yaml 파일 생성

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.14.21.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.14.56.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.20.03.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.19.53.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.21.31.png]]

## 참고

- kubectl 치기 번거로울 때 아래와 같은 명령어를 사용 시
    
    ```
    \#Window$ Set-Alias k 'kubectl'\#mac & linux$ alias k='kubectl'
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.37.23.png]]
    
    - ‘k’ 한 글자로 편하게 사용 할 수 있다.
    
      
    
- [12Factor.net](https://12factor.net/ko/) → 프로그래머로서 알아야 하는 12가지

  

  

## 서비스(Service) 란?

- K8s 특성상 노드 또는 Pod에 장애가 발생할 경우~
- 어떤 곳에 띄워져 있는 것이 아닌, 설정(Configuration File) 같은 느낌이다?

  

## K8s 에서 제공하는 서비스의 종류

- K8s 에서는 4가지의 서비스를 제공.
    
    → 전부 Load Balancing을 위한 서비스.
    
      
    
    **내부 접속**
    
    - **ClusterIP (Default)**
        - 각각의 Pod에 배달해주는 것 (Round Robin …)
    
      
    
    **외부 접속**
    
    - **NodePort**
        - Node Port, ClusterIP 둘 다 생성된다.
        - 외부에서 접속하기 위해 사용.
        - 30000 초과의 포트번호만 사용 가능
    - **LoadBalancer**
        - L4 : Virtual host 불가.
        - Loadbalance 오브젝트는 K8s 내부에 존재.
            - 하지만 Loadbalancer는 K8s 외부에 존재한다.
    - **Ingress**
        
        - L7
            - Virtual host:
                
                하나의 웹 서버에 여러 개의 IP(도메인)를 가질 수 있다.
                
        
          
        

## ClusterIP 란?

- kube-proxy 기반으로 동작 (IPTables 또는 IPVS 모드 선택 가능)
    - IPVS 모드: 방화벽 생성 시 사용
- IPTables 모드에서 라우팅은 랜덤 선택
- IPVS 모드에서는 아래와 같은 라우팅 알고리즘 선택가능
    - RR (Round Robin) : 번갈아 가며 한번씩
    - LC (Least Connection) : 최소연결(커넥션이 가장 적은 노드에 라우팅)
    - DH (Destination Hashing) : 목적지 해싱
    - SH (Source Hashing) : 소스 해싱
    - SED (Shortest Expected Delay) : 최단 지연 예상
    - NQ (Never Queue)

  

## NodePort 란?

- NodePort 서비스는 전체 노드에 걸쳐 생성되어 있다.

  

## LoadBalancer 란?

  

## Ingress 란?

- NodePort + 외부 로드밸런서
- 반드시 Ingress Controller 가 있어야 한다. (Nginx 등..)

  

  

  

# Deployment - 관심사의 분리

→ 버전 관리를 위해 만들어진 개념

- RS (복제)
    - POD (재가동)

  

- maxSurge
    - 기존에 선언된 Replicas 의 최대 개수에서 버전 업 과정 중 더 추가적으로 생성할 수 있는 POD 의 개수
- maxUnavailable
    - 기존에 선언된 Replicas 의 최대 개수에서 버전 업 과정에서 작동되지 않아도 되는 POD의 개수

장점

1. 설정 및 사용 쉬움
2. 전체 인스턴스들에 신규 버전이 천천히 배포됨.
3. 데이터 Rebalancing 이 진행되는 ~

  

## Deployment Strategy

1. recreate
    - 서비스 버전 업 과정에서 서비스가 되지 않는 시간~

  

  

1. Blue / Green
    - 기존 버전, 신규 버전을 두 운영계에서 운영.
        - 기존 버전은 서비스 운영중.
        - 신규 버전을 운영하는 운영계에서는 갖가지 테스트를 실시.
            - 신규 버전에 문제가 없으면 기존 버전을 운영하던 운영계로 설정된 Routing을 신규 버전이 준비된 운영계로 돌려 버전업.
2. A/B testing
    - 각 사용자에 맞춰 두 개의 버젼을 운영?
3. Shadow
    - 새로운 버전을 테스트 하기 위해서 기존 버전에 들어오는 트래픽을 복사해서 새로운 버전에 똑같이 흘려줌.

  

## Deploy 실습

1. yaml 생성
    
    ```
    apiVersion: apps/v1kind: Deploymentmetadata:  name: nginx-deployspec:  selector:    matchLabels:      app: nginx  template:    metadata:      labels:        app: nginx    spec:      containers:      - name: nginx-container        image: nginx:1.18        resources:          limits:            memory: "128Mi"            cpu: "500m"        ports:        - containerPort: 80
    ```
    

  

1. apply
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.52.06.png]]
    
      
    
2. 서비스 생성
    1. deploy-service.yaml 생성
        
        ```
        apiVersion: v1kind: Servicemetadata:  name: nginx-servicespec:  selector:    app: nginx  ports:  - port: 8080 # 받을 포트    targetPort: 80 # nginx의 포트
        ```
        
          
        
    2. apply
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.03.52.png]]
        
          
        
    3. telepresence 를 사용해서 service에 접속하기
        
        ```
        $ telepresence helm install
        ```
        
        - install
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.07.34.png]]
            
        
          
        
        ```
        $ telepresence connect
        ```
        
        - connect / login
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.07.49.png]]
            
        
          
        
        ```
        $ kubectl get svc
        ```
        
        - get SVC 를 통해 ClusterIP를 얻어 해당 IP로 브라우저에서 접속
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.08.02.png]]
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.08.13.png]]
            
        
          
        

# 실습

# Context

Config 파일을 이용하여 여러 개의 클러스터에 쉽게 접근할 수 있도록 하는 것이 context입니다. kubectl을 이용하여 CLI로 쿠버네티스 작업을 할 때, 어느 클러스터 혹은 네임스페이스에 작업을 할지를 결정할 수 있습니다.

## Context 1.

  

  

## Context 2. AWS

ID : cwave25

  

1. AWS 설정
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.27.28.png]]
    
      
    
      
    
      
    
      
    

## Context 3. Terraform

  

variable.tf

- 변수를 저장하기 위한 terraform
- 변경 할 것과 변경하지 않을 것을 구분
- instance type,

  

dev-aws-tfvars

- 변수를 입력하기 위한 terraform
- 변수를 여기에 넣어주면 variable.tf 에 저장된다.

main.tf

  

  

1. terraform workspace 생성
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.30.09.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.30.18.png]]
    
    - workspace: 자신이 활동한 모든 기록이 저장됨.
    
      
    
2. terraform init
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.31.35.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.31.53.png]]
    
      
    
      
    
3. dev-aws.tfvars 설정 (K8s 를 AWS에서 사용하기 위해)
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.43.15.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.43.26.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.43.41.png]]
    

  

1. terraform plan --var-file=dev-aws.tfvars
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-19_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.02.40.png]]
    
    - 해당 terraform 실행 시 어떻게 결과가 나올지를 보여준다.
    - 여기까지 성공하면 EKS가 90% 생성된다고 볼 수 있다.