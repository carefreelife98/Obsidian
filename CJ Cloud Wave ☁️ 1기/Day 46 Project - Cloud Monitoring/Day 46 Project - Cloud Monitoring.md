  

# Project Introduction

- 심화 프로젝트의 예행 프로젝트

  

# EKS

## IAM

- Amazon EKS node의 Kubelet daemon 은 AWS APIs 에 Call을 함(사용자 측에서).
- 해당 Node들은 AWS API에 call을 하기 위한 권한을 IAM instance profile, 그리고 연결된 IAM Policies 들에 의해 부여받음.
- 따라서 Node를 생성하고 Cluster에 등록하기 전에 해당 Node들에게 IAM Role을 부여해야 함(노드가 실행될 때 사용될 수 있도록).
- 노드 생성 전에 아래와 같은 Policies 를 가진 IAM role을 반드시 생성 할 것.
    - • [`AmazonEKSWorkerNodePolicy`](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AmazonEKSWorkerNodePolicy.html)
    - • [`AmazonEC2ContainerRegistryReadOnly`](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AmazonEC2ContainerRegistryReadOnly.html)
    - IPv4 클러스터 사용시 [`AmazonEKS_CNI_Policy`](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AmazonEKS_CNI_Policy.html)

  

  

## EKS Cluster 생성

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.31.14.png]]

  

  

## EKS Node Group 생성

1. Node Group 이름 설정
2. Node 의 IAM Role 설정
    1. IAM Role 생성 (service : ec2)
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.48.30.png]]
        
        - `**AmazonEKSWorkerNodePolicy**`
        - **`AmazonEC2ContainerRegistryReadOnly`**
        
          
        

## EC2-Bastion

1. bastion ec2 접속 (ssh)
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.20.49.png]]
    

  

1. eks-config.yaml 생성
    
    ```
    $ vi eks-config.yaml
    ```
    
    ```
    apiVersion: eksctl.io/v1alpha5kind: ClusterConfigmetadata:  name: lastDanceOfGooloom  region: ap-northeast-2 # 원하는 리전을 입력합니다.vpc:  id: vpc-0e0cd2a474acff09f # VPC ID를 붙여넣습니다.  cidr: 10.0.0.0/16  securityGroup: sg-09bf39a0321ad789b # EC2 보안그룹에서 Control Plane Security Group의 ID를 붙여넣습니다.  subnets:    private:      ap-northeast-2a:        id: subnet-01bd912ac4e2bd047 # Subnet의 cidr를 잘 확인하고 ID를 붙여넣습니다.        cidr: 172.31.10.0/24			      ap-northeast-2c:        id: subnet-061c2cf9af821ff49 # Subnet의 cidr를 잘 확인하고 ID를 붙여넣습니다.        cidr: 172.31.12.0/24  clusterEndpoints:    publicAccess: false # Cluster에 대한 외부 접속을 차단합니다.    privateAccess: true # VPC 내부 접속을 허용합니다.managedNodeGroups:  - name: eks-nodegroup    instanceType: t2.micro    availabilityZones:      - ap-northeast-2a      - ap-northeast-2c    privateNetworking: true # privateAccess: true 인 경우 true로 설정해야 합니다.    desiredCapacity: 3 # 초기에 생성하는 노드 갯수    minSize: 3 # 최소 노드 갯수    maxSize: 9 # 오토스케일링 시 늘어날 수 있는 최대 노드 갯수    ssh: # use existing EC2 key      allow: false # false 시 노드에 대한 ssh 접속 차단    iam:      withAddonPolicies:        imageBuilder: true # AWS ECR에 대한 권한 추가        albIngress: true  # albIngress에 대한 권한 추가        cloudWatch: true # cloudWatch에 대한 권한 추가        autoScaler: true # auto scaling에 대한 권한 추가    instanceName: EKS-WORKER    volumeSize: 30 # 노드의 볼륨 사이즈
    ```
    
    [코드 출처]([https://341123.tistory.com/6](https://341123.tistory.com/6))
    
      
    
    ```
    $ eksctl create cluster -f eks-config.yaml
    ```
    
    - eksctl create 을 사용하여 적용해보자.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.25.37.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.26.03.png]]
    
      
    
