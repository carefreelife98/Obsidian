  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.41.05.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.40.50.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.42.12.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.45.21.png]]

- Rollback
    
    ```
    $ kubectl rollout undo deployment.apps/nginx-deploy --to-revision=2
    ```
    
      
    
      
    

## Deployment

```
구조DeployReplicaSetPod
```

  

## Service(= ClusterIP)

```
구조clusterIPNodePortLoadBalancerIngress
```

  

  

# Volume

  

## Volume의 필요성

Container를 종료하면 read-write layer가 사라진다.

- 실제로 사라지는 것은 아니지만, 컨테이너를 다시 생성하게 된다면 컨테이너 ID가 바뀌기 때문에.

  

Container Layer에 존재하는 모든 데이터는 Stateless 이다.

- 컨테이너가 종료되면 사라진다.

  

컨테이너가 종료되어도 데이터를 유지할 수 있는 방법은 **외부 디스크 볼륨을 마운트 하는 것.**

  

## Kubernetes가 지원하는 Volume

1. **EmptyDir (Docker의 Read-Write Layer과는 다른 것.)**
    
    - **Pod가 종료되면 영구적으로 삭제된다.**
        
        → Pod와 동일한 LifeCycle을 가진다.
        
    
      
    
    - **EmpthDir 의 사용 용도**
        - **대규모 파일** 기반 Sorting 작업 (디스크 용량의 추가)
        - **Pod 내의 컨테이너간 파일교환**
            
            → 통신을 통한 교환 대신 **디스크를 사용한 복사로 빠른 파일교환**
            
        - 복구를 위한 임시파일 보관 (Log등..)
2. **HostPath**
    - Pod가 종료 되어도 상태가 유지 된다.
        - 하지만, POD는 항상 다른 노드로 이동 가능성이 존재한다.
3. **Persistent Volume**
    
    Read-Write Once 의 주체는 Pod이 아닌 Node이다.
    
      
    

Volume 특징 정리

- Pod 에서 실제 데이터가 있는 디렉토리를 보존하기 위해 사용
- Pod의 일부로 정의되며, Pod와 라이프사이클을 같이한다.
- 독립적인 쿠버네티스 오브젝트가 아니며 스스로 생성하거나 삭제할 수 없다.
- ~

  

Volume의 종류

- 일반 Volume
- 특수 Volume
    - configMap
    - Secret (key manager - KMS / Vault)
        - base-64 인코딩만 가능. → 인증서 같은 것들을 다룸
        - 암호화 불가.

  

  

## Configmap

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.36.36.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.36.45.png]]

  

```
apiVersion: v1kind: Podmetadata:  name: fortune5s  labels:    name: fortune5sspec:  containers:  - name: web-server    image: nginx:alpine    resources:      limits:        memory: "128Mi"        cpu: "500m"    ports:      - containerPort: 80    volumeMounts:    - name: html      mountPath: /usr/share/nginx/html      readOnly: true # 읽기 전용 설정을 해주면 Lock을 할 필요가 없어지므로 효율성이 증가한다.  - name: html-generator    args: ["5"]    image: csm4903/fortuneloop    resources:      limits:        memory: "128Mi"        cpu: "500m"    volumeMounts:    - name: html      mountPath: /var/htdocs  volumes:  - name: html    emptyDir: {}
```

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.50.14.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.54.38.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.03.07.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.10.15.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.10.01.png]]

  

listen 80;

→ 해당 웹서버 IP의 default port.

  

listen [::]:80;

→ 모든 IP의 default port.

  

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.16.13.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.19.22.png]]

  

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.22.57.png]]

- Data 밑을 기준으로 - - - - 위가 Key 이고, 아래가 Value이다.

  

  

  

## 정리

  

eptydir

host

persistent volume

config map

  

## 11. 리소스 제어 (Resource Control)

  

Resource Request 및 Limit

- CPU 및 메모리에 대해 리소스 설정가능
- CPU 는 하이퍼쓰레딩 된 가상 코어 단위
    - 1000m = 1 Core
    - 250m = 1/4 Core

  

Pod 스케줄링 시 리소스 고려 사항

- 스케줄러는 요청사항을 합해서 미 할당된 리소스를 계산
- 실제 사용량은 여유가 있어도 아래와 같이 Pod가 할당되지 않을 수도 있음

  

## HPA (Horizontal Pod Auto-scaler)