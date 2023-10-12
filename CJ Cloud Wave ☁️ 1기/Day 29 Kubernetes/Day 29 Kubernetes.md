  

## CI/CD2

**Hostservice 작성**

  

.yaml | sed “s/image: csm4903\/node-app:.*/images: csm4903\/node-app:qwqwqwqw/g”

- sed(= Simple Edit)

  

# Persistent Volume

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.27.21.png]]

NFS(물리적인 볼륨)을 PersistentVolume(가상 볼륨)으로 추상화.

  

PersistentVolume (운영자가 확인)

PersistentVolumeClaim(Volume 요청서 객체) - 사용자(요청자)가 작성

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.34.58.png]]

Persistent Volume Reclaim Policy

- retain (유지)
    - PVC를 삭제해도 PV는 유지.
- delete(삭제)
    - 관련된 모든 볼륨 삭제
- recycle(재사용)
    - 볼륨 자체의 삭제는 아니지만 rm-rf /*로 모든 데이터만 삭제

  

AccessModes

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.35.29.png]]

  

Storage Class

- Volume 메뉴판으로 생각

  

  

## GCP K8s 클러스터 연동하기

```
$ gcloud container clusters get-credentials (GCP 클러스터 이름) --location=asia-northeast3-a
```

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.36.50.png]]

- GCP 콘솔에서 클러스터 정보 확인

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.37.55.png]]

- 연동 성공

  

  

CNI

- Container Netwok Interface

  

CSI

- Container Storage Interface

  

  

## 사용하는 DISK 종류 세 가지

1. Block Storage
    - aws : EBS
    - Google : Google Persistent Disk
    - Update 시 이전 data 삭제.
2. Object Storage
    - Modify(수정) 불가
    - 데이터 / 머신 러닝에서 필수 사용
    - 상대적으로 낮은 가격.
    - RESTful 제공
    - Http 통신 가능 (URL을 가짐)
        - HTTP Method(GET, POST..) 사용하여 데이터 관리 가능.
        - Update를 위한 PUT Method는 사용 불가. (수정 불가이므로)
    - Time Travel
        - 이전 데이터 전부 보관됨
        - 따라서 Write만 사용하면 됨.
        - 가장 최근의 데이터가 Default로 출력됨.
        - 시간 / 날짜를 지정하여 과거의 데이터도 출력 가능
    - 종류
        - Snowflake (유료)
        - trino (오픈 소스)
        - Presto (오픈 소스)
        - Detalake
3. NFS (Network File Storage)
    
    - Network 사용하여 파일 공유
        - Samba
    - 여러 명이 동시에 사용할 수 있다. (Many read Many Write)
    
      
    
      
    

**CEPH / LONGHORN**

- Disk를 연결시켜주면 사용가능 (해당 프로그램의 CSI 드라이버 설치)
    
    - Block Storage / Object Storage 로서 사용가능.
    
      
    
      
    

## 연습 문제 - PV 생성 (Nginx)

1. nginx 용 Disk 생성 (PD)
2. PV 생성
3. PVC 생성
4. POD에 PD 바로 MOUNT 하여 서비스
    - MountPoint : /user/share/nginx/html
        - nginx를 띄룬 후
        - /etc/nginx/conf.d/default.conf → 루트 찾아서 Mount
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.24.10.png]]
            
5. POD가 pvc를 이용해서 MOUNT 후 서비스.

  

  

## Storage Class

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.19.36.png]]

  

  

  

  

  

# Kubernetes 운영 시 반드시 알아야 하는 것!

## < Scheduling >

  

1. **Pod Affinity and Anti - Affinity**
    - Pod가 함께 배치될 수 있는 Pod에 대한 선호도 (또는 반선호도) 를 지정할 수 있게 해준다.
        - Application의 지연 조건, 보안 등을 위해.
        - Affinity
            - 예)
            - Spring boot와 Redis가 함께 떠있으면 좋음(Network 거치지않고 바로 DB 데이터 전달가능)
            - 이때 Redis Pod은 Spring Boot의 Pod와 친밀하기 때문에 붙어서 배치 시킬 수 있다.
    - 노드는 배치를 제어할 수 없다.
    - 노드의 레이블과 Pod의 레이블 Selector를 사용하여 Pod배치를 위한 규칙을 생성한다.
        - 규칙은 필수(required) 또는 최선의 노력(Preffered) 가 있다.
            
            ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.59.31.png]]
            
2. **Node Affinity**
    
    ```
    preferedDuringSchedulingIgnoredDuringExecution# preferedDuringScheduling : best effort. 스케줄링 간에는 최대한 배치 해줘라 ?# requiredDuringScheduling : 조건에 맞지 않으면 조건이 될 때까지 해당 Pod는 배치되지 않고 Pending(대기).#	= Selector와 거의 비슷한 기능을 한다.
    ```
    
      
    
    **Inter Pod Affinity**
    
    - TopologyKey를 기준으로 추가적인 조건을 붙여 분산 배치한다.
3. **Node Selectors**
4. **Taints and Tolerations**