2. 문제 발생
    
    - eksctl 을 통해 eks 클러스터 구성(Build)과정에서
        
        `API server is unreachable`
        
        이라는 에러 발생했지만 빌드는 완료가 됨. (20분 정도 소요)
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.28.56.png]]
        
          
        
    - 이후 `kubectl get nodes` 와 같은 명령어 사용 시 i/o Timeout이 발생.
        
        역시 server API Group을 가져오지 못했다고 함.
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.29.18.png]]
        
          
        
    
      
    
    **문제 해결**
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.30.29.png]]
    
    [출처]([https://docs.microfocus.com/doc/AMX/2022.05/EksTsBastionFailConnection](https://docs.microfocus.com/doc/AMX/2022.05/EksTsBastionFailConnection))
    
    - **원인**
        
        - EKS ControlPlane 의 Security Group - Inbound 규칙에 Bastion 의 접근을 허용해 주어야 한다고 한다.
            
            → 이것 덕분에 bastion의 노드들이 EKS cluster(Control Plane에 K8s API server가 존재하므로) 에 접근하지 못하고 있던 것.
            
        
          
        
    - **문제 해결**
        - 따라서 eksctl create 했던 yaml 파일에 지정된 보안그룹 아이디를 찾아 해당 보안그룹의 인바운드 규칙에 bastion 의 ip를 명시해주어 해결.
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.34.55.png]]
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.53.41.png]]
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.52.37.png]]
            
              
            
3. 클러스터 생성 및 kubectl 명령어 사용 모습
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-13_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.07.17.png]]
    
      
    
      
    

# Cluster Autoscaler & HPA (Horizontal Pod Autoscaler)

> Kubernetes 에서는 다음 기능을 통해 클러스터를 자동으로 확장하거나 축소 가능.

- [Cluster Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)
    - Cluster Autoscaler는 클러스터의 사용가능한 자원과 Pod가 요청하는 자원량을 비교 하여 Node를 증가시키거나 감소시킵니다.
