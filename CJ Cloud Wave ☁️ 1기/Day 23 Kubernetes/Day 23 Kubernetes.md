  

[https://bit.ly/3JY3IH5](https://www.notion.so/CloudWave-Kubernetes-2f983e6c7d8e411aa930b5ba9e970e8b?pvs=21)

  

**Micro Service Architecture (MSA)**

- Database가 분리 되어 있어야 함.
    - 정확히는 스키마의 분리.
    - 논리적인 분리.
- 개발을 나눠서 하기 위함.
    - 고객을 위한 서비스를 개발
- Squad 별로 독립적인 서비스를 개발
    - 타 Squad 에 영향을 주지 않기 위함
- High-Cohesion
    - 서로 의존하여 같이 변경되어야 할 서비스들을 하나의 서비스 안에 담는 것
- 회복성
    - 하나의 서비스 장애가 다른 서비스로 전이되지 않음.
        - Kubernetes : Circuit Breaker 를 통해 구현 가능.
    - 전체 장애를 차단하고 기능은 저하시킴.
    - 분산 기술에 대한 깊은 이해 필요.
- 확장성
- 배포 용이성
    - 전체 빌드(Build)가 아닌 서비스 단위의 빌드 및 배포.
    - 테스트 코드 또는 TDD 필요.
    - 컨테이너 환경에 매우 적합.
    - 다같이 야근 안해도 됨.
- Micro Service 의 단점
    - 업무 도메인 구분이 어렵다.(경험의 부족)
        - 업무 도메인 구분하여, 잘게 분리하는 것은 생각보다 복잡한 작업.
    - 운영하기 어려움
        - 작은 서비스들이 흩어져 있어 모니터링이 어려움
        - 산재된 로그 분석이 어려움
            - 메트릭 : 액셀 파일의 컬럼과 같은 것.
            - 로그
            - 트레이스 : 쪼개어져 있는 컴포넌트들의 일련의 과정을 보아 추적하는 것.
        - 운영자가 마이크로 서비스를 구성하는 컴포넌트들에 대해 잘 알고 있어야함 (신속한 장애 대처를 위해)

  

```
Micro Service Architecture (MSA) - LayerApplication(Front)Application(Backend)Integration - 시스템 간의 연동Database
```

  

**Conway’s Law**

- **어플리케이션 아키텍쳐는 그것을 개발하는 조직 구조를 그대로 반영한다.**

  

**Loosely Coupled (느슨한 결합)**

- 하나의 서비스가 변경될 때 다른 서비스가 변경되는 일이 없다.

  

**High-Cohesion**

- 단일 책임 원칙

  

**Bounded-Context (결정 경계)**

- 명료한 경계에 의해 강제된 구체적인 책임을 구분.

  

마이크로 서비스 아키텍쳐 OSS 예시

- Service Discovery
    - 서비스 시작시 (톰캣이 뜰 때) Eureka에 등록.
    - Eureka 가 여러 서비스 중 가용 가능한 서비스를 탐지 / 장애가 발생한 서비스로 접근 불가하게 설정해줌.

  

**Spring Cloud Kubernetes**

  

  

  

  

## Kubernetes 환경 설정

  

**Terraform 다운로드**

```
# M1 Mac$ brew install terraform
```

- IaC
    - Idempotent(멱등성)
    - Ansible
    - **Pulume**
        - 지원하는 언어
            - Java
            - Node
            - Python

  

**Kubernetes cli 다운로드**

```
# M1 Mac$ brew install kubernetes-cli
```

- Kubernetes 명령어 패키지

  

**Telepresence 다운로드**

```
# intel mac$> brew install datawire/blackbird/telepresence# M1 Mac$ brew install datawire/blackbird/telepresence-arm64
```

- NAT Gateway 등을 사용하여 망끼리의 통신에 Proxy 서버를 사용해서 연결해준다.(?)
- 내가 Kubernetes 망 내부에 있는것 처럼 만들어준다.
- 터널링을 사용하여 연결 (Overlay ?)

  

**Helm 다운로드**

```
# M1 Macbrew install helm
```

- Docker Compose
    
    - 메모리, 이미지 등 변동성이 있는 데이터를 변수로 만들어 Compose yaml 파일을 상황에 맞추어 쉽게 생성할 수 있도록 도와주는 것이 Helm.
    
      
    

  

  

## Kubernetes - Container

## **History of Cloud Native**

Google Trend Search - 애자일 선언

- 작게 일해라 → 작은 것부터 개발
- 지속 가능한 속도

  

Google Trend Search - 클라우드와 자동화 도구의 서막

  

Google Trend Search - 컨테이너의 본격적인 확산 (Kubernetes)

  

**추상화?**

- 복잡한 것은 감추는 것 (Encapsulation)
- 필요한 기능만 오픈하는 것 (Interf~)
    - Docker 명령어 → 필요한 것만 생성되어 있다.
- 인터페이스를 표준화 할 것(ISO, IEEE..)
    - Option

  

하드웨어와 소프트웨어가 만나는 경계선

- OS 의 Kernel → 어떻게 추상화 하지?
    - = 리눅스를 사용하자. (표준화 당했다)
    - 모든 리눅스의 커널은 같으나 유틸(배포판)이 다르다.
- 소프트웨어의 추상화?
    - 컨테이너에

  

**Container ?**

- **커널은 공유**하고 **리소스를 격리**해주는 것.
    - Linux의 리소스의 격리 담당 기술 : namespace(격리) / cgroup (제어) (Control Group)
        - 위의 Linux 기술을 바탕으로 만든 것이 Docker

  

VM(가상화) / Container

- VM(가상화) : 하드웨어 관점의 추상화
    - 서버 담당자가 편하다.
- Container : 서비스 관점의 추상화
    - 개발자가 편하다.
- 결론 : 둘 다 같이 쓰는 것이 좋다.
    - 둘의 목적이 다름. 비교 대상이 아니다.

  

따라서, 가장 이상적인 구조는 아래와 같다. (= 추상화 수준 높음)

```
Service MeshKubernetesContainerVirtual Machine
```

  

  

## Dockerfile

Tips

1. 캐싱을 위한 명령어(Layer) 순서를 최적화
    - 각 명령어 한 줄 마다 하나의 Layer.
    - 제일 위의 명령어가 가장 아래 Layer가 된다.
    - 도커 → 이미지 전송시 해쉬값이 바뀐 부분만 전송
2. 캐싱을 위해 정해진 파일만 복사하라
3. 캐싱 단위를 정확하게 구분할 것
4. 항상 자신의 애플리케이션에 테스트된 태그를 지정할 것. (= latest 절대 사용하지 마라)
5. 가능한 용량이 적은 공식 이미지의 사용
    - Dockerfile 명령어 : FROM Scratch
        
        이전의 필요없는 빌드 파일은 무시한다.
        
          
        
          
        

## Kubernetes

- Self-Healing
    - 선언/정의된 상태를 유지하기 위해 Kubernetes는 자신에게 주어진 인프라 리소스 내에서 끊임없이 노력한다.

  

## Kubernetes

- Control Plane → Kubernetes 의 머리. (컨테이너가 올라가지 않음)
    - etcd
        - in-memory data base
    - controller manager
    - scheduler
        - 명령어 입력 시 API server 를 통해 Scheduler 한테 먼저 간다.
        - scheduler 내부의 queue를 통해 최대한의 효율로 작업을 배정한다.
    - RAFT 알고리즘을 통해 Leader node를 선출한다?
- Nodes
    - Kubelet : 일꾼
    - Kube-proxy : 여러개의 VM 네트워크를 하나로
    - Container - Runtime

  

- 컨트롤 플레인은 쿠버네티스 전체 클러스터를 관리.
- 쿠버네티스 오브젝트의 레코드를 유지 및 관리 (제어루프=컨트롤러 매니저)
- Control Plane(Master Node) 의 상세 구성
    
    - API 서버 : 쿠버네티스 오브젝트에 댇한 데이터를 설정하고 검증. REST 서비스 제공
    - Scheduler
    
      
    
- Node(Worker)

  

  

## ETCD

- 모든 구성정보를 담고 있다.

  

## XML, JSON, YAML

  

  

## 쿠버네티스 자동 설치

- MiniKube → 아주 작은 테스트용?
- K3s →
- Kind

  

  

## [Kubernetes] Yaml & Kind 사용

```
kind: ClusterapiVersion: kind.x-k8s.io/v1alpha4 # 버전/nodes:- role: control-plane # control-plane 생성  # image 이름 / 버전(tag) / hash값 (이미지의 더 상세한 정보를 제공)  image: kindest/node:v1.27.3@sha256:9dd3392d79af1b084671b05bcf65b21de476256ad1dcc853d9f3b10b4ac52dde  kubeadmConfigPatches:    # | : 다음 attribute가 나올때까지 한 문장으로 인식함.  - |    kind: InitConfiguration    nodeRegistration:      kubeletExtraArgs:        node-labels: "ingress-ready=true"  extraPortMappings:  - containerPort: 80 # Kubernetes 내부망의 포트    hostPort: 80 # VM 의 포트    protocol: TCP  - containerPort: 443    hostPort: 443    protocol: TCP- role: worker  image: kindest/node:v1.27.3@sha256:9dd3392d79af1b084671b05bcf65b21de476256ad1dcc853d9f3b10b4ac52dde- role: worker  image: kindest/node:v1.27.3@sha256:9dd3392d79af1b084671b05bcf65b21de476256ad1dcc853d9f3b10b4ac52ddenetworking:  serviceSubnet: "10.120.0.0/16"  podSubnet: "10.110.0.0/16"
```

```
$ kind create cluster --name cwave-cluster --config kind-3-node-clusters.yaml
```

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.49.32.png]]

