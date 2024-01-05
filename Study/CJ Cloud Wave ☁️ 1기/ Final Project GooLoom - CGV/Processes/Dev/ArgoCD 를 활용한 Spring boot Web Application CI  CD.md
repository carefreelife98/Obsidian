  

> 본 진행 과정은 AWS EC2 로 생성된 Bastion server에서 진행됩니다.

  

## ArgoCD 설치

  

1. 공식 홈페이지에서 제공하는 설치 방법.
    
    ```
    $ kubectl create namespace argocd$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.05.52.png]]
    

  

1. 설치 후 확인
    
    ```
    $ kubectl get po -n argocd
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.09.11.png]]
    

  

1. ArgoCD 초기 Admin 비밀번호 확인
    
    ```
    $ kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.11.43.png]]
    
    - 위 비밀번호는 다음 스텝에서 사용되므로 복사해두자.
    - vHSYrrZhhjJAm326

  

1. [ ArgoCD CLI ] ArgoCD 초기 비밀번호 변경
    1. ArgoCD Pod에 직접 접근 후 Login
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.19.56.png]]
        
        ```
        $ kubectl exec -it -n argocd deployment/argocd-server -- /bin/bash
        ```
        
        ```
        $ argocd login localhost:8080
        ```
        
        - username : admin
        - password : 이전 step에서 얻어낸 admin password 입력
        
          
        
    2. ArgoCD CLI 를 사용해서 비밀번호 변경. (ArgoCD container 에 이미 CLI가 준비되어 있다.)
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.25.57.png]]
        
        ```
        $ argocd account update-password
        ```
        
        - 기존 password 입력 후 새로운 password 등록
        - gooloom4

  

1. 현재 단계까지 진행하던 중 ArgoCD 의 접근 편리성? 을 위해 ALB 가 가장 처음 마주하는 첫번째 Private Subnet (WAS 동작 중) 에 존재하는 EKS 클러스터에 다시 설치를 진행하였다.

  

1. ArgoCD 로드 밸런서 생성하기 (참고 :[https://velog.io/@junsugi/Argo-CD-를-AWS-EKS-에-사용하기](https://velog.io/@junsugi/Argo-CD-%EB%A5%BC-AWS-EKS-%EC%97%90-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0))
    
    - **설치한 ArgoCD 에 접속하기 위해** **`K8s의 Ingress`****를 통해 외부와 통신할 수 있도록 해준다.**
        
        > **Ingress 란?**  
        >   
        > 한 네임스페이스 안에 여러개의 서비스들이 존재하고 서비스 안에는 여러개의 파드가 존재할 수 있다. 이때 외부에서 인그레스의 라우팅을 이용, 원하는 서비스에 접근해서 실제 애플리케이션에 접근할 수 있도록 해주는 리소스이다. 그림은 아래와 같고, 쿠버네티스 공식 홈페이지에서 가져왔다.
        > 
        > ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.56.27.png]]
        
    
      
    
    ```
    apiVersion: networking.k8s.io/v1kind: Ingressmetadata:  annotations:    alb.ingress.kubernetes.io/certificate-arn: <인증서 arn>    alb.ingress.kubernetes.io/healthcheck-path: /    alb.ingress.kubernetes.io/healthcheck-protocol: HTTP    alb.ingress.kubernetes.io/listen-ports: '[{"HTTPS":443}, {"HTTP":80}]'    alb.ingress.kubernetes.io/scheme: internet-facing    alb.ingress.kubernetes.io/target-type: ip    kubernetes.io/ingress.class: alb  finalizers:  - ingress.k8s.aws/resources  labels:    app: velog-test    tier: backend  name: argocd-ingress # 인그레스 이름 정하기  namespace: argocd    # 설치할 네임스페이스spec:  rules:  - http:      paths:      - backend:          service:            name: argocd-server # 연결할 서비스 (이부분은 고정)            port:              number: 80 # (이부분도 고정)        path: /*        pathType: ImplementationSpecific
    ```
    
    - ingress 생성을 위한 yaml 파일.
    - YAML 파일을 보면 `spec` 밑에 `service` 부분에 `argocd-server` 라는 부분이 적혀있는데 해당 서비스 내부에 `Argo CD` 홈페이지가 설치되어 있고 우리는 홈페이지 UI에 접근하려고 해당 서비스를 `Ingress` 에 연결한 것이다.
    
      
    
      
    
    ```
    kubectl apply -f <ingress yaml 경로> -n argocd
    ```
    
    ```
    $ pwd/home/ec2-user/argocd/(yaml 이름).yaml$ kubectl apply -f (pwd에서 나온 yaml 파일 경로) -n argocd
    ```
    
    - 위 파일을 .yaml 파일로 생성한 후, argocd namespace에 apply하여 적용해주자.
    
      
    
      
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.27.58.png]]
    
    ```
    $ kubectl get ing -A
    ```
    
    - ingress 를 조회해보면 `ADDRESS` 를 볼 수 있다.
    
      
    
2. EKS Cluster 보안그룹에 ArgoCD ALB 허용하기
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.33.28.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.35.13.png]]
    
    - 위 경로에서 클러스터 보안그룹을 찾아 편집해주자.
    - in-bound 규칙 : 모든 트래픽 → ArgoCD 보안그룹

  

  

1. `argocd-server` Service 편집하기
    
    ```
    $ kubectl edit service argocd-server -n argocd
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.21.52.png]]
    
    - 가장 아래 type: ClusterIP 부분을 LoadBalancer 로 수정.
    
      
    
      
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.23.32.png]]
    
    ```
    $ kubectl get svc -n argocd
    ```
    
    - service 검색 후 ArgoCD-server 의 Type이 LoadBalancer 로 수정된 것을 확인.
    - 생성된 해당 External-IP로 접속하면 접속 성공.

  

1. 접속 성공
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.24.19.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.29.18.png]]
    
    - ID : admin
    - PW : gooloom4

  

## ArgoCD에 Git Repository 연동

  

1. ArgoCD 접속 → settings → Connect Repo
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.34.30.png]]
    

  

  

  

  

  

  

## Github에 Helm Chart Push

  

  

  

## ArgoCD 활용, AWS EKS 환경에 Spring boot Web Application 배포

  

1. Deployment 작성을 위한 WAS Application image의 성능 확인
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_1.19.23.png]]
    
    - WAS 이미지를 도커 컨테이너에서 그냥 running test
        - CPU : 약 30% 사용
        - Memory(7.5 GiB) : 약 5% 사용

  

1. deployment.yaml 작성 및 Git Push
2. 배포되는 모습.

  

  

## 3 Tier 구조 - EKS Cluster 두 개 (WEB / WAS) 에 각각 배포하기

1. [ArgoCD CLI] ArgoCD 주소로 Login
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.23.47.png]]
    
    ```
    $ argocd login (ArgoCD 주소)
    ```
    
    - `kubectl get svc -n argocd` 해서 나오는 External-IP 주소로 로그인
    
      
    
2. [ArgoCD CLI] Login 한 ArgoCD 에 새로운 Cluster 추가하기
    
    ```
    $ argocd cluster add <클러스터 arn 명>
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_3.59.40.png]]
    
    [[ Trouble Shooting exec plugin error - no valid apiVersion- v1alpha1]]
    
      
    
3. 두 개의 EKS 클러스터(WEB / WAS)에 application 적용
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.05.54.png]]
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_4.07.55.png]]
    

  

  

# 성공

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.44.02.png]]

- Web / Was 두 개의 클러스터를 하나의 ArgoCD에 연동하여 관리되는 모습