- [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
    - Horizontal Pod Autoscaler는 현재 Pod가 사용 중인 자원에 따라서 필요한 Pod의 개수를 증가시키거나 감소시킵니다.

> 두 가지 Autoscaler 를 사용하면 클러스터 부하에 따라서 Pod, Node 개수를 자동으로 조절되도록 할 수 있다.

  

## Cluster Autoscaler

**노드 증가 Rule**

- 사용자가 요청한 Pod의 자원량 보다 현재 Cluster의 자원이 부족한 경우 노드 증가.

  

**노드 감소 Rule**

- 일정 시간 동안 특정 노드의 사용률 저조 시 해당 노드 감소.

  

**노드 감소 예외 - 아래와 같은 경우 특정 노드의 사용률이 저조하더라도 축소 되지 않음.**

- Controller(예: Deployment, StatefulSet .. )에 의해 제어되지 않는 경우
- Local Storage가 설정되어 있는 경우
- 다른 노드로 Pod가 이동할 수 없는 경우
- Annotation `"cluster-autoscaler.kubernetes.io/safe-to-evict": "false"`이 설정되어 있는 경우

  

  

## Cluster 에 Autoscale 설정하기

- Cluster 생성 과정에서 이미 Autoscale Group은 생성 되었을 것이나, 쿠버네티스 환경에서 해당 Autoscaling Group에 명령을 내리기 위한 설정이 필요하다.

1. 아래 yaml 파일을 적용하여 cluster-autoscaler를 생성하자.
    
    ```
    ---apiVersion: v1kind: ServiceAccountmetadata:  labels:    k8s-addon: cluster-autoscaler.addons.k8s.io    k8s-app: cluster-autoscaler  name: cluster-autoscaler  namespace: kube-system---apiVersion: rbac.authorization.k8s.io/v1kind: ClusterRolemetadata:  name: cluster-autoscaler  labels:    k8s-addon: cluster-autoscaler.addons.k8s.io    k8s-app: cluster-autoscalerrules:  - apiGroups: [""]    resources: ["events", "endpoints"]    verbs: ["create", "patch"]  - apiGroups: [""]    resources: ["pods/eviction"]    verbs: ["create"]  - apiGroups: [""]    resources: ["pods/status"]    verbs: ["update"]  - apiGroups: [""]    resources: ["endpoints"]    resourceNames: ["cluster-autoscaler"]    verbs: ["get", "update"]  - apiGroups: [""]    resources: ["nodes"]    verbs: ["watch", "list", "get", "update"]  - apiGroups: [""]    resources:      - "namespaces"      - "pods"      - "services"      - "replicationcontrollers"      - "persistentvolumeclaims"      - "persistentvolumes"    verbs: ["watch", "list", "get"]  - apiGroups: ["extensions"]    resources: ["replicasets", "daemonsets"]    verbs: ["watch", "list", "get"]  - apiGroups: ["policy"]    resources: ["poddisruptionbudgets"]    verbs: ["watch", "list"]  - apiGroups: ["apps"]    resources: ["statefulsets", "replicasets", "daemonsets"]    verbs: ["watch", "list", "get"]  - apiGroups: ["storage.k8s.io"]    resources: ["storageclasses", "csinodes", "csidrivers", "csistoragecapacities"]    verbs: ["watch", "list", "get"]  - apiGroups: ["batch", "extensions"]    resources: ["jobs"]    verbs: ["get", "list", "watch", "patch"]  - apiGroups: ["coordination.k8s.io"]    resources: ["leases"]    verbs: ["create"]  - apiGroups: ["coordination.k8s.io"]    resourceNames: ["cluster-autoscaler"]    resources: ["leases"]    verbs: ["get", "update"]---apiVersion: rbac.authorization.k8s.io/v1kind: Rolemetadata:  name: cluster-autoscaler  namespace: kube-system  labels:    k8s-addon: cluster-autoscaler.addons.k8s.io    k8s-app: cluster-autoscalerrules:  - apiGroups: [""]    resources: ["configmaps"]    verbs: ["create", "list", "watch"]  - apiGroups: [""]    resources: ["configmaps"]    resourceNames: ["cluster-autoscaler-status", "cluster-autoscaler-priority-expander"]    verbs: ["delete", "get", "update", "watch"]---apiVersion: rbac.authorization.k8s.io/v1kind: ClusterRoleBindingmetadata:  name: cluster-autoscaler  labels:    k8s-addon: cluster-autoscaler.addons.k8s.io    k8s-app: cluster-autoscalerroleRef:  apiGroup: rbac.authorization.k8s.io  kind: ClusterRole  name: cluster-autoscalersubjects:  - kind: ServiceAccount    name: cluster-autoscaler    namespace: kube-system---apiVersion: rbac.authorization.k8s.io/v1kind: RoleBindingmetadata:  name: cluster-autoscaler  namespace: kube-system  labels:    k8s-addon: cluster-autoscaler.addons.k8s.io    k8s-app: cluster-autoscalerroleRef:  apiGroup: rbac.authorization.k8s.io  kind: Role  name: cluster-autoscalersubjects:  - kind: ServiceAccount    name: cluster-autoscaler    namespace: kube-system---apiVersion: apps/v1kind: Deploymentmetadata:  name: cluster-autoscaler  namespace: kube-system  labels:    app: cluster-autoscalerspec:  replicas: 1  selector:    matchLabels:      app: cluster-autoscaler  template:    metadata:      labels:        app: cluster-autoscaler      annotations:        prometheus.io/scrape: 'true'        prometheus.io/port: '8085'    spec:      priorityClassName: system-cluster-critical      securityContext:        runAsNonRoot: true        runAsUser: 65534        fsGroup: 65534        seccompProfile:          type: RuntimeDefault      serviceAccountName: cluster-autoscaler      containers:        - image: registry.k8s.io/autoscaling/cluster-autoscaler:v1.26.2          name: cluster-autoscaler          resources:            limits:              cpu: 100m              memory: 600Mi            requests:              cpu: 100m              memory: 600Mi          command:            - ./cluster-autoscaler            - --v=4            - --stderrthreshold=info            - --cloud-provider=aws            - --skip-nodes-with-local-storage=false            - --expander=least-waste            - --node-group-auto-discovery=asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/<YOUR CLUSTER NAME>          volumeMounts:            - name: ssl-certs              mountPath: /etc/ssl/certs/ca-certificates.crt # /etc/ssl/certs/ca-bundle.crt for Amazon Linux Worker Nodes              readOnly: true          imagePullPolicy: "Always"          securityContext:            allowPrivilegeEscalation: false            capabilities:              drop:                - ALL            readOnlyRootFilesystem: true      volumes:        - name: ssl-certs          hostPath:            path: "/etc/ssl/certs/ca-bundle.crt"
    ```
    
    - 위 파일을 bastion에 직접 vi 로 작성하여 적용해도 되고, 아래 명령어를 사용하여 끌어와도 된다.
    
    ```
    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml
    ```
    

  

1. safe-to-evict=”false” annotation을 추가.
    
    ```
    $ kubectl -n kube-system annotate deployment.apps/cluster-autoscaler cluster-autoscaler.kubernetes.io/safe-to-evict="false"
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-13_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.51.12.png]]
    
    - 특정 노드의 사용률이 저조해도 노드가 감소하지 않도록 해주는 옵션.

  

1. 자신의 Cluster name을 찾아 Cluster 명을 입력 & 저장
    
    ```
    $ kubectl -n kube-system edit deploy/cluster-autoscaler
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-13_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.03.38.png]]
    
      
    

## Autoscaling 설정 후 테스트