- 다음과 같이 설정이 완료되면 성공이다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.50.11.png]]

  

Kind

- VM 이 아닌 Container 를 생성한다.
- Control /

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.08.43.png]]

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.09.33.png]]

- ns : namespace

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.09.48.png]]

  

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.10.35.png]]

  

  

  

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

- nginx 설치
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.17.07.png]]
    
      
    

  

```
kubectl wait --namespace ingress-nginx --for=condition=ready pod --selector=app.kubernetes.io/component=controller --timeout=90s
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.25.54.png]]

- ingress : 들어오는 트래픽을 받아주는 것. (내 네트워크로 들어오는거)
    - 외부와 내부를 이어주는 네트워크 서비스
    - ingress 를 통해 nginx 를 다운하는 과정.

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.26.57.png]]

  

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.35.32.png]]

- 쉘 띄우기

  

## POD

- POD 는 쿠버네티스 애플리케이션의 기본 실행 단위 (만들고 배포할 수 있는 가장 작은 단위)
- 파드 당 컨테이너의 비율은 대체로 1:1이지만 (관례) , 파드 당 여러개의 컨테이너가 포함되는 경우도 있음.
- 파드 내의 컨테이너는 오로지 하나의 노드 내에만 존재.
    - 노드를 걸쳐서 존재하지 않음.
- 동일 파드 내의 컨테이너는 네트워킹 및 볼륨을 공유한다.
- 파드 끼리의 통신은 무조건 가능하다. (Kube Proxy 를 통해서)
- K8s 에서 HOST Network 는 Container이다.

  

Kubernetes resources

- Kubernetes 의 resource는 매우 많은 종류가 있다.
- 각 resource 마다 API Group이 있다.
    - core 는 pod와 같은 메인 기능을 가짐 → v1뒤에 Group 정보를 붙혀주지 않아도 된다.
- 각 API Group은 version이 존재.
    - Alpha
    - Beta
    - Stable

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.17.24.png]]

```
kubectl delete all --all
```

- namespace 안의 모든 리소스를 삭제

  

  

## 라벨 및 주석 기본

**라벨 이란?**

- 리소스에 부여하는 임의의 key/value 쌍
- DB Query 와 같은 필터링 기능을 함.
    - where~ 절처럼 자신이 원하는 정보만 라벨을 사용해 추출할 수 있다.
- 트래픽을 다운스트림으로 줄 때 IP 통신을 하지 않는다.
    - IP = 변동 가능성이 있기 때문에
    - 라벨의 포트 등의 정보를 보고 적절한 곳으로 트래픽을 보내준다.

  

**주석 이란?**

- 라벨과 같은 기능
- 길이가 라벨보다 더 길다.

## Label 확인

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.39.49.png]]

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.41.07.png]]

  

AND 조건 확인

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.46.09.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.46.23.png]]

  

OR 조건 확인

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.46.34.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.46.46.png]]

  

  

## Label Selector

- 쿠버네티스 에서는 어떤 노드로 포드가 스케줄 될 지 알 수 없다.
- Label을 이용해서 특정 조건에 맞는 노드로 스케줄링이 가능.
- GPU 의 유무, 메모리, SSD 유무를 노드의 라벨에 적용

  

```
$ kubectl get no$ kubectl get node$ kubectl get nodes
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.10.09.png]]

  

  

  

**edit 으로 라벨 바꾸기**

```
$ kubectl edit no 노드이름
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.23.07.png]]

  

**Label 지우기**

```
$ kubectl label no (노드이름) (Labelkey=SSD)-
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.32.42.png]]

  

  

## 연습문제

  

  

  

  

  

  

Kubernetes Cheat Sheet 사용하기 - 시험에도 사용 가능

[https://kubernetes.io/ko/docs/reference/kubectl/cheatsheet/](https://kubernetes.io/ko/docs/reference/kubectl/cheatsheet/)

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.13.17.png]]