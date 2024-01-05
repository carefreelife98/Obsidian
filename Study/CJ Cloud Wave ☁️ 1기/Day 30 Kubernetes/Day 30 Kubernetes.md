  

## ArgoCD

  

[topology.kubernetes.io/region:](http://topology.kubernetes.io/region:) asia-northeast3  
[topology.kubernetes.io/zone:](http://topology.kubernetes.io/zone:) asia-northeast3-a

  

[kubernetes.io/hostname:](http://kubernetes.io/hostname:) gke-cwave25-default-pool-d52addb6-27wp

  

  

## Kubernetes 계정

유저 계정 : 일반적으로 사용하는 ID

- ROOT 계정, IAM 계정..
- 주로 클러스터 관리에 사용

서비스 계정

  

Role Binding

  

PV : 클러스터 수준

  

PVC : Namespace 수준

  

## Taint

  

  

```
spec:	containers:	...		env:		- name: GET_HOSTS_FROM			value: dns
```

- 컨테이너 스펙에 환경변수 입력

  

모든 트랜잭션은 redis backlog에 기록된다.

Slave가 Master에게서 데이터를 가져오는 것이 효율적.

- Read만 하면되기 때문에 더 가볍다.
- Master가 Slave에게 써주는 것은 상대적으로 더 무거움.

  

따라서, Slave노드에 환경변수 env를 설정하여 Master주소를 알려준다.

또한, Frontend에도 해당 변수를 통해 Master를 거쳐 Slave에게서 데이터를 가져갈 수 있도록 할 수 있다.’

  

  

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.23.42.png]]

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.49.02.png]]

8OjevrAG8EkfPcUt

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.48.49.png]]

  

  

## Cordon

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.16.03.png]]

  

dbyun@redhat.com

010-8544-0170

  

  

  

  

  

# CI/CD 파이프라인 구축해보기

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.14.29.png]]

- 프로젝트 생성 및 Yaml 파일 작성

## 전체 구조

- Redis_finalPrac
    
    - guestbook
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.15.17.png]]
        
        - frontend-deployment.yaml
        
          
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.17.35.png]]
        
        - frontend-loadbalancer.yaml
    
      
    
    - redis
        - redis-master
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.21.32.png]]
            
            - redis-master-deployment.yaml
            
              
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.21.46.png]]
            
            - redis-master.yaml
            
              
            
        - redis-slave
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.22.08.png]]
            
            - redis-slave-deployment.yaml
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.22.22.png]]
            
            - redis-slave-service.yaml
            
              
            

1. Cluster 구축하기
    1. 새로운 namespace 생성
        
        ```
        $ kubectl create ns redis-prac
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-26_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.18.48.png]]
        
          
        
    2. 기존에 작성해둔 Service, Deployment Apply하여 Pod 생성.
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-26_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.22.47.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-26_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.00.13.png]]
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-26_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.00.58.png]]
        
          
        
2. Git 연동
    1. Git repository 생성 및 연동
        
        ```
        $ git init$ git remote add https://github.com/carefreelife98/redis.git$ git add . -> Git repo 에 올라갈 파일이 있는 경로$ git commit -am "new ArgoCD"$ git push
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.05.27.png]]
        
          
        
3. ArgoCD 네임스페이스 생성 및 설치
    
    ```
    $ kubectl create namespace argocd$ kubens argocd$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.08.01.png]]
    
      
    
4. ArgoCD 서버 설정
    1. argocd-server 를 ClusterIP가 아닌 LoadBalancer로 변환 시켜준다.
        
        ```
        $ kubectl edit service argocd-server
        ```
        
          
        
5. ArgoCD 로그인
    1. 로그인을 위해 Secret에 저장되어 있는 Password를 찾는다.
        
        ```
        $ kubectl get secret$ kubectl get secret argocd-initial-admin-secret -o yaml
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.15.54.png]]
        
          
        
    2. base 64 decoding 진행.
        
        ```
        $ echo "(PASSWORD)" | base64 -d
        ```
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.36.08.png]]
        
        - 반환되는 문자열이 ArgoCD 비밀번호이므로 잘 복사해두자.
        
          
        
    3. ArgoCD 접속
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.59.12.png]]
        
        - argocd-server의 Type이 LoadBalancer로 바뀌었고, EXTERNAL-IP 가 생성된 것을 볼 수 있다.
        - 해당 IP를 사용하여 접속해보자.
        
          
        
    4. ArgoCD 접속 모습
        
        ![[ArgoCD.png]]