```
$ vi test-autoscaling.yaml
```

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: test-autoscalingspec:  replicas: 10  selector:    matchLabels:      service: nginx      app: nginx  template:    metadata:      labels:        service: nginx        app: nginx    spec:      containers:      - image: nginx        name: test-autoscaled-carefree        resources:          limits:            cpu: 300m            memory: 512Mi          requests:            cpu: 300m            memory: 512Mi
```

```
$ kubectl apply -f test-autoscaling.yaml
```

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-13_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.11.42.png]]

- Running 중인 Pod 과 Pending 상태의 Pod 들.
- Pending 상태인 Pod 들은 Node가 추가로 Scale out 하여 자원이 확보된 후 running 될 것이다.

  

  

  

# EKS Prac 2

  

# EKS Cluster 생성 - eksctl cli

```
$ eksctl create cluster \--version 1.27 \--name eks-carefreev1 \--vpc-nat-mode HighlyAvailable \--node-private-networking \--region ap-northeast-2 \--node-type t3.medium \--nodes 2 \--with-oidc \--ssh-access \--ssh-public-key (.pem 키 이름) \--managed
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-13_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.21.22.png]]

  

```
$ kubectl get nodes -o wide
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.29.50.png]]

- 클러스터 생성 확인

  

  

```
$ eksctl utils update-cluster-endpoints --cluster=eks-carefreev1 --private-access=true --public-access=true --approve$ eksctl utils set-public-access-cidrs --cluster=eks-carefreev1 (bastion server의 IP/32) --approve
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_1.53.12.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_1.55.57.png]]

- Cluster Endpoint 가 Public 하게 허용되어 있으므로 Private access 를 허용하나, CIDR 블록 제한으로 bastion 서버에서만 명령을 내려 K8s에 접근할 수 있도록 한다.

  

  

# 2. Amazon EKS에 Istio 설치 및 설정

[참고]([https://devocean.sk.com/experts/techBoardDetail.do?ID=163655&boardType=experts&page=&searchData=&subIndex=&idList=](https://devocean.sk.com/experts/techBoardDetail.do?ID=163655&boardType=experts&page=&searchData=&subIndex=&idList=))

## 최신 버전의 Istioctl 설치 및 path 설정

```
$ curl -sL https://istio.io/downloadIstioctl | sh -$ cp ~/.istioctl/bin/istioctl ~/bin
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.00.40.png]]

  

## istio-operator.yaml 생성

```
apiVersion: install.istio.io/v1alpha1kind: IstioOperatormetadata:  namespace: istio-system  name: istiocontrolplanespec:  profile: default  components:    egressGateways:    - name: istio-egressgateway      enabled: true      k8s:        hpaSpec:          minReplicas: 2    ingressGateways:    - name: istio-ingressgateway      enabled: true      k8s:        hpaSpec:          minReplicas: 2    pilot:      enabled: true      k8s:        hpaSpec:          minReplicas: 2  meshConfig:    enableTracing: true    defaultConfig:      holdApplicationUntilProxyStarts: true    accessLogFile: /dev/stdout    outboundTrafficPolicy:      mode: REGISTRY_ONLY
```

- 가장 기본적인 `default` profile 을 사용한다.
- 모든 component의 `minReplicas`를 2로 설정하여 HA를 보장하고 `PodDisruptionBudget` 으로 인해 istio 버전 upgrade 가 실패하는걸 막는다
- `enableTracing` 을 설정하여 추후에 Datadog 이나 Jaeger 를 통해 distributed tracing 이 가능하게 한다.
- `holdApplicationUntilProxyStarts` 설정으로 istio-proxy 가 완전히 올라오면 서비스가 되게 한다. (Java 같은 경우는 서비스가 올라가는게 한참 걸려서 상관없지만 Go, Python 같은 경우 먼저 올라가서 서비스가 가능한 상태가 먼저 되고 request를 받으려고 하지만 istio-proxy가 준비되지 않아 request가 실패하는 경우가 있다. 물론 deployment 의 readiness/liveness 설정으로 대부분 istio-proxy 가 먼저 뜨지만 로컬에서 개발할때는 빠르게 테스트 하기 위해 readiness/liveness 없이 하기도 한다)
- `accessLogFile` 설정으로 Envoy proxy 의 access log를 콘솔로 남긴다.
- `outboundTrafficPolicy` 설정을 REGISTRY_ONLY 로 하여 오직 `ServiceEntry` 를 통해 허가된 IP/Domain 만 outbound를 허용한다. (이 설정은 개발중에 좀 성가실 수 있다... 하지만 개발을 완료한 후에 적용하려 하면 더욱 힘들고 결국 해당 설정 없이 서비스 하게 되는 경우도 있다) 이를 통해 더욱 secure 한 서비스를 만들 수 있다. (default 설정은 ALLOW_ANY 이다)

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.27.07.png]]

  

```
# Deploy Istio to K8s$ istioctl install -f istio-operator.yaml# Istio 배포 확인$ kubectl get pods -n istio-system
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.30.15.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.31.05.png]]

  

