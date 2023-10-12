  

```
# for ARM systems, set ARCH to: `arm64`, `armv6` or `armv7`ARCH=amd64PLATFORM=$(uname -s)_$ARCHcurl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"# (Optional) Verify checksumcurl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --checktar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gzsudo mv /tmp/eksctl /usr/local/bin
```

  

# kubectl Installation

```
$ curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.23.6/bin/linux/amd64/kubectl$ chmod +x ./kubectl$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin$ echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc$ kubectl version --short --client
```

**curl -LO https://dl.k8s.io/release/v1.23.6/bin/linux/amd64/kubectl**

curl -LO "https://dl.k8s.io/**v1.23.6**/bin/linux/amd64/kubectl.sha256"

  

```
$ curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl$ chmod +x ./kubectl$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin$ echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc$ kubectl version --short --client
```

  

gooloom-cluster-v2.yaml

```
apiVersion: eksctl.io/v1alpha5kind: ClusterConfigmetadata:  name: gooloom-cluster-v1  region: ap-northeast-2 # 원하는 리전을 입력합니다.vpc:  id: vpc-04a25dc65a16e7ef9 # VPC ID를 붙여넣습니다.  cidr: 10.0.0.0/16  securityGroup: sg-0f0d09ae6989843a3 # EC2 보안그룹에서 Control Plane Security Group의 ID를 붙여넣습니다.  subnets:    # must provide 'private' and/or 'public' subnets by availibility zone as shown    ### public    public:      public-a:        id: subnet-0dc43fdbfa7cf7577        cidr: 10.0.0.0/20        # (optional, must match CIDR used by the given subnet)      public-b:        id: subnet-01fe8fb560113bfca        cidr: 10.0.16.0/20        # (optional, must match CIDR used by the given subnet)      public-c:        id: subnet-0e01925b727c60084        cidr: 10.0.32.0/20        # (optional, must match CIDR used by the given subnet)    ### private    private:      private-cache-a:        id: subnet-0d7754996698a58f7 # Subnet의 cidr를 잘 확인하고 ID를 붙여넣습니다.        cidr: 10.0.128.0/20        # (optional, must match CIDR used by the given subnet)      private-cache-b:        id: subnet-0628ecb1764f4fd28 # Subnet의 cidr를 잘 확인하고 ID를 붙여넣습니다.        cidr: 10.0.144.0/20        # (optional, must match CIDR used by the given subnet)      private-cache-c:        id: subnet-055fa79e7f4ed3b01 # Subnet의 cidr를 잘 확인하고 ID를 붙여넣습니다.        cidr: 10.0.160.0/20        # (optional, must match CIDR used by the given subnet)      private-db-a:        id: subnet-08c82861cf361d226        cidr: 10.0.176.0/20        # (optional, must match CIDR used by the given subnet)      private-db-b:        id: subnet-02da564ffe3ad0733        cidr: 10.0.192.0/20        # (optional, must match CIDR used by the given subnet)      private-db-c:        id: subnet-01bbbb5e9c34f455a        cidr: 10.0.208.0/20        # (optional, must match CIDR used by the given subnet)  clusterEndpoints:    publicAccess: false # Cluster에 대한 외부 접속을 차단합니다.    privateAccess: true # VPC 내부 접속을 허용합니다.managedNodeGroups:  - name: csm-cache-ng    instanceType: t3.medium    privateNetworking: true # privateAccess: true 인 경우 true로 설정해야 합니다.    subnets:      - private-cache-a      - private-cache-b      - private-cache-c    desiredCapacity: 3 # 초기에 생성하는 노드 갯수    tags:      nodegroup: csm-cache    minSize: 3 # 최소 노드 갯수    maxSize: 9 # 오토스케일링 시 늘어날 수 있는 최대 노드 갯수    ssh: # use existing EC2 key      allow: false # false 시 노드에 대한 ssh 접속 차단    iam:      withAddonPolicies:        imageBuilder: true # AWS ECR에 대한 권한 추가        albIngress: true  # albIngress에 대한 권한 추가        cloudWatch: true # cloudWatch에 대한 권한 추가        autoScaler: true # auto scaling에 대한 권한 추가    instanceName: csm-WORKER    volumeSize: 50 # 노드의 볼륨 사이즈  - name: csm-db-ng    instanceType: t3.medium    privateNetworking: true # privateAccess: true 인 경우 true로 설정해야 합니다.    subnets:      - private-db-a      - private-db-b      - private-db-c    desiredCapacity: 3 # 초기에 생성하는 노드 갯수    tags:      nodegroup: csm-db    minSize: 3 # 최소 노드 갯수    maxSize: 9 # 오토스케일링 시 늘어날 수 있는 최대 노드 갯수    ssh: # use existing EC2 key      allow: false # false 시 노드에 대한 ssh 접속 차단    iam:      withAddonPolicies:        imageBuilder: true # AWS ECR에 대한 권한 추가        albIngress: true  # albIngress에 대한 권한 추가        cloudWatch: true # cloudWatch에 대한 권한 추가        autoScaler: true # auto scaling에 대한 권한 추가    instanceName: csm-WORKER    volumeSize: 50 # 노드의 볼륨 사이즈
```

  

  

# Upgrade K8s Version

```
eksctl upgrade nodegroup \  --name=gooloom-cache-ng \  --cluster=gooloom-cluster-v2 \  --region=ap-northeast-2
```

  

```
eksctl upgrade nodegroup \  --name=gooloom-db-ng \  --cluster=gooloom-cluster-v2 \  --region=ap-northeast-2
```

  

```
eksctl upgrade nodegroup \  --name=gooloom-cache-ng \  --cluster=gooloom-cluster-v2 \  --region=ap-northeast-2 \  --kubernetes-version=1.26
```

  

Error

```
aws eks update-kubeconfig --region ap-northeast-2 --name gooloom-cluster-v2
```