## Namespace Lable 을 통해 Auto Injection 설정하기

```
$ kubectl label namespace csm istio-injection=enabled
```

- 원하는 namespace (Generally `default` namespace) 에 아래와 같이 label을 추가해서 `Istio 가 자동으로 application 을 배포할 때 Envoy sidecar proxy를 주입`하도록 설정한다.

  

- 이제 `default` namespace 에 배포하는 서비스에는 `istio-proxy`가 sidecar 로 같이 올라가서 traffic management, security 등 다양한 Istio 의 기능을 사용할 수 있게 되었다.

  

# [AWS EKS] 3. Istio 와 Application Load Balancer 연결

[참고]([https://devocean.sk.com/experts/techBoardDetail.do?ID=163656&boardType=experts&page=&searchData=&subIndex=&idList=](https://devocean.sk.com/experts/techBoardDetail.do?ID=163656&boardType=experts&page=&searchData=&subIndex=&idList=))

## 개요

- EKS 에서 Istio 를 설치하면 기본적으로 Classic Load Balancer (CLB) 를 사용하여 설치가 된다.
- CLB는 잘 사용하지 않고 권장되지 않으므로 L4 Network Load Balancer(NLB) 또는 L7 Application Load Balancer(ALB)로 변경해야 한다.
- 하지만 [AWS WAF](https://aws.amazon.com/waf/) 와 같은 서비스를 사용하기 위해서는 반드시 ALB를 사용해야 한다.

  

## Architecture 변경사항

- 전체적인 architecture는 이렇게 변경된다.
- 기존 istio ingress gateway의 service type 은 LoadBalancer 에서 NodePort로 변경하여 기존 CLB가 삭제되고, 대신 Kubernetes 의 Ingress resource를 통해 ALB를 생성한다.
- 그리고 ALB에서 TLS termination 을 하고 모든 request를 istio ingress gateway pod로 보내고 istio ingress gateway가 application service로 routing 을 하게 된다.

  

## ACM - 인증서 요청 (무료) → Domain 없으므로 Pass 함

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_3.09.19.png]]

- AWS Certificate Manager 에 접속하여 퍼블릭 SSL/TLS 인증서 요청.

## ACM - TLS 인증서 설치 (AWS CLI) → Domain 없으므로 Pass 함

- AWS cli 를 사용해서 TLS 인증서를 AWS ACM에 import 하자.

  

**Notice**

> The PEM-encoded certificate is stored in a file named `Certificate.pem`.
> 
>   
> 
> The PEM-encoded certificate chain is stored in a file named `CertificateChain.pem`.
> 
>   
> 
> The PEM-encoded, unencrypted private key is stored in a file named `PrivateKey.pem`.

  

## AWS Load Balancer Controller 설치

[참고]([https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html](https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html))

> AWS Load Balancer Controller 은 K8s 를 위한 AWS ELB를 관리하며 다음과 같은 resource를 provision 한다.

- K8s Ingress
    - K8s Ingress 생성 시 AWS Application Load Balancer(ALB) 를 생성한다.
- K8s service of the LoadBalancer type
    
    - K8s service of type LoadBalancer 생성 시 AWS Network Load Balancer(NLB) 를 생성한다. (Target Type을 LoadBalancer 로 추가 지정하여 생성)
    
      
    

**아래와 같이 Bastion 서버에서 설치한다.**

1. IAM Policy 생성
    
    ```
    # US-East / US-West 제외 모든 Region$ curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
    ```
    
    ```
    # US-East / US-West Region$ curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy_us-gov.json
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_3.38.18.png]]
    
    - IAM Policy 다운로드
    
      
    
    ```
    $ aws iam create-policy \    --policy-name AWSLoadBalancerControllerIAMPolicy \    --policy-document file://iam_policy.json
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_3.39.23.png]]
    
    - IAM Policy 생성
    
      
    
2. IAM Role & K8s Service Account 생성
    
    - 직전에 생성한 IAM Policy 를 이용하여 아래와 같이 eksctl create 한다.
    - 여기서 attach-policy-arn 을 위에서 생성한 policy를 추가.
    
    ```
    $ eksctl create iamserviceaccount \  --cluster=gooloom-cluster-v3 \  --namespace=kube-system \  --name=aws-load-balancer-controller \  --role-name AmazonEKSLoadBalancerControllerRole \  --attach-policy-arn=arn:aws:iam::422608248610:policy/AWSLoadBalancerControllerIAMPolicy \  --override-existing-serviceaccounts \  --approve
    ```
    
    - `my-cluster` : cluster 의 이름으로 변경
    - `111122223333` : 자신의 account id 로 변경 422608248610 **gooloom-cluster-v3**
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.00.22.png]]
    
      
    
3. Helm 설치 및 repository 추가
    
    ```
    $ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh$ chmod 700 get_helm.sh$ ./get_helm.sh$ helm version --short | cut -d + -f 1 # 버전 / 설치 확인$ helm repo add eks https://aws.github.io/eks-charts$ helm repo update eks
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.10.19.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.10.04.png]]
    
    - AWS Load Balancer Controller 를 Helm 을 통해 설치하게 된다.
    
      
    
4. AWS Load Balancer Controller 설치
    
    ```
    $ helm install aws-load-balancer-controller eks/aws-load-balancer-controller \  -n kube-system \  --set clusterName=gooloom-cluster-v3 \  --set serviceAccount.create=false \  --set serviceAccount.name=aws-load-balancer-controller \  --set image.repository=602401143452.dkr.ecr.ap-northeast-2.amazonaws.com/amazon/aws-load-balancer-controller
    ```
    
      
    
    ```
    $ kubectl get deployment -n kube-system aws-load-balancer-controller
    ```
    
    - 설치 확인
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.15.48.png]]
    
      
    
      
    
5. Istio 설정 변경
    - 이제 Istio 의 ingress gateway의 service type을 NodePort로 변경하고 ALB가 istio ingreas gateway로 health check 을 할 수 있도록 관련 정보를 넣어준다.

  

1. Health Check 를 위한 Port Number 확인
    
    ```
    $ kubectl get service istio-ingressgateway -n istio-system -o jsonpath='{.spec.ports[?(@.name=="status-port")].nodePort}'
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.18.57.png]]
    
    - 우선 ALB 가 health check을 위해 사용할 istio ingress gateway 의 health check 관련 nodePort number를 알아본다.
    - 얻은 NodePort 를 아래에서 적용한다.

  

1. Istio 설정 Update
    
    - 아래와 같이 이전에 생성했던 istio-operator.yaml 를 업데이트 하자.
        
        ```
        apiVersion: install.istio.io/v1alpha1kind: IstioOperatormetadata:  namespace: istio-system  name: istiocontrolplanespec:  profile: default  components:    egressGateways:    - name: istio-egressgateway      enabled: true      k8s:        hpaSpec:          minReplicas: 2    ingressGateways:    - name: istio-ingressgateway      enabled: true      k8s:        hpaSpec:          minReplicas: 2        service:          type: NodePort # ingress gateway 의 NodePort 사용        serviceAnnotations:  # Health check 관련 정보          alb.ingress.kubernetes.io/healthcheck-path: /healthz/ready          alb.ingress.kubernetes.io/healthcheck-port: "31213" # 위에서 얻은 port number를 사용    pilot:      enabled: true      k8s:        hpaSpec:          minReplicas: 2  meshConfig:    enableTracing: true    defaultConfig:      holdApplicationUntilProxyStarts: true    accessLogFile: /dev/stdout    outboundTrafficPolicy:      mode: REGISTRY_ONLY
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.24.28.png]]
        
        - Istio ingress gateway 의 service type을 NodePort로 설정한다.
        - 전 단계에서 얻은 port number를 healthcheck-port 에 넣어 health check 관련 정보를 istio ingress gateway 에 annotation 으로 저장한다.
    
    업데이트 한 yaml 파일을 Istio에 적용한다.
    
    ```
    $ istioctl install -f istio-operator.yaml
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.29.46.png]]
    
    - 여기까지 하면 기존 ingress gateway의 service type을 기존 `LoadBalancer` 에서 `NodePort`로 변경하였기 때문에 CLB (Classic Load Balancer)는 삭제된다.

  

1. ALB 생성
    
    - 이제 마지막으로 Kubernetes 의 Ingress를 통해 ALB를 생성한다. (AWS Load Balancer Controller는 Kubernetes 의 Ingress 를 통해 ALB 를 생성한다)
    - 아래와 같이 `kube-ingress.yaml` 파일을 생성한다.
    
    ```
    apiVersion: networking.k8s.io/v1kind: Ingressmetadata:  name: ingress-alb  namespace: istio-system  annotations:    kubernetes.io/ingress.class: alb    alb.ingress.kubernetes.io/scheme: internet-facing    alb.ingress.kubernetes.io/certificate-arn: "arn:aws:acm:ap-northeast-2:422608248610:certificate/2c3ee83f-dbe2-45eb-a8fb-d9aa49bc0d2d"    alb.ingress.kubernetes.io/actions.ssl-redirect: '{"Type": "redirect", "RedirectConfig": { "Protocol": "HTTPS", "Port": "443", "StatusCode": "HTTP_301"}}'spec:  rules:  - http:      paths:        - path: /*          pathType: Prefix          backend:            service:              name: ssl-redirect              port:                number: 443        - path: /*          pathType: Prefix          backend:            service:              name: istio-ingressgateway              port:                number: 80
    ```
    
    - certificate-arn 에는 AWS ACM 에 생성한 TLS certificate의 ARN을 입력한다.
    
      
    
    생성 후 아래와 같이 Ingress를 apply 하여 ALB 생성을 완료한다.
    
    ```
    $ kubectl apply -f kube-ingress.yaml
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_5.29.24.png]]
    
    - K8s Ingress 기능 중 Deprecated 된 것들이 많아 yaml 파일 고치는 데 애먹었다..
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_5.29.48.png]]
        

이렇게 기존 CLB를 삭제하고 ALB 로 대체하면서 TLS Termination 은 ALB 에서 일어나고 ALB는 `istio-system` namespace 에 배포되어 있는 `istio-ingressgateway` 로 request를 보내게 되고 `istio-ingressgateway`는 비로소 서비스로 라우팅을 하게 된다.

  

> 여기까지 하면 EKS 에 상용에서 사용가능한 Kubernetes Cluster 생성 및 ALB를 통한 TLS Termination, istio ingress gateway의 라우팅이 가능해 진다. 결국 AWS의 [WAF](https://aws.amazon.com/waf/) 와 같이 ALB가 필수인 서비스를 사용할 수 있다.

  

  

# Kubernetes (AWS EKS) 의 Autoscaler

[출처]([https://devocean.sk.com/experts/techBoardDetail.do?ID=163657&boardType=experts&page=&searchData=&subIndex=&idList=](https://devocean.sk.com/experts/techBoardDetail.do?ID=163657&boardType=experts&page=&searchData=&subIndex=&idList=))

**Kubernetes 의 Autoscaler 를 사용하기 위해서**

1. Deployment 의 Resource request, limit을 모두 설정해야 한다.
2. Horizontal Pod Autoscaler (HPA) 를 설정한다.
3. AWS 의 Cluster Autoscaler (CA) 를 설정한다.

- 여기서 Resource의 request, limit 설정을 위해 [Vertical Pod Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) 및 [Goldilocks](https://goldilocks.docs.fairwinds.com/#how-can-this-help-with-my-resource-settings)를 활용하고 이를 통해 request, limit 을 설정하는데 참고 할 수 있다.

  

## **VPA 및 Goldilocks의 역할**

- 사실 [VPA](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)는 Kubernetes 의 Autoscaler 중 하나로 Horizontal 하게 Pod/ Node를 늘리는 HPA와 달리 Vertical 하게 스펙을 높이는 방법.
- 그런데 VPA controller stack은 recommendation engine 을 포함하고 있고, 가이드라인을 제공하기 위해 현재 resource 사용량을 계속 수집한다. 결국 보통 실제 VPA를 사용한 Autoscale을 사용 하지는 않고, **recommendation engine 만 사용하도록 설정하여 request, limit을 정하고 HPA, CA를 통해 Autoscale을 한다.**
- 여기서 [Goldilocks](https://goldilocks.docs.fairwinds.com/#how-can-this-help-with-my-resource-settings) 의 역할은 불편하게 `kubectl` 을 통해 필요한 서비스의 VPA recommendation 을 확인하는 대신 쉽게 **Web UI**를 통해 이를 확인 할 수 있게 도와준다.

  

## Kube Metrics Server 설치

```
# Kube Metrics Server 설치$ kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml# 설치 확인$ kubectl get deployment metrics-server -n kube-system
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.35.47.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.36.43.png]]

- 지표 수집을 위해 K8s의 Metrics-server를 설치

  

## VPA 및 Goldilocks 설치

```
$ helm repo add fairwinds-stable https://charts.fairwinds.com/stable$ helm install --set updater.enabled=false vpa fairwinds-stable/vpa --namespace vpa --create-namespace$ helm install goldilocks fairwinds-stable/goldilocks --namespace goldilocks --create-namespace
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.39.46.png]]

- Full VPA 가 필요하지 않고 Recommender만 필요.
- 아래와 같이 VPA Recommender와 Goldilocks 를 설치

  

```
$ kubectl label ns default goldilocks.fairwinds.com/enabled=true
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-14_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.42.06.png]]

- VPA Recommender를 사용할 namespace 를 지정한다.

  

## Goldilocks 확인 (실패)

```
$ kubectl -n goldilocks port-forward svc/goldilocks-dashboard 8080:80
```

- Kubernetes port forwarding 을 통해 Goldilocks 에 접속
- kubernetes port forwarding을 remote 에서 하는 경우 localhost를 통해 Goldilocks를 접근하기 위해 local 에서 Local Port Forwarding을 해야한다.

  

# AWS EKS 의 CLuster Autoscaler 설정

  

## IAM Policy 생성

1. cluster-autoscaler-policy.json 생성
    
    ```
    {   "Version": "2012-10-17",   "Statement": [       {           "Action": [               "autoscaling:DescribeAutoScalingGroups",               "autoscaling:DescribeAutoScalingInstances",               "autoscaling:DescribeLaunchConfigurations",               "autoscaling:DescribeTags",               "autoscaling:SetDesiredCapacity",               "autoscaling:TerminateInstanceInAutoScalingGroup",               "ec2:DescribeLaunchTemplateVersions"           ],           "Resource": "*",           "Effect": "Allow"       }   ]}
    ```
    

  

1. policy 생성
    
    ```
    aws iam create-policy \   --policy-name AmazonEKSClusterAutoscalerPolicy \   --policy-document file://cluster-autoscaler-policy.json
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.31.04.png]]
    
    - 위 Command 후 반환되는 ARN을 다음에서 사용.

  

## IAM Role 생성

```
eksctl create iamserviceaccount \--cluster=eks-carefreev1 \--namespace=kube-system \--name=cluster-autoscaler \--attach-policy-arn=arn:aws:iam::767391686884:policy/AmazonEKSClusterAutoscalerPolicy \--override-existing-serviceaccounts \--approve
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.33.40.png]]

- eksctl 사용하여 IAM Role 생성

  

## Deploy Cluster Autoscaler

1. Cluster Autoscaler YAML 파일 다운로드.
    
    ```
    curl -o cluster-autoscaler-autodiscover.yaml https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml
    ```
    
      
    
2. YAML 파일을 열어 <YOUR CLUSTER NAME> 에 자신의 클러스터 이름을 넣어준다.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.43.10.png]]
    

  

1. 적용
    
    ```
    kubectl apply -f cluster-autoscaler-autodiscover.yaml
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.44.14.png]]
    
      
    
2. `cluster-autoscaler.kubernetes.io.safe-to-evict`를 추가
    
    ```
    kubectl patch deployment cluster-autoscaler \ -n kube-system \ -p '{"spec":{"template":{"metadata":{"annotations":{"cluster-autoscaler.kubernetes.io/safe-to-evict": "false"}}}}}'
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.00.34.png]]
    
      
    
3. `cluster-autoscaler` deployment를 수정
    
    ```
    kubectl -n kube-system edit deployment.apps/cluster-autoscaler
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.03.16.png]]
    
    - container command 에 `--balance-similar-node-groups` 와 `--skip-nodes-with-system-pods=false` 를 추가

  

1. Cluster Autoscaler [release](https://github.com/kubernetes/autoscaler/releases) 페이지에서 최신 CA 확인후 적용
    
    ```
    kubectl set image deployment cluster-autoscaler \ -n kube-system \ cluster-autoscaler=k8s.gcr.io/autoscaling/cluster-autoscaler:v1.27.4
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.05.40.png]]
    
      
    
2. Cluster Autoscaler Deploy 후 로그 확인하여 동작 확인
    
    ```
    kubectl -n kube-system logs -f deployment.apps/cluster-autoscaler
    ```
    
      
    
3. OpenSource인 Karpenter도 사용 가능.
    
    [https://karpenter.sh/docs/](https://karpenter.sh/docs/)
    
      
    
      
    

# Loki를 이용한 Kubernetes Logging

## Loki 설치

```
$ helm repo add grafana https://grafana.github.io/helm-charts$ helm repo update$ helm upgrade --install loki grafana/loki-stack --create-namespace --namespace=loki --set grafana.enabled=true,grafana.service.type=LoadBalancer,loki.config.table_manager.retention_deletes_enabled=true,loki.config.table_manager.retention_period=336h,loki.persistence.enabled=true,loki.persistence.size=5Gi
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.12.51.png]]

- Loki Namespace 생성
- 상용을 위해 데이터를 Persist
- Grafana를 외부로 노출
- 최근 2주간의 로그 저장 및 오래된 순서대로 삭제

  

## Loki 확인

```
$ kubectl get svc -n loki
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.15.57.png]]

- 접속 주소 확인
- loki ClusterIP 10.100.25.36:3100/TCP
- loki-grafana LoadBalancer 10.100.228.247:80:31577/TCP

  

```
$ kubectl get secret --namespace loki loki-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.17.54.png]]

- Grafana의 Admin Password 확인