# Learning Kubernetes

[toc]

## 3. 도커 기본 다지기

### 3.1 도커 설치하기

### 3.1.1 Ubuntu 설치

```
sudo apt-get updatesudo apt-get install docker.iosudo ln -sf /usr/bin/docker.io /usr/local/bin/docker
```

> 우분투 패키지 메니저는 apt-get , apt-cache, apt 가 있습니다. 모두 동일한 명령어라고 보면 되지만, apt 를 쓸경우 일단 글자수가 적고, 출력 Output 에 색상이 추가되어 좀더 예쁘게 보입니다.

### 3.1.2 CentOS 6

```
sudo yum install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpmsudo yum install docker-io
```

### 3.1.3 CentOS 7

```
sudo yum install dockersudo service docker startsudo chkconfig docker on # 부팅시에 자동 스타트업
```

### 3.1.4 Mac 및 Windows 에 설치

아래 URL 에서 다운받아 설치 하면 됩니다.

MAC 의 경우 Hyperkit, Windows 의 경우 Hyper-V 를 가상화 레이어로 사용 합니다.

Docker Desktop for Mac : https://hub.docker.com/editions/community/docker-ce-desktop-mac/

Docker Desktop for Windows : https://hub.docker.com/editions/community/docker-ce-desktop-windows/

### 3.2 도커 기본 명령어

도커 파일 내에서 사용 가능한 명령어

|   |   |   |   |
|---|---|---|---|
|명령|설명|명령|설명|
|**FROM**|베이스 이미지 지정|**VOLUME**|볼륨 마운트|
|**RUN**|명령실행|**USER**|사용자 지정|
|**CMD**|컨테이너 실행 명령|**WORKDIR**|작업 디렉토리|
|**LABEL**|라벨 설정|**ONBUILD**|Dockerfile 내의 변수|
|**EXPOSE**|포트 익스포트|**STOPSIGNAL**|빌드 완료 후 실행되는 명령|
|**ENV**|환경변수|**HEALTHCHECK**|시스템 콜 시그널 설정|
|**ADD**|파일/디렉토리 추가|**SHELL**|기본 쉘 설정|
|**CPOY**|파일 복사|||
|**ENTRYPOINT**|컨테이너 실행 명령사|||

### 3.2.1 도커 hub 사용을 위한 계정생성

```
sudo docker login
```

> docker hub (hub.docker.dom) 에 가입 후 명령어를 실행 해야 로그인이 가능 합니다.

### 3.2.2 도커 이미지 검색

search 명령으로 nginx 를 검색해 봅니다.

```
sudo docker search nginx
```

[![](https://www.notion.so./img/docker_k8s_search.png)](https://www.notion.so./img/docker_k8s_search.png)

docker_k8s_search

> sudo 를 매번 사용하지 않고 싶으면 사용자 계정을 Docker 그룹에 추가 해주면 됩니다.
> 
> ```
> sudo usermod -aG docker kubeadmin
> ```

### 3.2.3 도커 이미지 다운로드

nginx 이미지를 설치해봅니다. 최신 안정버전인 1.18.0 버전을 설치 합니다.

```
sudo docker pull nginx:latest    # 최신버전 다운로드sudo docker pull nginx:1.18.0   # 특정버전 다운로드sudo docker pull -a nginx        # 모든버전 다운로드
```

[![](https://www.notion.so./img/dcoker-pull.png)](https://www.notion.so./img/dcoker-pull.png)

dcoker-pull

### 3.2.4 도커 이미지 목록 보기

```
docker image listdocker image lsdocker images
```

[![](https://www.notion.so./img/docker-image-list.png)](https://www.notion.so./img/docker-image-list.png)

docker-image-list

### 3.2.5 도커 이미지 삭제

```
# 도커 이미지 삭제docker rmi [image_id]# 컨데이터가 존재 할경우에 함께 강제 삭제docker rmi -f [image_id]
```

### 3.3 도커 이미지 생성 하기

### 3.3.1 서비스를 위한 Application 코드 작성

hostname_finder 라는 폴더를 만들고 그 아래 main.go 및 Dockerfiles 2개 파일을 작성 합니다.

먼저 vi 또는 gedit 를 실행해서 아래 파일을 main.go 라는 이름 으로 작성 합니다.

```
package mainimport (    "fmt"    "os"    "log"    "net/http")func handler(w http.ResponseWriter, r *http.Request){    name, err := os.Hostname()    if err != nil {        panic(err)    }    fmt.Fprintln(w,"hostname:", name)}func main() {  fmt.Fprintln(os.Stdout,"Starting GoApp Server......")    http.HandleFunc("/",handler)    log.Fatal(http.ListenAndServe(":8080",nil))}
```

```
# go run main.go# go build main.go# ./main
```

### 3.3.2 이미지 생성을 위한 Dockerfile 작성

Dockerfile

```
FROM golang:1.11-alpine AS buildRUN apk update && \    apk add --no-cache bashWORKDIR /src/COPY main.go go.* /src/RUN CGO_ENABLED=0 go build -o /bin/demoENTRYPOINT ["/bin/demo"]
```

Dockerfile2

```
FROM golang:1.11-alpine AS buildRUN apk update && \    apk add --no-cache bashWORKDIR /src/COPY main.go go.* /src/RUN CGO_ENABLED=0 go build -o /bin/demoFROM scratchCOPY --from=build /bin/demo /bin/demoENTRYPOINT ["/bin/demo"]
```

Dockerfile3

```
FROM golang:1.11-alpine AS buildRUN apk update && \    apk add --no-cache bashWORKDIR /src/COPY main.go go.* /src/RUN CGO_ENABLED=0 go build -o /bin/demoFROM alpine:3.4COPY --from=build /bin/demo /bin/demoENTRYPOINT ["/bin/demo"]
```

> 서로 다른 방법으로 생성한 이미지를 비교해보세요. 어떤차이가 있나요?

### 3.3.3 컨테이너 이미지 생성

hostname_finder 폴더 안에서 아래 명령어를 실행 합니다.

```
sudo docker build -t goapp .sudo docker built -t goapp2 -f Dockerfile2
```

> . 은 현재 디렉토리서 Dockerfile 자동으로 찾아서 goapp 이라는 이미지를 생성 합니다.

[출력]

```
Sending build context to Docker daemon  3.072kBStep 1/7 : FROM golang:1.11-alpine AS build ---> e116d2efa2abStep 2/7 : WORKDIR /src/ ---> Using cachedocker ---> c3210d8eb11fStep 3/7 : COPY main.go go.* /src/ ---> ef55118ea78cStep 4/7 : RUN CGO_ENABLED=0 go build -o /bin/demo ---> Running in e557730bf11cRemoving intermediate container e557730bf11c ---> d55bd9bd3f81Step 5/7 : FROM scratch --->Step 6/7 : COPY --from=build /bin/demo /bin/demo ---> bb4b1250a05eStep 7/7 : ENTRYPOINT ["/bin/demo"] ---> Running in 4419d56988aaRemoving intermediate container 4419d56988aa ---> 36f5c919e3b8Successfully built 36f5c919e3b8Successfully tagged goapp:latest
```

이미지가 생성 되었는지 명령어를 통해 확인 합니다.

```
docker images
```

[출력]

```
REPOSITORY                           TAG                 IMAGE ID            CREATED              SIZE<none>                               <none>              d55bd9bd3f81        About a minute ago   325MBgoapp                              latest              36f5c919e3b8        About a minute ago   6.51MB<none>                               <none>              1c688e9c7e3c        3 days ago           325MB<none>                               <none>              9b60c66a5b82        3 days ago           6.51MB<none>                               <none>              7fc44021a96f        3 days ago           325MB<none>                               <none>              2caa0c2ac791        3 days ago           325MB<none>                               <none>              c46d81105b65        3 days ago           6.51MB<none>                               <none>              2d78705fb4ae        3 days ago           312MB
```

### 3.4 도커 컨테이너 시작 및 서비스 확인

### 3.4.1 도커 컨테이너 시작

```
docker run --name goapp-project -p 8080:8080 -d goappdocker run -it --name goapp-project -p 8080:8080 -d goapp /bin/bash
```

> –name : 실행한 도커 컨테이너의 이름 지정
> 
> - p : 포트 맵핑 정보 localhost 와 컨테이너 포트를 맵핑 합니다.
> - d : Docker 컨테이너를 백그라운드로 수행하고 컨테이너 ID를 출력 합니다.
> - i : STDIN 계속 interactive 모드로 유지

### 3.4.2 도커 컨테이너 서비스 확인

curl 명령어를 통해 정상적인 서비스 수행 여부를 확인 합니다.

```
curl localhost:8080
```

[출력]

```
hostname: 96fc3a5eb914
```

### 3.4.3 도커 프로세서 확인

```
docker ps
```

**[출력]**

```
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                    NAMES96fc3a5eb914        goapp                   "/bin/demo"              43 seconds ago      Up 42 seconds       0.0.0.0:8080->8080/tcp   goapp-project
```

### 3.4.4 프로세서 상세 정보 출력

```
docker inspect goapp-project
```

[출력]

```
[    {        "Id": "96fc3a5eb914c58ed83e088681d53a46188edeaab061ff2de0b9852e9dd276c9",        "Created": "2020-01-10T04:32:12.269012485Z",        "Path": "/bin/demo",        "Args": [],        "State": {            "Status": "running",            "Running": true,            "Paused": false,            "Restarting": false,            "OOMKilled": false,            "Dead": false,            "Pid": 31285,            "ExitCode": 0,            "Error": "",            "StartedAt": "2020-01-10T04:32:12.902395362Z",            "FinishedAt": "0001-01-01T00:00:00Z"        },        "Image": "sha256:36f5c919e3b88f1c991eda67d96e62fe02b763182e44f19fb78b2fc055165f3d",        "ResolvConfPath": "/var/lib/docker/containers/96fc3a5eb914c58ed83e088681d53a46188edeaab061ff2de0b9852e9dd276c9/resolv.conf",        "HostnamePath": "/var/lib/docker/containers/96fc3a5eb914c58ed83e088681d53a46188edeaab061ff2de0b9852e9dd276c9/hostname",        "HostsPath": "/var/lib/docker/containers/96fc3a5eb914c58ed83e088681d53a46188edeaab061ff2de0b9852e9dd276c9/hosts",        "LogPath": "/var/lib/docker/containers/96fc3a5eb914c58ed83e088681d53a46188edeaab061ff2de0b9852e9dd276c9/96fc3a5eb914c58ed83e088681d53a46188edeaab061ff2de0b9852e9dd276c9-json.log",
```

### 3.4.5 Docker 컨테이너 접속

```
# docker 컨테이너 접속docker exec -it goapp-project bash# docker 외부에서 컨테이너에 명령수행 (ls 명령 수행)docker exec goapp-project ls
```

> -i 또는 –interactive : STDIN을 오픈한 상태로 인터랙티브 모드상태
> 
> - t 또는 –tty : terminal 모드
> 
> container 이름이 보이지 않고 ID 만 기본적으로 보입니다. 컨테이너 이름을 출력하려면 아래 명령어를 수행 하면 됩니다.
> 
> docker ps –format “{{.Names}}”

### 3.4.6 도커 출력 포맷 조정 하기

- –format 키워드로 출력 조정하기

```
docker ps --format '{{.Names}}'docker ps --format "table {{.Names}}\t{{.ID}}"docker ps --format "table {{.Image}}\t{{.Ports}}\t{{.Names}}"
```

- 원하는 컨테이너 삭제

```
docker rm -f $(docker ps -a --format "table {{.Names}}\t{{.ID}}" | grep hostfinder | awk '{print $1}')
```

### 3.4.7 도커 인스턴스 중단 및 삭제

도커 명령어 모음 참조 : https://docs.docker.com/engine/reference/commandline/docker/

```
# 컨테이너 중단docker stop goapp-project# 컨테이너 삭제docker rm goapp-project# 컨테이너 중단 + 삭제docker rm -f goapp-project# 모든 컨테이너 삭제docker rm `docker ps -a -q`
```

### 3.5. 도커 이미지를 도커 허브에 업로드

### 3.5.1 도커 허브양식에 맞게 tag 수정하기

```
docker tag goapp  dangtong/goapp
```

[출력]

```
REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZEdangtong/goapp                       latest              12e9a84d9e23        3 days ago          6.51MBgoapp                                latest              12e9a84d9e23        3 days ago          6.51MB
```

> dangtong/firstapp 과 first-container 의 image ID 가 같은 것을 확인 할 수 있습니다.
> 
> 사실 하나의 이미지를 서로 다른 TAGID 로 공유 하는 것입니다. 디스크 공간이 늘어나지 않습니다.

### 3.5.2 도커 허브에 이미지 업로드 하기

```
docker login --username dangtongdocker push dangtong/goapp
```

[출력]

```
The push refers to repository [docker.io/dangtong/goapp]cc282a374c26: Pushedlatest: digest: sha256:b18b5ff03599893a7361feda054ebe26de61a71f019dc8725bb33d87f2115968 size: 528
```

도커 허브에 로그인 하게 되면 아래와 같이 goapp 이미지가 업로드 된것을 확인 할 수 있습니다. 이제 인터넷만 연결 되면 어디서는 자신이 만든 이미지로 컨테이너를 실행 할 수 있습니다.

[![](https://www.notion.so./img/image-20200110124234902.png)](https://www.notion.so./img/image-20200110124234902.png)

image-20200110124234902

### 3.5.3 도커 허브의 이미지로 컨테이너 실행

docker hub 에 있는 이미지를 로딩하여 컨테이너 생성

```
docker run --name goapp-project -p 8080:8080 -d dangtong/goapp
```

```
docker ps
```

[출력]

```
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                    NAMES0938068f8709        dangtong/goapp          "/bin/demo"              5 seconds ago       Up 4 seconds        0.0.0.0:8080->8080/tcp   goapp-project
```

현재 서비스에 접속하여 확인

```
curl http://localhost:8080
```

```
hostname: 0938068f8709
```

### [연습문제] 3-1 Conatinerize 실습

1. 서버 호스트명을 출력하는 node.js 프로그램을 만드세요
2. 해당 소스로 node 버전 7 기반으로 서비스 하는 Docker file을 만들고 이미지를 build 하세요
3. tag 명령을 이용해서 docker hub 에 올릴수 있도록 이름(${dockerhub username}/nodejs-app)을 바꾸세요.
4. docker login 후에 docker hub 에 업로드하여 실제 업로드가 되었는지 확인하세요

- node.js 프로그램 (app.js)

```
const http = require('http');const os = require('os');console.log("Learning Kubernetes server starting...");var handler = function(request, response) {  console.log("Received request from " + request.connection.remoteAddress);  response.writeHead(200);  response.end("You've hit " + os.hostname() + "\n");};var www = http.createServer(handler);www.listen(8080);
```

- Dcokerfile

```
# FROM 으로 BASE 이미지 로드FROM node:7# ADD 명령어로 이미지에 app.js 파일 추가ADD app.js /app.js# ENTRYPOINT 명령어로 node 를 실행하고 매개변수로 app.js 를 전달ENTRYPOINT ["node", "app.js"]
```

### [연습문제] 3-2 Conatinerize 실습

1. Dockerfile을 만들지 않고, Ubuntu 컨테이너 2개를 생성 합니다.
2. 컨테이너 이름은 my-ubuntu 로 합니다.
3. Ubuntu 환경에 터미널로 접속하세요
4. Ubuntu 환경에 ifconfig 명령을 사용하기 위해 net-tools 를 설치 하세요 (명령어로)

> docker run -it -d –name my-ubuntu ubuntu
> 
> docker exec -it my-ubuntu sh -c “apt-get update && apt install net-tools”
> 
> docker exec -it my-ubuntu ifconfig -a or docker exec -it my-ubuntu sh -c “ifconfig -a”

### 3.6 도커 명령어 둘러보기

```
docker inspectdocker inspect my-ubuntu | grep IPAddressdocker volume lsdocker network ls
```

### 3.7 도커에서 볼륨 사용하기

### 3.7.1 도커 볼륨 생성하기

```
docker volume create db-datadocker volume create --name my-volume --opt type=none --opt device=/Users/hbyun/Dropbox/05.Lecture/cloudWave/kubetpl/docker/volume/my-volume --opt o=binddocker volume ls
```

index.html (/Users/hbyun/Dropbox/05.Lecture/cloudWave/kubetpl/docker/volume/my-volume) 작성

```
Hellow World !!!
```

### 3.7.2 생성된 볼륨 사용하기

```
docker run -v my-volume:/usr/share/nginx/html -p 8080:80 --name nginx -d nginx:stable-alpine3.17-slim
```

> 브라우저로 http://localhost:8080 으로 접속해서 페이지가 표시 되는지 확인 합니다.

```
docker exec -it nginx shcd /usr/share/nginx/htmlcat index.htmlcd /etc/nginx/conf.dcat default.conf # root Document의 위치를 확인 합니다.
```

### 3.8 Docker Compose 사용하기

### 3.8.1 Docker Compose 이용해서 Mysql 서비스

```
version: '3'services:  db:    image: mysql:5.7    container_name: db    environment:      MYSQL_ROOT_PASSWORD: admin123      MYSQL_DATABASE: mysqldb      MYSQL_USER: dangtong      MYSQL_PASSWORD: admin123    ports:      - "3306:3306"    volumes:      - ~/Dropbox/dockerCompose/dockerPersistence/mysql/data:/var/lib/mysql
```

## 4. 쿠버네티스 간단하게 맛보기

### 4.1 도커 허브 이미지로 컨테이너 생성 및 확인

- 컨테이너 생성 : run/v1 으로 수행 합니다.
    
    ```
    # POD 만 생성$ kubectl run goapp-project --image=dangtong/goapp --port=8080
    ```
    
    > generator 를 run/v1 으로 수행 할 경우 내부적으로 goapp-project-{random-String} 이라는 컨테이너를 만들면서 goapp-project- 이름의 replication controller 도 생기게 됩니다.
    
    - Generator 의 종류 : **run-pod/v1** 외에 모두 deprecated 될 예정
    
    |   |   |   |
    |---|---|---|
    |Resource|API group|kubectl command|
    |Pod|v1|`kubectl run --generator=run-pod/v1`|
    |ReplicationController _(deprecated)_|v1|`kubectl run --generator=run/v1`|
    |Deployment _(deprecated)_|extensions/v1beta1|`kubectl run --generator=deployment/v1beta1`|
    |Deployment _(deprecated)_|apps/v1beta1|`kubectl run --generator=deployment/apps.v1beta1`|
    |Job _(deprecated)_|batch/v1|`kubectl run --generator=job/v1`|
    |CronJob _(deprecated)_|batch/v2alpha1|`kubectl run --generator=cronjob/v2alpha1`|
    |CronJob _(deprecated)_|batch/v1beta1|`kubectl run --generator=cronjob/v1beta1`|
    
- 컨테이너 확인
    
    ```
    $ kubectl get pods$ kubectl get rc
    ```
    
    ```
    $ kubectl get podsNAME             READY   STATUS    RESTARTS   AGEgoapp-project-bcv5q   1/1     Running   0          2m26s$ kubectl get rcNAME            DESIRED   CURRENT   READY   AGEgoapp-project   1         1         1       8m58s
    ```
    
    > 아래 명령어를 추가적으로 수행해 보세요
    > 
    > ```
    > kubectl get pods -o widekubectl describe pod goapp-project-bcv5q
    > ```
    
- k8s 서비스 생성
    
    ```
    $ kubectl expose pod goapp-project --type=LoadBalancer --name goapp-http
    ```
    
    ```
    service/firstapp-http exposed
    ```
    
- 생성한 서비스 조회
    
    ```
    $ kubectl get services
    ```
    
    ```
    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGEgoapp-http   LoadBalancer   10.96.225.172   <pending>     8080:31585/TCP   104skubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          14d
    ```
    
- 서비스 테스트 (여러번 수행)
    
    ```
    curl http://10.96.225.172:8080
    ```
    
    [출력]
    
    ```
    hostname: goapp-project-bcv5qhostname: goapp-project-bcv5qhostname: goapp-project-bcv5qhostname: goapp-project-bcv5q
    ```
    
- POD 삭제

Replication Controller 를 통해 생성된 POD 는 개별 POD 가 삭제 되지 않습니다. Replication Controller 자체를 삭제 해야 합니다.

```
kubectl delete rc goapp-project
```

## 5. PODS

### 5.1 POD 기본

```
kubectl get pod goapp-project-bcv5q -o yamlkubectl get po goapp-project-bcv5q -o json
```

크게 **metadata, spec, status** 항목으로 나누어 집니다.

[출력 - yaml]

```
apiVersion: v1kind: Podmetadata:  creationTimestamp: "2020-01-10T07:37:49Z"  generateName: goapp-project-  labels:    run: goapp-project  name: goapp-project-bcv5q  namespace: default  ownerReferences:  - apiVersion: v1    blockOwnerDeletion: true    controller: true    kind: ReplicationController    name: goapp-project    uid: e223237f-17e2-44f4-aaee-07e8ff4592b2  resourceVersion: "3082141"  selfLink: /api/v1/namespaces/default/pods/goapp-project-bcv5q  uid: 72bebcbd-8e6e-4906-9f66-1c3821fa49ecspec:  containers:  - image: dangtong/goapp    imagePullPolicy: Always    name: goapp-project    ports:    - containerPort: 8080      protocol: TCP    resources: {}    terminationMessagePath: /dev/termination-log    terminationMessagePolicy: File    volumeMounts:    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount      name: default-token-qz4fh      readOnly: true  dnsPolicy: ClusterFirst  enableServiceLinks: true  nodeName: worker01.sas.com  priority: 0  restartPolicy: Always  schedulerName: default-scheduler  securityContext: {}  serviceAccount: default  serviceAccountName: default  terminationGracePeriodSeconds: 30  tolerations:  - effect: NoExecute    key: node.kubernetes.io/not-ready    operator: Exists    tolerationSeconds: 300  - effect: NoExecute    key: node.kubernetes.io/unreachable    operator: Exists    tolerationSeconds: 300  volumes:  - name: default-token-qz4fh    secret:      defaultMode: 420      secretName: default-token-qz4fhstatus:  conditions:  - lastProbeTime: null    lastTransitionTime: "2020-01-10T07:37:49Z"    status: "True"    type: Initialized  - lastProbeTime: null    lastTransitionTime: "2020-01-10T07:37:55Z"    status: "True"    type: Ready  - lastProbeTime: null    lastTransitionTime: "2020-01-10T07:37:55Z"    status: "True"    type: ContainersReady  - lastProbeTime: null    lastTransitionTime: "2020-01-10T07:37:49Z"    status: "True"    type: PodScheduled  containerStatuses:  - containerID: docker://de3418e51fea7f7a19e7b1d6ff5a4a6f386f337578838e6f1ec0589275ca6f48    image: dangtong/goapp:latest    imageID: docker-pullable://dangtong/goapp@sha256:e5872256539152aecd2a8fb1f079e132a6a8f247c7a2295f0946ce2005e36d05    lastState: {}    name: goapp-project    ready: true    restartCount: 0    started: true    state:      running:        startedAt: "2020-01-10T07:37:55Z"  hostIP: 192.168.56.103  phase: Running  podIP: 10.40.0.2  podIPs:  - ip: 10.40.0.2  qosClass: BestEffort  startTime: "2020-01-10T07:37:49Z"
```

### 5.2 POD 생성을 위한 YAML 파일 만들기

아래와 같이 goapp.yaml 파일을 만듭니다.

```
apiVersion: v1kind: Podmetadata:  name: goapp-podspec:  containers:  - image: dangtong/goapp    name: goapp-container    ports:    - containerPort: 8080      protocol: TCP
```

> ports 정보를 yaml 파일에 기록 하지 않으면 아래 명령어로 향후에 포트를 할당해도 됩니다.
> 
> ```
> kubectl port-forward goapp-pod 8080:8080
> ```

### 5.3 YAML 파일을 이용한 POD 생성 및 확인

```
$ kubectl create -f goapp.yaml
```

[output]

```
 pod/goapp-pod created
```

```
$ kubectl get pod
```

[output]

```
NAME                  READY   STATUS    RESTARTS   AGEgoapp-pod             1/1     Running   0          12mgoapp-project-bcv5q   1/1     Running   0          41mgoapp-project-c8hml   1/1     Running   0          25mgoapp-project-r7kx5   1/1     Running   0          25m
```

### 5.4 POD 및 Container 로그 확인

- POD 로그 확인

```
kubectl logs goapp-pod
```

[output]

```
Starting GoApp Server......
```

- Container 로그 확인

```
kubectl logs goapp-pod -c goapp-container
```

[output]

```
Starting GoApp Server......
```

> 현재 1개 POD 내에 Container 가 1이기 때문에 출력 결과는 동일 합니다. POD 내의 Container 가 여러개 일 경우 모든 컨테이너의 표준 출력이 화면에 출력됩니다.

### [연습문제] 5-1 Pod 생성

아래 정보를 기반으로 POD를 생성 하세요.

|   |   |
|---|---|
|항목|내용|
|이미지|nginx:1.18.0|
|POD 이름|nginx-app|
|Port|80|

1. curl 명령어르 사용해서 nginx 서비스에 접속
2. nginx Pod 의 정보를 yaml 파일로 출력 하세요
3. nginx-app Pod 를 삭제 하세요

## 6. Lable

### 6.1 Lable 정보를 추가해서 POD 생성하기

- goapp-with-lable.yaml 이라 파일에 아래 내용을 추가 하여 작성 합니다.

```
apiVersion: v1kind: Podmetadata:  name: goapp-pod2  labels:    env: prodspec:  containers:  - image: dangtong/goapp    name: goapp-container    ports:    - containerPort: 8080      protocol: TCP
```

- yaml 파일을 이용해 pod 를 생성 합니다.

```
$ kubectl create -f ./goapp-with-lable.yaml
```

[output]

```
pod/goapp-pod2 created
```

- 생성된 POD를 조회 합니다.

```
kubectl get po --show-labels
```

[output]

```
NAME                  READY   STATUS    RESTARTS   AGE     LABELSgoapp-pod             1/1     Running   0          160m    <none>goapp-pod2            1/1     Running   0          3m53s   env=prodgoapp-project-bcv5q   1/1     Running   0          9h      run=goapp-projectgoapp-project-c8hml   1/1     Running   0          9h      run=goapp-projectgoapp-project-r7kx5   1/1     Running   0          9h      run=goapp-project
```

- Lable 태그를 출력 화면에 컬럼을 분리해서 출력

```
kubectl get pod -L env
```

[output]

```
NAME                  READY   STATUS    RESTARTS   AGE     ENVgoapp-pod             1/1     Running   0          161mgoapp-pod2            1/1     Running   0          5m19s   prodgoapp-project-bcv5q   1/1     Running   0          9hgoapp-project-c8hml   1/1     Running   0          9hgoapp-project-r7kx5   1/1     Running   0          9h
```

- Lable을 이용한 필터링 조회

```
kubectl get pod -l env=prod
```

[output]

```
NAME         READY   STATUS    RESTARTS   AGEgoapp-pod2   1/1     Running   0          39h
```

- Label 추가 하기

```
kubectl label pod goapp-pod2 app="application" tier="frondEnd"
```

- Label 삭제 하기

```
kubectl label pod goapp-pod2 app- tier-
```

### 6.2 Label 셀렉터 사용

- AND 연산

```
kubectl get po -l 'app in (application), tier in (frontEnd)'
```

- OR 연산

```
kubectl get po -l 'app in (application,backEnd)'kubectl get po -l 'app in (application,frontEnd)'
```

### 6.3 생성된 POD 로 부터 yaml 파일 얻기

```
kubectl get pod goapp-pod -o yaml
```

[output]

```
apiVersion: v1kind: Podmetadata:  creationTimestamp: "2020-01-10T08:07:04Z"  name: goapp-pod  namespace: default  resourceVersion: "3086366"  selfLink: /api/v1/namespaces/default/pods/goapp-pod  uid: 18cf0ed0-be56-4b54-869c-4473117800b1spec:  containers:  - image: dangtong/goapp    imagePullPolicy: Always    name: goapp-container    ports:    - containerPort: 8080      protocol: TCP    resources: {}    terminationMessagePath: /dev/termination-log    terminationMessagePolicy: File    volumeMounts:    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount      name: default-token-qz4fh      readOnly: true  dnsPolicy: ClusterFirst  enableServiceLinks: true  nodeName: worker02.sas.com  priority: 0  restartPolicy: Always  schedulerName: default-scheduler  securityContext: {}  serviceAccount: default  serviceAccountName: default  terminationGracePeriodSeconds: 30  tolerations:  - effect: NoExecute    key: node.kubernetes.io/not-ready    operator: Exists    tolerationSeconds: 300  - effect: NoExecute    key: node.kubernetes.io/unreachable    operator: Exists    tolerationSeconds: 300  volumes:  - name: default-token-qz4fh    secret:      defaultMode: 420      secretName: default-token-qz4fhstatus:  conditions:  - lastProbeTime: null    lastTransitionTime: "2020-01-10T08:07:04Z"    status: "True"    type: Initialized  - lastProbeTime: null    lastTransitionTime: "2020-01-10T08:07:09Z"    status: "True"    type: Ready  - lastProbeTime: null    lastTransitionTime: "2020-01-10T08:07:09Z"    status: "True"    type: ContainersReady  - lastProbeTime: null    lastTransitionTime: "2020-01-10T08:07:04Z"    status: "True"    type: PodScheduled  containerStatuses:  - containerID: docker://d76af359c556c60d3ac1957d7498513f42ace14998c763456190274a3e4a1d5e    image: dangtong/goapp:latest    imageID: docker-pullable://dangtong/goapp@sha256:e5872256539152aecd2a8fb1f079e132a6a8f247c7a2295f0946ce2005e36d05    lastState: {}    name: goapp-container    ready: true    restartCount: 0    started: true    state:      running:        startedAt: "2020-01-10T08:07:08Z"  hostIP: 10.0.2.5  phase: Running  podIP: 10.32.0.4  podIPs:  - ip: 10.32.0.4  qosClass: BestEffort  startTime: "2020-01-10T08:07:04Z"
```

### 6.4 Lable을 이용한 POD 스케줄링

- 노드목록 조회

```
kubectl get nodes
```

[output]

```
NAME               STATUS   ROLES    AGE   VERSIONmaster.sas.com     Ready    master   16d   v1.17.0worker01.sas.com   Ready    <none>   16d   v1.17.0worker02.sas.com   Ready    <none>   16d   v1.17.0
```

- 특정 노드에 레이블 부여

```
kubectl label node worker02.sas.com memsize=high
```

- 레이블 조회 필터 사용하여 조회

```
kubectl get nodes -l memsize=high
```

[output]

```
NAME               STATUS   ROLES    AGE   VERSIONworker02.sas.com   Ready    <none>   17d   v1.17.0
```

- 특정 노드에 신규 POD 스케줄링
    
    아래 내용과 같이 goapp-label-node.yaml 파을을 작성 합니다.
    

```
apiVersion: v1kind: Podmetadata:  name: goapp-pod-memhighspec:  nodeSelector:    memsize: "high"  containers:  - image: dangtong/goapp    name: goapp-container-memhigh
```

- YAML 파일을 이용한 POD 스케줄링

```
kubectl create -f ./goapp-lable-node.yaml
```

[output]

```
pod/goapp-pod-memhigh created
```

- 생성된 노드 조회

```
kubectl get pod -o wide
```

[output]

```
NAME                  READY   STATUS    RESTARTS   AGE    IP          NODE               NOMINATED NODE   READINESS GATESgoapp-pod-memhigh     1/1     Running   0          17s    10.32.0.5   worker02.sas.com   <none>           <none>
```

## 7. Annotation

### 7.1 POD 에 Annotation 추가하기

```
kubectl annotate pod goapp-pod-memhigh maker="dangtong" team="k8s-team"
```

[output]

```
pod/goapp-pod-memhigh annotated
```

### 7.2 Annotation 확인하기

- YAML 파일을 통해 확인하기

```
kubectl get po goapp-pod-memhigh -o yaml
```

[output]

```
kind: Podmetadata:  annotations:    maker: dangtong    team: k8s-team  creationTimestamp: "2020-01-12T15:25:05Z"  name: goapp-pod-memhigh  namespace: default  resourceVersion: "3562877"  selfLink: /api/v1/namespaces/default/pods/goapp-pod-memhigh  uid: a12c35d7-d0e6-4c01-b607-cccd267e39ecspec:  containers:
```

- DESCRIBE 를 통해 확인하기

```
kubectl describe pod goapp-pod-memhigh
```

[output]

```
Name:         goapp-pod-memhighNamespace:    defaultPriority:     0Node:         worker02.sas.com/10.0.2.5Start Time:   Mon, 13 Jan 2020 00:25:05 +0900Labels:       <none>Annotations:  maker: dangtong              team: k8s-teamStatus:       RunningIP:           10.32.0.5
```

### 7.3 Annotation 삭제

```
kubectl annotate pod  goapp-pod-memhigh maker- team-
```

### [Exercise \#4]

- bitnami 라는 네임스페이스를 생성 하세요
- 이후 리소스는 BITNAMI 네임스페이스에 생성하세
- bitnami/apache 이미지로 Pod 를 만들고 tier=FronEnd, app=apache 라벨 정보를 포함하세요
- Pod 정보를 출력 할때 라벨을 함께 출력 하세요
- app=apache 라벨틀 가진 Pod 만 조회 하세요
- 만들어진 Pod에 env=dev 라는 라벨 정보를 추가 하세요
- created_by=kevin 이라는 Annotation을 추가 하세요
- apache Pod를 삭제 하세요

```
apiVersion: v1kind: Podmetadata:  name: goapp-pod-memhighspec:  nodeSelector:    memsize: "high"  containers:  - image: dangtong/goapp    name: goapp-container-memhigh
```

## 8. Namespace

### 8.1 네임스페이스 조회

```
kubectl get namespace
```

> kubectl get ns 와 동일함

[output]

```
NAME              STATUS   AGEdefault           Active   17dkube-node-lease   Active   17dkube-public       Active   17dkube-system       Active   17d
```

### 8.2 특정 네임스페이스의 POD 조회

```
kubectl get pod --namespace kube-system# kubectl get po -n kube-system
```

> kubectl get pod -n kube-system 과 동일함

[output]

```
coredns-6955765f44-glcdc                 1/1     Running   0          17dcoredns-6955765f44-h7fbb                 1/1     Running   0          17detcd-master.sas.com                      1/1     Running   1          17dkube-apiserver-master.sas.com            1/1     Running   1          17dkube-controller-manager-master.sas.com   1/1     Running   1          17dkube-proxy-gm44f                         1/1     Running   1          17dkube-proxy-ngqr6                         1/1     Running   0          17dkube-proxy-wmq7d                         1/1     Running   0          17dkube-scheduler-master.sas.com            1/1     Running   1          17dweave-net-2pm2x                          2/2     Running   0          17dweave-net-4wksv                          2/2     Running   0          17dweave-net-7j7mn                          2/2     Running   0          17d
```

### 8.3 YAML 파일을 이용한 네임스페이스 생성

- YAML 파일 작성 : first-namespace.yaml 이름으로 파일 작성

```
apiVersion: v1kind: Namespacemetadata:  name: first-namespace
```

- YAML 파일을 이용한 네이스페이스 생성

```
kubectl create -f first-namespace.yaml
```

[output]

```
namespace/first-namespace created
```

- 생성된 네임스페이스 확인

```
kubectl get namespacekubectl get ns
```

[output]

```
NAME              STATUS   AGEdefault           Active   17dfirst-namespace   Active   5skube-node-lease   Active   17dkube-public       Active   17dkube-system       Active   17d
```

> kubectl create namespace first-namespace 와 동일 합니다.

### 8.4 특정 네임스페이스에 POD 생성

- first-namespace 에 goapp 생성

```
kubectl create -f goapp.yaml -n first-namespace
```

[output]

```
pod/goapp-pod created
```

- 생성된 POD 확인하기

```
kubectl get pod -n first-namespace
```

[output]

```
NAME        READY   STATUS    RESTARTS   AGEgoapp-pod   1/1     Running   0          12h
```

### 8.5 POD 삭제

```
'kubectl' delete pod goapp-pod-memhigh
```

```
kubectl delete pod goapp-pod
```

```
kubectl delete pod goapp-pod -n first-namespace
```

> 현재 네임스페이스 에서 존재 하는 모든 리소스를 삭제하는 명령은 아래와 같습니다.
> 
> kubectl delete all –all
> 
> 현재 네임스페이스를 설정하고 조회 하는 명령은 아래와 같습니다.
> 
> ```
> # 네임스페이스 설정kubectl config set-context --current --namespace=<insert-namespace-name-here># 확인kubectl config view --minify | grep namespace:
> ```

### [Exercise \#5]

1. 쿠버네티스 클러스터에 몇개의 네임스페이가 존재 하나요?
2. my-dev 라는 네임스페이를 생성하고 nginx Pod를 배포 하세요

## 9. kubectl 기본 사용법

### 9.1 단축형 키워드 사용하기

```
kubectl get po          # PODskubectl get svc         # Servicekubectl get rc          # Replication Controllerkubectl get deploy  # Deploymentkubectl get ns          # Namespacekubectl get no          # Nodekubectl get cm          # Configmapkubectl get pv          # PersistentVolumns
```

### 9.2 도움말 보기

```
kubectl -h
```

```
kubectl controls the Kubernetes cluster manager. Find more information at: https://kubernetes.io/docs/reference/kubectl/overview/Basic Commands (Beginner):  create         Create a resource from a file or from stdin.  expose         Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service  run            Run a particular image on the cluster  set            Set specific features on objectsBasic Commands (Intermediate):  explain        Documentation of resources  get            Display one or many resources  edit           Edit a resource on the server  delete         Delete resources by filenames, stdin, resources and names, or by resources and label selectorDeploy Commands:
```

```
kubectl get -h
```

```
Display one or many resources Prints a table of the most important information about the specified resources. You can filter the list using a labelselector and the --selector flag. If the desired resource type is namespaced you will only see results in your currentnamespace unless you pass --all-namespaces. Uninitialized objects are not shown unless --include-uninitialized is passed. By specifying the output as 'template' and providing a Go template as the value of the --template flag, you can filterthe attributes of the fetched resources.Use "kubectl api-resources" for a complete list of supported resources.Examples:  # List all pods in ps output format.  kubectl get pods  # List all pods in ps output format with more information (such as node name).  kubectl get pods -o wide
```

### 9.3 리소스 정의에 대한 도움말

```
kubectl explain pods
```

```
KIND:     PodVERSION:  v1DESCRIPTION:     Pod is a collection of containers that can run on a host. This resource is     created by clients and scheduled onto hosts.FIELDS:   apiVersion   <string>     APIVersion defines the versioned schema of this representation of an     object. Servers should convert recognized schemas to the latest internal     value, and may reject unrecognized values. More info:     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md\#resources   kind <string>     Kind is a string value representing the REST resource this object     represents. Servers may infer this from the endpoint the client submits     requests to. Cannot be updated. In CamelCase. More info:     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md\#types-kinds   metadata <Object>     Standard object's metadata. More info:     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md\#metadata   spec <Object>     Specification of the desired behavior of the pod. More info:     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md\#spec-and-status   status   <Object>     Most recently observed status of the pod. This data may not be up to date.     Populated by the system. Read-only. More info:     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
```

### 9.4 리소스 감시하기

- Kube-system 네임스페이스에 있는 모든 pod에 대해 모니터링 합니다.

```
kubectl get pods --watch -n kube-system
```

```
root@master:~# k get pods --watch -n kube-systemNAME                                     READY   STATUS    RESTARTS   AGEcoredns-6955765f44-glcdc                 1/1     Running   0          19dcoredns-6955765f44-h7fbb                 1/1     Running   0          19detcd-master.sas.com                      1/1     Running   1          19dkube-apiserver-master.sas.com            1/1     Running   1          19dkube-controller-manager-master.sas.com   1/1     Running   1          19dkube-proxy-gm44f                         1/1     Running   1          19dkube-proxy-ngqr6                         1/1     Running   0          19dkube-proxy-wmq7d                         1/1     Running   0          19dkube-scheduler-master.sas.com            1/1     Running   1          19dweave-net-2pm2x                          2/2     Running   0          19dweave-net-4wksv                          2/2     Running   0          19dweave-net-7j7mn                          2/2     Running   0          19d...
```

### 9.5 리소스 비교하기

```
kubectl diff -f goapp.yaml
```

### 9.6 kubectx 및 kubens 사용하기

현재 컨텍스트 및 네임스페이스를 확인하고 전환 할때 손쉽게 사용 할수 있는 도구

### 9.6.1 kubectx 및 kubens 설치

```
git clone https://github.com/ahmetb/kubectx.git ~/.kubectxCOMPDIR=$(pkg-config --variable=completionsdir bash-completion)ln -sf ~/.kubectx/completion/kubens.bash $COMPDIR/kubensln -sf ~/.kubectx/completion/kubectx.bash $COMPDIR/kubectxcat << FOE >> ~/.bashrcexport PATH=~/.kubectx:\$PATHFOE
```

### 9.6.2 kubectx 및 kubens 사용

- kubectx 사용

```
kubectxkubectx <변경하고 싶은 컨텍스트 이름>
```

- kubens 사용

현재 사용중인 네임스페이스를 조회 합니다. 현재 사용중인 네이스페이스는 하이라이트 됩니다.

```
kubens
```

```
defaultfirst-namespacekube-node-leasekube-publickube-system
```

Kube-system 네이스페이스로 전환 해봅니다.

```
kubens kube-system
```

```
Context "kubernetes-admin@kubernetes" modified.Active namespace is "kube-system".
```

pod 조회 명령을 내리면 아래와 같이 kube-system 네임스페이스의 pod 들이 조회 됩니다.

```
kubectl get po
```

```
NAME                                     READY   STATUS    RESTARTS   AGEcoredns-6955765f44-glcdc                 1/1     Running   0          34dcoredns-6955765f44-h7fbb                 1/1     Running   0          34detcd-master.sas.com                      1/1     Running   1          34dkube-apiserver-master.sas.com            1/1     Running   1          34dkube-controller-manager-master.sas.com   1/1     Running   1          34dkube-proxy-gm44f                         1/1     Running   1          34dkube-proxy-ngqr6                         1/1     Running   0          34dkube-proxy-wmq7d                         1/1     Running   0          34dkube-scheduler-master.sas.com            1/1     Running   1          34dweave-net-2pm2x                          2/2     Running   0          34dweave-net-4wksv                          2/2     Running   0          34dweave-net-7j7mn                          2/2     Running   0          34d
```

### 9.7 kubernetes 컨텍스트 및 네임스페이스 표시하기

kube-ps1을 다운로드 하여 /usr/local/kube-ps1 설치 하고 .bashrc 파일에 아래와 같이 설정 합니다. [링크](https://github.com/jonmosco/kube-ps1)

```
source /usr/local/kube-ps1/kube-ps1.shPS1='[\u@\h \W $(kube_ps1)]\$ '
```

```
[root@master ~ (⎈ |kubernetes-admin@kubernetes:kube-public)]#
```

### 9.8 kubectl Context 추가 하기

. kube 디렉토리의 config 에 virtualBox 에

### 9.8.1 Context 조회

```
kubectl config get-contexts
```

### 9.8.2 Context 추가

로컬 VM 의 control plane 의 $HOME/.kube/config 파일에서 다음 3가지 항목을 복사해서 로컬 디렉토에 각각 파일을 만듭니다.

- K8s 클러스터의 config 파일 내 인증 정보를 로컬의 아래 위치의 파일로 각각 복사 합니다.

|   |   |
|---|---|
|항목|로컬 파일|
|certificate-authority-data|$HOME/.kube/kmaster/ca-origin|
|client-certificate-data|$HOME/.kube/kmaster/cli-cert-origin|
|client-key-data|$HOME/.kube/kmaster/cli-key-origin|

- 내용이 BASE64 로 인코딩 되어 있기 때문에 각각의 파일을 아래와 같이 디코딩 해줍니다.
- Mac 및 Linux 에서는 아래와 같이 수행 합니다.

```
cat $HOME/.kube/kmaster/ca-origin | base64 -D > $HOME/.kube/kmaster/cacat $HOME/.kube/kmaster/cli-cert-origin | base64 -D > $HOME/.kube/kmaster/cli-certcat $HOME/.kube/kmaster/cli-key-origin | base64 -D > $HOME/.kube/kmaster/cli-key
```

- Window 에서는 아래와 같이 수행 합니다.

```
certutil -decode ca-origin cacertutil -decode cli-cert-origin cli-certcertutil -decode cli-key-origin cli-key# type config | findstr name
```

이제부터는 로컬 머신에서 아래와 같이 3단계로 kubectl 명령어를 사용해 kubernetes 클러스터를 추가 해줍니다.

- 1단계 클러스터 추가 (set-cluster)

> kubectl config –kubeconfig=[config-file-name] set-cluster [cluster-name] –server=[api-server-url] –certificate-authority=[ca-certificate]

```
kubectl config --kubeconfig=config set-cluster local-k8s --server=https://192.168.56.111:6443 --certificate-authority=/Users/dangtongbyun/.kube/kmaster/ca
```

- 2단계 접속 정보 추가 (set-credentials)

> kubectl config –kubeconfig=[config-file-name] set-credentials [cluster-name] –client-certificate=[client-cert-file] –client-key=[client-key-file]

```
kubectl config --kubeconfig=config set-credentials kubernetes-admin --client-certificate=/Users/dangtongbyun/.kube/kmaster/cli-cert --client-key=/Users/dangtongbyun/.kube/kmaster/cli-key
```

- 3단계 Context 추가 (set-context)

> kubectl config –kubeconfig=[config-file-name] set-context [context-name] –cluster=[cluster-name] –namespace=[namepsace-name] –user=[username]

```
kubectl config --kubeconfig=config set-context kubernetes-admin@local-k8s --cluster=local-k8s  --user=kubernetes-admin
```

### 9.8.3 Context 변경

```
kubectl config get-contexts# get-contexts 결과에서 혹인후 아래 명령어로 현재 클러스터 변경kubectl config use-context <Context-Name>
```

### 9.8.4 Context 삭제

```
kubectl config delete-cluster [cluster-name]kubectl config delete-context [context-name]kubectl config delete-user [user-name]orkubectl config unset users.[cluster-name]kubectl config unset contexts.[context-name]kubectl config unset clusters.[context-name]
```

> google cloud 에서는 cluster-name, context-name, user-name. 이 모두 동일함

### 9.8.5 alias 만들기

- Linux 및 MAC

```
alias chks='kubectl config use-context'alias lsks='kubectl config get-contexts'
```

- Windows

```
Set-Alias chks 'kubectl config use-context'Set-Alias lsks 'kubectl config get-contexts'
```

## 10. Liveness probes

liveness prove는 Pod에 지정된 주소에 Health Check 를 수행하고 실패할 경우 Pod를 다시 시작 합니다.

이때 중요한 점은 단순히 다시 시작만 하는 것이 아니라, 리포지토리로 부터 이미지를 다시 받아 Pod 를 다시 시작 합니다.

아래 내용으로.

```
apiVersion: v1kind: Podmetadata:  labels:    test: liveness  name: liveness-httpspec:  containers:  - name: liveness    image: k8s.gcr.io/liveness    args:    - /server    livenessProbe:      httpGet:        path: /healthz        port: 8080        httpHeaders:        - name: Custom-Header          value: Awesome      initialDelaySeconds: 3      periodSeconds: 3
```

K8s.gcr.io/liveness 이미지는 liveness 테스트를 위해 만들어진 이미지 입니다. Go 언어로 작성 되었으며, 처음 10초 동안은 정상적인 서비스를 하지만, 10초 후에는 에러를 발생 시킵니다. 자세한 사항은 [URL](https://github.com/kubernetes/kubernetes/blob/master/test/images/agnhost/liveness/server.go) 을 참고 하세요

### 10.1 Pod 생성

```
kubectl create -f ./liveness-probe-pod.yaml
```

### 10.2 Pod 확인

```
kubectl get pod
```

아래

```
NAME            READY   STATUS    RESTARTS   AGEliveness-http   1/1     Running   0          5sNAME            READY   STATUS    RESTARTS   AGEliveness-http   1/1     Running   1          26sNAME            READY   STATUS    RESTARTS   AGEliveness-http   1/1     Running   3          68sNAME            READY   STATUS             RESTARTS   AGEliveness-http   0/1     CrashLoopBackOff   3          81sNAME            READY   STATUS             RESTARTS   AGEliveness-http   0/1     CrashLoopBackOff   5          2m50s
```

### 10.3 Pod 로그 이벤트 확인

```
kubectl describe pod liveness-http
```

```
Name:         liveness-httpNamespace:    defaultPriority:     0Node:         worker02.acorn.com/192.168.56.110Start Time:   Wed, 01 Apr 2020 05:54:29 +0000Labels:       test=livenessAnnotations:  <none>Status:       RunningIP:           10.36.0.1IPs:  IP:  10.36.0.1Containers:  liveness:    Container ID:  docker://0f1ba830b830d5879fe99776cd0db5f3678bf52a11e3ccb1a1e9c65460957817    Image:         k8s.gcr.io/liveness    Image ID:      docker-pullable://k8s.gcr.io/liveness@sha256:1aef943db82cf1370d0504a51061fb082b4d351171b304ad194f6297c0bb726a    Port:          <none>    Host Port:     <none>    Args:      /server    State:          Running      Started:      Wed, 01 Apr 2020 06:01:15 +0000    Last State:     Terminated      Reason:       Error      Exit Code:    2      Started:      Wed, 01 Apr 2020 05:58:16 +0000      Finished:     Wed, 01 Apr 2020 05:58:32 +0000    Ready:          True    Restart Count:  7    Liveness:       http-get http://:8080/healthz delay=3s timeout=1s period=3s \#success=1 \#failure=3    Environment:    <none>    Mounts:      /var/run/secrets/kubernetes.io/serviceaccount from default-token-zshgs (ro)Conditions:  Type              Status  Initialized       True  Ready             True  ContainersReady   True  PodScheduled      TrueVolumes:  default-token-zshgs:    Type:        Secret (a volume populated by a Secret)    SecretName:  default-token-zshgs    Optional:    falseQoS Class:       BestEffortNode-Selectors:  <none>Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s                 node.kubernetes.io/unreachable:NoExecute for 300sEvents:  Type     Reason     Age                    From                         Message  ----     ------     ----                   ----                         -------  Normal   Scheduled  <unknown>              default-scheduler            Successfully assigned default/liveness-http to worker02.acorn.com  Normal   Pulled     6m14s (x3 over 6m53s)  kubelet, worker02.acorn.com  Successfully pulled image "k8s.gcr.io/liveness"  Normal   Created    6m14s (x3 over 6m52s)  kubelet, worker02.acorn.com  Created container liveness  Normal   Started    6m14s (x3 over 6m52s)  kubelet, worker02.acorn.com  Started container liveness  Normal   Pulling    5m55s (x4 over 6m54s)  kubelet, worker02.acorn.com  Pulling image "k8s.gcr.io/liveness"  Warning  Unhealthy  5m55s (x9 over 6m40s)  kubelet, worker02.acorn.com  Liveness probe failed: HTTP probe failed with statuscode: 500  Normal   Killing    5m55s (x3 over 6m34s)  kubelet, worker02.acorn.com  Container liveness failed liveness probe, will be restarted  Warning  BackOff    108s (x17 over 5m36s)  kubelet, worker02.acorn.com  Back-off restarting failed container
```

로그이벤트를 보면 Liveness Probe 가 실패해서 컨테이너를 재가동 하는 메시지가 보입니다.

뿐만아니라, 재가동 시에서 Pull image 를 통해 이미지를 다시 가져 와서 재가동 시키는 것을 볼 수 있습니다.

## 11. Replication Controller

### 11.1 Replication Controller 생성

아래와 같이 template를 작성합니다.

```
apiVersion: v1kind: ReplicationControllermetadata:  name: goapp-rcspec:  replicas: 3  selector:    app: goapp  template:    metadata:      name: goapp-pod      labels:        tier: forntend        app: goapp        env: prod        priority:  high    spec:      containers:      - name: goapp-container        image: dangtong/goapp        ports:        - containerPort: 8080
```

### 11.2 Pod 생성

```
kubectl create -f ./rc-goapp.yaml
```

### 11.3 Pod 생성 확인

```
kubectl get poNAME             READY   STATUS    RESTARTS   AGEgoapp-rc-9q689   1/1     Running   0          39sgoapp-rc-d5rnf   1/1     Running   0          39sgoapp-rc-fm7kr   1/1     Running   0          39s
```

### 11.4 Replication Controller 확인

```
kubectl get rcNAME       DESIRED   CURRENT   READY   AGEgoapp-rc   3         3         3       58skubectl get rc -o wideNAME       DESIRED   CURRENT   READY   AGE   CONTAINERS        IMAGES           SELECTORgoapp-rc   3         3         3       72s   goapp-container   dangtong/goapp   app=goapp
```

### 11.5 특정 Pod 삭제하고 변화 확인하기

아래와 같이 3개의 Pod 중에 하나를 선택해서 삭제 합니다.

```
kubectl delete pod goapp-rc-9q689
```

```
kubectl get podNAME             READY   STATUS                         RESTARTS   AGEgoapp-rc-d5rnf   1/1     Running                        0          2mgoapp-rc-fm7kr   1/1     Running                        0          2mgoapp-rc-szv2r   1/1     ContainerCreating      0          6s
```

기존 컨테이너를 Terminating 하고 새로운 컨테이너를 생성하는 것을 볼 수 있습니다.

### 11.6 Pod 정보를 라벨과 함께 출력해보기

```
kubectl get pod --show-labelsNAME             READY   STATUS    RESTARTS   AGE     LABELSgoapp-rc-d5rnf   1/1     Running   0          7m26s   app=goappgoapp-rc-fm7kr   1/1     Running   0          7m26s   app=goappgoapp-rc-szv2r   1/1     Running   0          4m51s   app=goapp
```

### 11.7 Pod 라벨을 변경해보기

기존 “app=nginx” 라는 label 을 “app=goapp-exit” 로 변경 합니다.

```
kubectl label pod goapp-rc-szv2r app=goapp-exit --overwrite
```

아래와 같이 pod 를 조회 해봅니다.

```
kubectl get poNAME             READY   STATUS              RESTARTS   AGEgoapp-rc-d5rnf   1/1     Running             0          8m49sgoapp-rc-fm7kr   1/1     Running             0          8m49sgoapp-rc-mmn2b   0/1     ContainerCreating   0          5sgoapp-rc-szv2r   1/1     Running             0          6m14s
```

기존 3개의 Pod 중 하나의 Label을 변경하면 기존 app=goapp 에는 2개의 Pod 만 남기 때문에 Replication Controller 는 **추가적으로 하나의 Pod 를 생성** 합니다.

```
NAME             READY   STATUS    RESTARTS   AGE     LABELSgoapp-rc-d5rnf   1/1     Running   0          9m27s   app=goappgoapp-rc-fm7kr   1/1     Running   0          9m27s   app=goappgoapp-rc-mmn2b   1/1     Running   0          43s     app=goappgoapp-rc-szv2r   1/1     Running   0          6m52s   app=goapp-exit
```

### 11.8 Pod Template 변경 해보기

아래와 같이 Pod Template의 spec ➢ spec ➢ containers ➢ image 항목을 dangtong/goapp-v2 로 변경 합니다.

```
kubectl edit rc nginx
```

```
# Please edit the object below. Lines beginning with a '#' will be ignored,# and an empty file will abort the edit. If an error occurs while saving this file will be# reopened with the relevant failures.\#apiVersion: v1kind: ReplicationControllermetadata:  creationTimestamp: "2020-04-01T09:32:23Z"  generation: 1  labels:    app: goapp  name: goapp-rc  namespace: default  resourceVersion: "405444"  selfLink: /api/v1/namespaces/default/replicationcontrollers/goapp-rc  uid: 17198300-d964-4de6-a160-825a7a9c16bfspec:  replicas: 3  selector:    app: goapp  template:    metadata:      creationTimestamp: null      labels:        app: goapp      name: goapp-pod    spec:      containers:      - image: dangtong/goapp-v2 # 이부분을 변경 합닏다.(기존 : dangtong/goapp)        imagePullPolicy: Always        name: goapp-container        ports:        - containerPort: 80          protocol: TCP        resources: {}        terminationMessagePath: /dev/termination-log        terminationMessagePolicy: File      dnsPolicy: ClusterFirst      restartPolicy: Always      schedulerName: default-scheduler      securityContext: {}      terminationGracePeriodSeconds: 30status:  availableReplicas: 3  fullyLabeledReplicas: 3  observedGeneration: 1  readyReplicas: 3  replicas: 3
```

저장후 편집기를 종료 합니다.

> 리눅스 편집기에는 다양한 종유가 있습니다. 만약 기본 편집기를 변경하고 싶으면
> 
> KUBE_EDITOR=“/bin/nano” 를 $HOME/.bashrc 파일에 선언 해주면 원하는 편집기를 사용 할수 있습니다.

### 11.9 Pod Template 를 적용하기 위해 임의의 Pod 삭제하기

```
kubectl get podgoapp-rc-mvw57   1/1     Running   0          3h6m    10.36.0.2   worker02.acorn.comgoapp-rc-qkrpw   1/1     Running   0          6m26s   10.32.0.2   worker01.acorn.comgoapp-rc-x6q4d   1/1     Running   0          3h6m    10.36.0.1   worker02.acorn.com
```

Pod 삭제

```
kubectl delete pod goapp-rc-mvw57pod "goapp-rc-mvw57" deleted
```

Pod 확인

```
kubectl get podNAME             READY   STATUS              RESTARTS   AGEgoapp-rc-bf2xk   0/1     ContainerCreating   0          8sgoapp-rc-qkrpw   1/1     Running             0          7m5sgoapp-rc-x6q4d   1/1     Running             0          3h7mkubectl get pod -o wideNAME             READY   STATUS    RESTARTS   AGE     IP          NODE                 NOMINATED NODE   READINESS GATESgoapp-rc-bf2xk   1/1     Running   0          21s     10.32.0.3   worker01.acorn.comgoapp-rc-qkrpw   1/1     Running   0          7m18s   10.32.0.2   worker01.acorn.comgoapp-rc-x6q4d   1/1     Running   0          3h7m    10.36.0.1   worker02.acorn.com
```

접속 해보기

```
# dangtong/goappcurl http://10.32.0.2:8080hostname: goapp-rc-qkrpw# dangtong/goapp-v3curl http://10.32.0.3:8080hostname: goapp-rc-bf2xkversion: goapp-v2
```

### 11.10 Pod 스케일링

- Template 변경을 통한 스케일링
    
    아래와 같이 goapp-rc 를 edit 명령으로 수정 합니다. (replicas 항목을 3에서 4로 수정)
    

```
kubectl edit rc goapp-rc
```

```
# Please edit the object below. Lines beginning with a '#' will be ignored,# and an empty file will abort the edit. If an error occurs while saving this file will be# reopened with the relevant failures.\#apiVersion: v1kind: ReplicationControllermetadata:  creationTimestamp: "2020-04-01T09:51:49Z"  generation: 3  labels:    app: goapp  name: goapp-rc  namespace: default  resourceVersion: "416408"  selfLink: /api/v1/namespaces/default/replicationcontrollers/goapp-rc  uid: 23f58f51-88ab-4828-9a76-cde8a646fff4spec:  replicas: 4  # 이부분을 변경 합니다. (기존 : 3)  selector:    app: goapp  template:    metadata:      creationTimestamp: null      labels:        app: goapp      name: goapp-pod    spec:      containers:      - image: dangtong/goapp-v2        imagePullPolicy: Always        name: goapp-container        ports:
```

저장 한다음 Pod 및 RC 확인

```
kubectl get podNAME             READY   STATUS              RESTARTS   AGEgoapp-rc-bf2xk   1/1     Running             0          19mgoapp-rc-mr6kb   0/1     ContainerCreating   0          7sgoapp-rc-qkrpw   1/1     Running             0          26mgoapp-rc-x6q4d   1/1     Running             0          3h26m
```

```
kubectl get rcNAME       DESIRED   CURRENT   READY   AGEgoapp-rc   4         4         4       4h17m
```

- 명령어를 통한 스케일링

명령어를 이용해서 스케일링을 수행 할 수 있습니다.

```
kubectl scale rc goapp-rc --replicas=5
```

실제로 Pod 가 늘어 났는지 확인해봅니다.

```
kubectl get podNAME             READY   STATUS              RESTARTS   AGEgoapp-rc-bf2xk   1/1     Running             0          72mgoapp-rc-dlgfc   0/1     ContainerCreating   0          4sgoapp-rc-mr6kb   1/1     Running             0          53mgoapp-rc-qkrpw   1/1     Running             0          79mgoapp-rc-x6q4d   1/1     Running             0          4h19m
```

### 11.11 Replication Controller 삭제

Replication Controller 와 POD 모두 삭제

```
kubectl delete rc goapp-rc
```

Replication Controller 만 삭제. POD 는 그대로 유지 합니다.

```
kubectl delete rc goapp-rc --cascade=orphan
```

## 12.ReplicaSet

### 12.1 RS 생성

Selector 를 작성 할때 **ForntEnd** 이고 **운영계** 이면서 중요도가 **High** 인 POD 에 대해 RS 를 생성 합니다.

```
apiVersion: apps/v1kind: ReplicaSetmetadata:  name: frontend  labels:    app: guestbook    tier: frontendspec:  replicas: 3  selector:    matchLabels:      tier: frontend    matchExpressions:      - {key: env, operator: In, values: [prod]}      - {key: priority, operator: NotIn, values: [low]}  template:    metadata:      labels:        tier: frontend        env: prod        priority: high    spec:      containers:      - name: php-redis        image: gcr.io/google_samples/gb-frontend:v3
```

### 12.2 RS 확인

```
$ kubectl get pod -o wideNAME             READY   STATUS    RESTARTS   AGE   IP          NODEfrontend-bstms   1/1     Running   0          53s   10.32.2.7   gke-gke1-default-pool-ad44d907-cq8jfrontend-d4znb   1/1     Running   0          53s   10.32.2.9   gke-gke1-default-pool-ad44d907-cq8jfrontend-rv9bl   1/1     Running   0          53s   10.32.2.8   gke-gke1-default-pool-ad44d907-cq8j
```

```
$ kubectl get rs -o wideAME             READY   STATUS    RESTARTS   AGE   IP          NODEfrontend-bstms   1/1     Running   0          68s   10.32.2.7   gke-gke1-default-pool-ad44d907-cq8jfrontend-d4znb   1/1     Running   0          68s   10.32.2.9   gke-gke1-default-pool-ad44d907-cq8jfrontend-rv9bl   1/1     Running   0          68s   10.32.2.8   gke-gke1-default-pool-ad44d907-cq8j
```

```
$ kubectl get pod --show-labelsNAME             READY   STATUS    RESTARTS   AGE    LABELSfrontend-bstms   1/1     Running   0          107s   env=prod,priority=high,tier=frontendfrontend-d4znb   1/1     Running   0          107s   env=prod,priority=high,tier=frontendfrontend-rv9bl   1/1     Running   0          107s   env=prod,priority=high,tier=frontend
```

### [Exercise \#5]

1. Nginx:1.18 Pod 3개로 구성된 Replication Controller를 작성 하세요
2. Replication Controller 만 삭제 하세요(Pod 는 유지)
3. 남겨진 Nginx Pod를 관리하는 ReplicaSet 을 작성하된 replica 4개로 구성 하시요
4. Nginx Pod 를 6개로 Scale Out 하세요

## 13.DeamonSet

### 13.1 데몬셋 생성

goapp-ds.yaml 이라는 이름으로 아래 파일을 작성 합니다.

```
apiVersion: apps/v1kind: DaemonSetmetadata:  name: goapp-on-ssdspec:  selector:    matchLabels:      app: goapp-pod  template:    metadata:      labels:        app: goapp-pod    spec:      nodeSelector:        disk: ssd      containers:      - name: goapp-container        image: dangtong/goapp
```

데몬셋을 생성 합니다.

```
$ kubectl create -f ./goapp-ds.yaml
```

Pod 와 데몬셋을 조회 합니다.

```
$ kubectl get pod$ kubectl get dsNAME           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGEgoapp-on-ssd   0         0         0       0            0           disk=ssd        <nome>
```

조회 하면 Pod 도 존재하지 않고 데몬셋 정보를 조회 해도 모두 0 으로 나옵닏다. 노드에 disk=ssd 라벨이 없기 때문입니다.

이제 라벨을 추가 합니다.

```
$ kubectl label node worker01.acorn.com disk=ssd$ kubectl get podNAME                 READY   STATUS    RESTARTS   AGEgoapp-on-ssd-vwvks   1/1     Running   0          7s$ kubectl label node worker02.acorn.com disk=ssd$ kubectl get podNAME                 READY   STATUS    RESTARTS   AGEgoapp-on-ssd-nbnwz   1/1     Running   0          7sgoapp-on-ssd-vwvks   1/1     Running   0          36s$ kubectl get ds -o wideAME           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGEgoapp-on-ssd   2         2         2       2            2           disk=ssd        10m
```

## 14.Deployment

### 14.1 Deployment 생성

아래와 같이 nginx 를 서비스 하고 replica 가 3개인 Deployment 를 작성합니다.(nginx-deploy.yaml)

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: nginx-deployment  labels:    app: nginxspec:  replicas: 4  strategy:    type: RollingUpdate    rollingUpdate:      maxSurge: 1      maxUnavailable: 0  selector:    matchLabels:      app: nginx  template:    metadata:      labels:        app: nginx    spec:      containers:      - name: nginx        image: nginx:1.7.9        ports:        - containerPort: 80
```

```
kubectl apply -f ./nginx-deploy.yamlkubectl annotate deploy nginx-deploy kubernetes.io/change-cause="migrate to nginx:1.7.9"
```

### 14.2 Deployment 확인

```
kubectl get pod,rs,deployNAME                                    READY   STATUS    RESTARTS   AGEpod/nginx-deployment-54f57cf6bf-dpsn4   1/1     Running   0          30spod/nginx-deployment-54f57cf6bf-ghfwm   1/1     Running   0          30spod/nginx-deployment-54f57cf6bf-rswwk   1/1     Running   0          30sNAME                                          DESIRED   CURRENT   READY   AGEreplicaset.apps/nginx-deployment-54f57cf6bf   3         3         3       30sNAME                               READY   UP-TO-DATE   AVAILABLE   AGEdeployment.apps/nginx-deployment   3/3     3            3           30s
```

### 14.3 이미지 업데이트

- 기본 방식

```
kubectl --record deployment.apps/nginx-deployment set image deployment.v1.apps/nginx-deployment nginx=nginx:1.9.1
```

- 단축 방식

```
kubectl set image deployment.apps/nginx-dp nginx=nginx:1.9.1 --record# nginx 는 container 이름임
```

- yaml 파일 수정(.spec.template.spec.containers[0].image 변경)

```
kubectl edit deployment.apps/nginx-deployment
```

- change cause 남기기

```
kubectl annotate deploy nginx-deploy kubernetes.io/change-cause="migrate to nginx:1.9.1"
```

- rollout History 조회

```
kubectl rollout history deployment.apps/nginx-deployment
```

- rollback 수행

```
kubectl rollout undo deployment.apps/nginx-dp --to-revision=1
```

### 14.4 Deployment ScaleOut

```
kubectl edit deploy goapp-deploymentkubectl scale deploy nginx-deploymentgoapp-deployment-5857594fbb-2hhnv   1/1     Running   0          32mgoapp-deployment-5857594fbb-6s9lx   1/1     Running   0          6sgoapp-deployment-5857594fbb-7nqsg   1/1     Running   0          32mgoapp-deployment-5857594fbb-9b28s   1/1     Running   0          32m
```

### 14.4 deployment 확인 하기

- 롤아웃 확인

```
kubectl rollout status deployment.v1.apps/nginx-deployment
```

- deployment 상세 확인

```
kubectl describe deploy nginx-deployment
```

## 5. 서비스 (Service)

### 5.0 Telepresense

### 5.0.1 Telepresense CLI 로컬 설치

- Mac

```
brew install datawire/blackbird/telepresence
```

- Windows

```
# 1. 다운로드curl -fL https://app.getambassador.io/download/tel2/windows/amd64/latest/telepresence.zip -o telepresence.zip# 2. 압축해지# 3. 명령어 수행Set-ExecutionPolicy Bypass -Scope Process .\install-telepresence.ps1# 4. 원본 압축파일 지우고, PowerShell 다시 열기
```

### 5.0.2 Telepresense 서버를 쿠버네티스 클러스터에 설치

```
# Telepresence namespace 생성kubectl create namespace telepresense# Telepresense 서버를 쿠버네티스에 설치telepresence helm install -n telepresense# 설치 확인kubectl get all -n telepresence
```

### 5.0.3 연결 하고 상태 확인하기

```
# Telepresense 프록시 연결telepresence connect  --manager-namespace telepresence# 연결 상태 확인telepresence status
```

### 5.1 ClusterIP

### 5.1.0 nodes app 생성

```
const http = require('http');const os = require('os');console.log("Kubia server starting...");var handler = function(request, response) {  console.log("Received request from " + request.connection.remoteAddress);  response.writeHead(200);  response.end("You've hit " + os.hostname() + "\n");};var www = http.createServer(handler);www.listen(8080);
```

```
# FROM 으로 BASE 이미지 로드FROM node:7# ADD 명령어로 이미지에 app.js 파일 추가ADD app.js /app.js# ENTRYPOINT 명령어로 node 를 실행하고 매개변수로 app.js 를 전달ENTRYPOINT ["node", "app.js"]
```

### 5.1.1. pod 생성

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: nodeapp-deployspec:  replicas: 3  selector:    matchLabels:      app: nodeapp-pod  template:    metadata:      labels:        app: nodeapp-pod    spec:      containers:      - name: nodeapp-container        image: dangtong/nodeapp        ports:        - containerPort: 8080
```

### 5.1.2 yaml을 통한 ClusterIP 생성

```
apiVersion: v1kind: Servicemetadata:  name: nodeapp-servicespec:  ports:  - port: 80    targetPort: 8080  selector:    app: nodeapp-pod
```

### 5.1.3 서비스 상태 확인

```
kubectl get  po,deploy,svcNAME                                      READY   STATUS    RESTARTS   AGEpod/nodeapp-deployment-55688d9d4b-8pzsk   1/1     Running   0          2m45spod/nodeapp-deployment-55688d9d4b-pslvb   1/1     Running   0          2m46spod/nodeapp-deployment-55688d9d4b-whbk8   1/1     Running   0          2m46sNAME                                 READY   UP-TO-DATE   AVAILABLE   AGEdeployment.apps/nodeapp-deployment   3/3     3            3           2m46sNAME                      TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)        AGEnodeapp-service   ClusterIP      10.101.249.42    <none>           80/TCP         78s
```

### 5.1.4 서비스 확인

- local VirtualBox

```
curl http://10.101.249.42  #여러번 수행 하기You've hit nodeapp-deployment-55688d9d4b-8pzsk
```

- GCP Cloud

먼저 Pod 를 조회 합니다.

```
kubectl get po, svcNAME                              READY   STATUS    RESTARTS   AGEnodeapp-deploy-6dc7c5dd68-lh26q   1/1     Running   0          116mnodeapp-deploy-6dc7c5dd68-r78cj   1/1     Running   0          116mnodeapp-deploy-6dc7c5dd68-wcm7d   1/1     Running   0          116mNAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGEkubernetes         ClusterIP   10.116.0.1      <none>        443/TCP        32hnodeapp-nodeport   NodePort    10.116.11.242   <none>        80:30123/TCP   121m
```

조회된 Pod 중 하나에 exec 옵션을 사용해 sh 로 접속 합니다.

```
kubectl exec nodeapp-deploy-6dc7c5dd68-lh26q -- sh
```

curl 을 설치 하고 Cluster IP 로 접속합니다.

```
apt-get install curlcurl http://10.116.11.242
```

### 5.1.5 원격 Pod에서 curl 명령 수행하기

```
kubectl exec nodeapp-deployment-55688d9d4b-8pzsk -- curl -s http://10.101.249.42You've hit nodeapp-deployment-55688d9d4b-whbk8
```

> .더블 대시는 kubectl 명령의의 종료를 가르킴

### 5.1.6 서비스 삭제

```
kubectl delete svc nodeapp-service
```

### 5.2 NodePort

### 5.2.1 yaml 을 이용한 NodePort 생성

```
apiVersion: v1kind: Servicemetadata:  name: node-nodeportspec:  type: NodePort  ports:  - port: 80    targetPort: 8080    nodePort: 30123  selector:    app:  nodeapp-pod
```

### 5.2.2 NodePort 조회

```
kubectl get po,rs,svcNAME                                      READY   STATUS    RESTARTS   AGEpod/nodeapp-deployment-55688d9d4b-8pzsk   1/1     Running   0          145mpod/nodeapp-deployment-55688d9d4b-pslvb   1/1     Running   0          145mpod/nodeapp-deployment-55688d9d4b-whbk8   1/1     Running   0          145mNAME                                            DESIRED   CURRENT   READY   AGEreplicaset.apps/nodeapp-deployment-55688d9d4b   3         3         3       145mNAME                       TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGEservice/kubernetes         ClusterIP   10.96.0.1      <none>        443/TCP        10dservice/nodeapp-nodeport   NodePort    10.108.30.68   <none>        80:30123/TCP   4m14s
```

### 5.2.3 NodePort 를 통한 서비스 접속 확인(여러번 수행)

- Vmware VM

```
$ curl http://localhost:30123You've hit nodeapp-deployment-55688d9d4b-pslvb$ curl http://localhost:30123You've hit nodeapp-deployment-55688d9d4b-whbk8$ curl http://localhost:30123You've hit nodeapp-deployment-55688d9d4b-pslvb
```

- GCP Cloud

```
kubectl get no -o wide # 결과에서 External-IP 를 참조NAME                                      STATUS   ROLES    AGE   VERSION             INTERNAL-IP   EXTERNAL-IP     OS-IMAGE                             KERNEL-VERSION   CONTAINER-RUNTIMEgke-istiok8s-default-pool-36a6222b-33vv   Ready    <none>   32h   v1.18.12-gke.1210   10.146.0.21   35.221.70.145   Container-Optimized OS from Google   5.4.49+          docker://19.3.9gke-istiok8s-default-pool-36a6222b-6rhk   Ready    <none>   32h   v1.18.12-gke.1210   10.146.0.20   35.221.82.6     Container-Optimized OS from Google   5.4.49+          docker://19.3.9gke-istiok8s-default-pool-36a6222b-xrzj   Ready    <none>   32h   v1.18.12-gke.1210   10.146.0.22   34.84.27.67     Container-Optimized OS from Google   5.4.49+          docker://19.3.9
```

```
$ curl http://35.221.70.145:30123You've hit nodeapp-deploy-6dc7c5dd68-wcm7d$ curl http://35.221.70.145:30123You've hit nodeapp-deploy-6dc7c5dd68-lh26q$ curl http://35.221.70.145:30123You've hit nodeapp-deploy-6dc7c5dd68-r78cj
```

### 5.2.4 NodePort 삭제

```
kubectl delete svc nodeapp-nodeport
```

### 5.3 LoadBalancer (GCP 에서 수행)

### 5.3.1 yaml 파일로 deployment 생성

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: nodeapp-deployment  labels:    app: nodeappspec:  replicas: 3  selector:    matchLabels:      app: nodeapp-pod  template:    metadata:      labels:        app: nodeapp-pod    spec:      containers:      - name: nodeapp-container        image: dangtong/nodeapp        ports:        - containerPort: 8080
```

### 5.3.2 서비스 확인

```
$ kubectl get po,rs,deployNAME                                      READY   STATUS    RESTARTS   AGEpod/nodeapp-deployment-7d58f5d487-7hphx   1/1     Running   0          20mpod/nodeapp-deployment-7d58f5d487-d74rp   1/1     Running   0          20mpod/nodeapp-deployment-7d58f5d487-r8hq8   1/1     Running   0          20mNAME                                                  DESIRED   CURRENT   READY   AGEreplicaset.extensions/nodeapp-deployment-7d58f5d487   3         3         3       20mNAME                                       READY   UP-TO-DATE   AVAILABLE   AGEdeployment.extensions/nodeapp-deployment   3/3     3            3           20m
```

```
kubectl get po -o wideNAME                                  READY   STATUS    RESTARTS   AGE   IP           NODE                                  NOMINATED NODE   READINESS GATESnodeapp-deployment-7d58f5d487-7hphx   1/1     Running   0          21m   10.32.2.10   gke-gke1-default-pool-ad44d907-cq8jnodeapp-deployment-7d58f5d487-d74rp   1/1     Running   0          21m   10.32.2.12   gke-gke1-default-pool-ad44d907-cq8jnodeapp-deployment-7d58f5d487-r8hq8   1/1     Running   0          21m   10.32.2.11   gke-gke1-default-pool-ad44d907-cq8j
```

### 5.3.2 nodeapp 접속 해보기

```
$ kubectl exec nodeapp-deployment-7d58f5d487-7hphx -- curl -s http://10.32.2.10:8080또는$ kubectl exec -it nodeapp-deployment-7d58f5d487-7hphx bash$ curl http://10.32.2.10:8080You've hit nodeapp-deployment-7d58f5d487-7hphx
```

### 5.3.3 yaml 파일을 이용해 LoadBalancer 생성

```
apiVersion: v1kind: Servicemetadata:  name:  nodeapp-lbspec:  type: LoadBalancer  ports:  - port: 80    targetPort: 8080  selector:    app: nodeapp-pod
```

### 5.3.5 LoadBalancer 생성 확인

```
kubectl get svcNAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGEkubernetes   ClusterIP      10.36.0.1      <none>        443/TCP        7d21hnodeapp-lb   LoadBalancer   10.36.14.234   <pending>     80:31237/TCP   33s
```

현재 pending 상태임 20초 정도 지나면

```
kubectl get svcNAME         TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGEkubernetes   ClusterIP      10.36.0.1      <none>           443/TCP        7d21hnodeapp-lb   LoadBalancer   10.36.14.234   35.221.179.171   80:31237/TCP   45s
```

### 5.3.6 서비스 확인

```
curl http://35.221.179.171You've hit nodeapp-deployment-7d58f5d487-r8hq8
```

### 5.4 Ingress (GCP 에서 수행)

### 5.4.1 Deployment 생성

- nginx / goapp deployment 생성

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: nginx-deployment  labels:    app: nginxspec:  replicas: 3  selector:    matchLabels:      app: nginx  template:    metadata:      labels:        app: nginx    spec:      containers:      - name: nginx-container        image: nginx:1.7.9        ports:        - containerPort: 80---apiVersion: apps/v1kind: Deploymentmetadata:  name: goapp-deployment  labels:    app: goappspec:  replicas: 3  selector:    matchLabels:      app: goapp  template:    metadata:      labels:        app: goapp    spec:      containers:      - name: goapp-container        image: dangtong/goapp        ports:        - containerPort: 8080
```

### 5.4.2 Service 생성

```
apiVersion: v1kind: Servicemetadata:  name:  nginx-lbspec:  type: LoadBalancer  ports:  - port: 80    targetPort: 80  selector:    app: nginx---apiVersion: v1kind: Servicemetadata:  name:  goapp-lbspec:  type: LoadBalancer  ports:  - port: 80    targetPort: 8080  selector:    app: goapp
```

### 5.4.3 Ingress 생성

```
apiVersion: networking.k8s.io/v1kind: Ingressmetadata:  name: nginx-goapp-ingressspec:  rules:  - host: nginx.acorn.com    http:      paths:      - path: /        pathType: Prefix        backend:          service:            name: nginx-lb            port:              number: 80  - host: goapp.acorn.com    http:      paths:      - path: /        pathType: Prefix        backend:          service:            name: goapp-lb            port:              number: 80
```

### 5.4.4 Ingress 조회

```
kubectl get ingressNAME                  HOSTS                             ADDRESS   PORTS     AGEnginx-goapp-ingress   nginx.acorn.com,goapp.acorn.com             80, 443   15s
```

> Ingress 가 완전히 생성되기 까지 시간이 걸립니다. 2~5분 소요

다시 조회 합니다

```
kubectl get ingressNAME                  HOSTS                             ADDRESS          PORTS     AGEnginx-goapp-ingress   nginx.acorn.com,goapp.acorn.com   35.227.227.127   80, 443   13m
```

### 5.4.5 /etc/hosts 파일 수정

```
sudo vi /etc/hosts35.227.227.127 nginx.acorn.com goapp.acorn.com
```

### 5.4.6 서비스 확인

```
$ curl http://goapp.acorn.comhostname: goapp-deployment-d7564689f-6rrzw$ curl http://nginx.acorn.com<!DOCTYPE html><html><head><title>Welcome to nginx!</title><style>    body {        width: 35em;        margin: 0 auto;        font-family: Tahoma, Verdana, Arial, sans-serif;    }</style></head><body>
```

### 5.4.7 HTTPS 서비스 (TLS OffLoad)

- 인증서 생성 및 인증서 secret 등록

```
openssl genrsa -out server.key 2048openssl req -new -x509 -key server.key -out server.cert -days 360 -subj /CN=nginx.acorn.comkubectl create  secret tls acorn-secret --cert=server.cert --key=server.key
```

- 테스트

```
$ curl -k https://nginx.acorn.com$ curl -k https://goapp.acorn.com
```

### 5.5 Headless Service

### 5.5.1 Headless Service 생성

## 7.Volums

### 7.1 EmptyDir (GCP 에서 수행)

### 7.1.1 Docker 이미지 만들기

아래와 같이 폴더를 만들고 ./fortune/docimg 폴더로 이동합니다.

```
$ mkdir -p ./fortune/docimg$ mkdir -p ./fortune/kubetmp
```

아래와 같이 docker 이미지를 작성하기 위해 bash 로 Application을 작성 합니다.

파일명 : fortuneloop.sh

```
#!/bin/bashtrap "exit" SIGINTmkdir /var/htdocswhile :do    echo $(date) Writing fortune to /var/htdocs/index.html    /usr/games/fortune  > /var/htdocs/index.html    sleep 10done
```

Dockerfile 을 작성 합니다.

```
FROM ubuntu:latestRUN apt-get update; apt-get -y install fortuneADD fortuneloop.sh /bin/fortuneloop.shRUN chmod 755 /bin/fortuneloop.shENTRYPOINT /bin/fortuneloop.sh
```

Dcoker 이미지를 만듭니다.

```
$ docker build -t dangtong/fortune .
```

Docker 이미지를 Docker Hub 에 push 합니다.

```
$ docker login$ docker push dangtong/fortune
```

### 7.1.2 Deployment 작성

fortune APP을 적용하기 위해 Deployment 를 작성 합니다.

```
cd ../ktmp/vi fortune-deploy.yaml
```

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: fortune-deployment  labels:    app: fortunespec:  replicas: 3  selector:    matchLabels:      app: fortune  template:    metadata:      labels:        app: fortune    spec:      containers:      - image: dangtong/fortune        name: html-generator        volumeMounts:        - name: html          mountPath: /var/htdocs      - image: nginx:alpine        name: web-server        volumeMounts:        - name: html          mountPath: /usr/share/nginx/html          readOnly: true        ports:          - containerPort: 80            protocol: TCP      volumes:      - name: html        emptyDir: {}
```

> html 볼륨을 html-generator 및 web-seerver 컨테이너에 모두 마운트 하였습니다.
> 
> html 볼륨에는 /var/htdocs 및 /usr/share/nginx/html 이름 으로 서로 따른 컨테이너에서 바라 보게 됩니다.
> 
> 다만, web-server 컨테이너는 읽기 전용(reeadOnly) 으로만 접근 하도록 설정 하였습니다.

> emptDir 을 디스크가 아닌 메모리에 생성 할 수도 있으며, 이를 위해서는 아래와 같이 설정을 바꾸어 주면 됩니다.
> 
> emptyDir:
> 
> medium: Memory

### 7.1.3 LoadBalancer 작성

```
vi fortune-lb.yaml
```

```
apiVersion: v1kind: Servicemetadata:  name: fortune-lbspec:  selector:    app: fortune  ports:    - port: 80      targetPort: 80  type: LoadBalancer  externalIPs:  - 192.168.56.108
```

### 7.1.4 Deployment 및 Loadbalancer 생성

```
$ kubectl apply -f ./fortune-deploy.yaml$ kubectl apply -f ./fortune-lb.yaml
```

### 7.1.5. 서비스 확인

```
curl http://192.168.56.108
```

### 7.2 Git EmptyDir

### 7.2.1 웹서비스용 Git 리포지토리 생성

Appendix3 . Git 계정 생성 및 Sync 참조

### 7.2.2 Deployment 용 yaml 파일 작성

```
$ cd ./gitvolume/kubetmp$ vi gitvolume-deploy.yaml
```

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: gitvolume-deployment  labels:    app: nginxspec:  replicas: 3  selector:    matchLabels:      app: nginx  template:    metadata:      labels:        app: nginx    spec:      containers:      - image: nginx:alpine        name: web-server        volumeMounts:        - name: html          mountPath: /usr/share/nginx/html          readOnly: true        ports:          - containerPort: 80            protocol: TCP      volumes:      - name: html        gitRepo:          repository: https://github.com/dangtong76/k8s-web.git          revision: master          directory: .
```

### 7.2.3 Deployment 생성

```
$ kubectl apply -f ./gitvolume-deploy.yaml
```

### 7.2.4 Service 생성

```
apiVersion: v1kind: Servicemetadata:  name: gitvolume-lbspec:  selector:    app: nginx  ports:    - port: 80      targetPort: 80  type: LoadBalancer
```

### 7.3 Persisteent DISK 사용하기

### 7.3.1. Persistent DISK 생성

- 리전/존 확인

```
# GCP$ gcloud container clusters list# AWS
```

- Disk 생성(GCP)

```
# GCP$ gcloud compute disks create --size=16GiB --zone asia-northeast1-b  mongodb## 삭제## gcloud compute disks delete mongodb --zone asia-northeast1-b# AWS$ aws ec2 create-volume --volume-type gp2 --size 80 --availability-zone ap-northeast-2a## 삭제## aws ec2 delete-volume --volume-id vol-049df61146c4d7901## 조회## aws ec2 describe-volumes --filters Name=status,Values=available Name=availability-zone,Values=ap-northeast-2a
```

### 7.3.2 Pod 생성을 위한 yaml 파일 작성

AWS 에서는 POD에 직접적으로 디스크를 마운트 하는것을 허용하지 않습니다.

- 파일명 : gce-pv.yaml

```
# GCPapiVersion: v1kind: Podmetadata:  name: mongodbspec:  volumes:  - name: mongodb-data    gcePersistentDisk:      pdName: mongodb      fsType: ext4  containers:  - image: mongo    name:  mongodb    volumeMounts:    -  name: mongodb-data       mountPath: /data/db    ports:    - containerPort: 27017      protocol: TCP
```

- Pod 생성

```
$ kubectl apply -f ./gce-pv.yaml$ kubectl get poNAME      READY   STATUS    RESTARTS   AGEmongodb   1/1     Running   0          8m42s
```

- Disk 확인

```
$ kubectl describe pod mongodb...(중략)Volumes:  mongodb-data:    Type:       GCEPersistentDisk (a Persistent Disk resource in Google Compute Engine)    PDName:     mongodb  # 디스크이름    FSType:     ext4    Partition:  0    ReadOnly:   false  default-token-dgkd5:    Type:        Secret (a volume populated by a Secret)    SecretName:  default-token-dgkd5    Optional:    false...(중략)
```

### 7.3.3 Mongodb 접속 및 데이터 Insert

- 접속

```
kubectl exec -it mongodb mongosh
```

- 데이터 Insert

```
> use mystore> db.foo.insert({"first-name" : "dangtong"})> db.foo.find(){ "_id" : ObjectId("5f9c4127caf2e6464e18331c"), "first-name" : "dangtong" }> exit
```

### 7.3.4 MongoDB Pod 재시작

- MongoDB 중단

```
$ kubectl delete pod mongodb
```

- MongoDB Pod 재생성

```
$ kubectl apply -f ./gce-pv.yaml
```

- 기존에 Insert 한 데이터 확인

```
$ kubectl exec -it mongodb mongo> use mystore> db.foo.find(){ "_id" : ObjectId("5e9684134384860bc207b1f9"), "first-name" : "dangtong" }
```

### 7.3.5 Pod 삭제

```
$ kubectl delete po mongodb
```

### 7.4 PersistentVolume 및 PersistentVolumeClaim

### 7.4.1 PersistentVolume 생성

- gce-pv2.yaml 로 작성

```
# GCPapiVersion: v1kind: PersistentVolumemetadata:   name: mongodb-pvspec:  capacity:    storage: 1Gi  accessModes:  - ReadWriteOnce  - ReadOnlyMany  persistentVolumeReclaimPolicy: Retain  gcePersistentDisk:   pdName: mongodb   fsType: ext4---# AWSapiVersion: v1kind: PersistentVolumemetadata:   name: mongodb-pvspec:  capacity:    storage: 1Gi  csi:    driver: ebs.csi.aws.com    fsType: ext4    volumeHandle: vol-xxxxxxxxxxxxx  accessModes:  - ReadWriteOnce  - ReadOnlyMany  persistentVolumeReclaimPolicy: Retain  nodeAffinity:    required:      nodeSelectorTerms:        - matchExpressions:            - key: topology.ebs.csi.aws.com/zone              operator: In              values:                - ap-northeast-2a
```

```
kubectl apply -f ./gce-pv2.yaml
```

```
kubectl get pv
```

### 7.4.2 PersistentVolumeClaim 생성

gce-pvc.yaml 로 작성

```
apiVersion: v1kind: PersistentVolumeClaimmetadata:  name: mongodb-pvcspec:  resources:    requests:      storage: 1Gi  accessModes:  - ReadWriteOnce  storageClassName: ""
```

```
kubectl apply -f ./gce-pvc.yaml
```

```
kubectl get pvc
```

### 7.4.3 PV, PVC 를 이용한 Pod 생성

gce-pod.yaml 파일 생성

```
apiVersion: v1kind: Podmetadata:  name: mongodbspec:  containers:  - image: mongo    name: mongodb    volumeMounts:    - name: mongodb-data      mountPath: /data/db    ports:    - containerPort: 27017      protocol: TCP  volumes:  - name: mongodb-data    persistentVolumeClaim:      claimName: mongodb-pvc
```

```
$ kubectl apply -f ./gce-pod.yaml
```

```
$ kubectl get po,pv,pvc
```

### 7.4.4 Mongodb 접속 및 데이터 확인

```
$ kubectl exec -it mongodb -- mongo> use mystore> db.foo.find()
```

### 7.5 Persistent Volume 의 동적 할당

### 7.5.1 StorageClass 를 이용해 스토리지 유형 정의

- 클라우드에서 제공하는 Default Storage Class 확인 해보기

```
kubectl get scNAME                 PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGEfast                 kubernetes.io/gce-pd    Delete          Immediate              false                  3m37spremium-rwo          pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   true                   10dstandard (default)   kubernetes.io/gce-pd    Delete          Immediate              true                   10dstandard-rwo         pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   true                   10d
```

- 상세 내역 확인

```
kubectl describe sc standardName:                  standardIsDefaultClass:        YesAnnotations:           storageclass.kubernetes.io/is-default-class=trueProvisioner:           kubernetes.io/gce-pdParameters:            type=pd-standard # 일반 디스크AllowVolumeExpansion:  TrueMountOptions:          <none>ReclaimPolicy:         DeleteVolumeBindingMode:     ImmediateEvents:                <none>kubectl describe sc premium-rwoName:                  premium-rwoIsDefaultClass:        NoAnnotations:           components.gke.io/component-name=pdcsi-addon,components.gke.io/component-version=0.8.7Provisioner:           pd.csi.storage.gke.ioParameters:            type=pd-ssd  # SSDAllowVolumeExpansion:  TrueMountOptions:          <none>ReclaimPolicy:         DeleteVolumeBindingMode:     WaitForFirstConsumer
```

- Storage Class 생성하기 위한 Zone 확인

```
gcloud container clusters listNAME      LOCATION           MASTER_VERSION    MASTER_IP       MACHINE_TYPE  NODE_VERSION      NUM_NODES  STATUSistiok8s  asia-northeast1-a  1.18.12-gke.1210  35.189.139.222  e2-medium     1.18.12-gke.1210  3          RUNNING
```

- GCP 지원 디스크 종류 확인 하기

```
gcloud compute disk-types list | grep asia-northeast1-alocal-ssd    asia-northeast1-a          375GB-375GBpd-balanced  asia-northeast1-a          10GB-65536GBpd-ssd       asia-northeast1-a          10GB-65536GBpd-standard  asia-northeast1-a          10GB-65536GB
```

- Stroage Class 생성 (파일명 : sc.yaml)

```
apiVersion: storage.k8s.io/v1kind: StorageClassmetadata:  name: fastprovisioner: kubernetes.io/gce-pdparameters:  type: pd-ssd  zone: asia-northeast1-a  #클러스터를 만든 지역으로 설정 해야함
```

```
$ kubectl apply -f ./gce-sclass.yaml
```

### 7.5.2 Storage Class 이용한 PVC 생성

- gce-pvc-sclass.yaml

```
apiVersion: v1kind: PersistentVolumeClaimmetadata:   name: mongodb-pvcspec:  storageClassName: fast  resources:    requests:      storage: 100Mi  accessModes:    - ReadWriteOnce
```

```
$ kubectl apply -f ./gce-pvc-sclass.yaml
```

### 7.5.3 PV 및 PVC 확인

- pvc 확인

```
$ kubectl get pvc mongdb-pvc
```

- pv 확인

```
$ kubectl get pv
```

### 7.5.4 PVC 를 이용한 POD 생성

파일명 : gce-pod.yaml 파일 생성

```
apiVersion: v1kind: Podmetadata:  name: mongodbspec:  containers:  - image: mongo    name: mongodb    volumeMounts:    - name: mongodb-data      mountPath: /data/db    ports:    - containerPort: 27017      protocol: TCP  volumes:  - name: mongodb-data    persistentVolumeClaim:      claimName: mongodb-pvc
```

## 8. ConfigMap

### 8.1 도커에서 매개변수 전달

### 8.1.1 디렉토리 생성 및 소스 코드 작성

```
mkdir -p ./configmap/docimg/indockermkdir -p ./configmap/kubetmpcd ./configmap/docimg/indockervi fortuneloop.sh
```

```
#!/bin/bashtrap "exit" SIGINTINTERVAL=$1 #파라메터로 첫번째 매개 변수를 INTERVAL 로 저장echo "Configured to generate neew fortune every " $INTERVAL " seconds"mkdir /var/htdocswhile :do    echo $(date) Writing fortune to /var/htdocs/index.html    /usr/games/fortune  > /var/htdocs/index.html    sleep $INTERVALdone
```

```
chmod 755 fortuneloop.sh
```

### 8.1.2 Docker 이미지 생성

> Docker CMD 는 3가지 형태로 사용 가능 합니다.
> 
> - CMD [ “실행파일” , “파라메터1” , “파라메터2” ] → **실행파일 형태**
> - CMD [ “파라메터1” , “파라메터2”] → **ENTRYPOINT 의 디펄트 파라메터**
> - CMD command 파라메터1 파라매터2 → **쉘 명령어 형태**

```
$ vi dockerfile
```

```
FROM ubuntu:latestRUN apt-get update;  apt-get -y install fortuneADD fortuneloop.sh /bin/fortuneloop.shRUN chmod 755 /bin/fortuneloop.shENTRYPOINT ["/bin/fortuneloop.sh"]CMD ["10"]  # args가 없으면 10초
```

```
$ docker build -t dangtong/fortune:args .$ docker push dangtong/fortune:args
```

> 테스트를 위해 다음과 같이 수행 가능
> 
> $ docker run -it dangtong/fortune:args # 기본 10초 수행
> 
> $ docker rm -f [docker-id]
> 
> $ docker run -it dangtong/fortune:args 15 # 15초로 매개변수 전달
> 
> $ docker rm -f [docker-id]

### 8.1.3 Pod생성

파일명 : config-fortune-pod.yaml

```
cd ../../kubetmpvi config-fortune-indocker-pod.yaml
```

```
apiVersion: v1kind: Podmetadata:  name: fortune5sspec:  containers:  - image: dangtong/fortune:args    args: ["5"]    name: html-generator    volumeMounts:    - name: html      mountPath: /var/htdocs  - image: nginx:alpine    name: web-server    volumeMounts:    - name: html      mountPath: /usr/share/nginx/html      readOnly: true    ports:    - containerPort: 80      protocol: TCP  volumes:  - name: html    emptyDir: {}
```

```
kubectl apply -f ./config-fortune-indockeer-pod.yaml
```

### 8.2 Yaml 파일을 통한 매개변수 전달

### 8.2.1 fortuneloop.sh 작성

```
cd ../mkdir -p ./docimg/inyamlcd ./docimg/inyamlvi fortuneloop.sh
```

```
#!/bin/bash# 매개변수르 받지 않고 환경변로에서 바로 참조trap "exit" SIGINTecho "Configured to generate neew fortune every " $INTERVAL " seconds"mkdir /var/htdocswhile :do    echo $(date) Writing fortune to /var/htdocs/index.html    /usr/games/fortune  > /var/htdocs/index.html    sleep $INTERVALdone
```

### 8.2.2 Dockerfile 작성 및 build

```
$ vi dockerfile
```

```
FROM ubuntu:latestRUN apt-get update;  apt-get -y install fortuneADD fortuneloop.sh /bin/fortuneloop.shENTRYPOINT ["/bin/fortuneloop.sh"]CMD ["10"]  # args가 없으면 10초
```

```
$ docker build -t dangtong/fortune:env .$ docker push dangtong/fortune:env
```

### 8.2.2 Pod 생성및 매개변수 전달을 위한 yaml 파일 작성

```
cd ../../kubetmpvi config-fortune-inyaml-pod.yaml
```

```
apiVersion: v1kind: Podmetadata:  name: fortune30sspec:  containers:  - image: dangtong/fortune:env    env:    - name: INTERVAL      value: "30"    name: html-generator    volumeMounts:    - name: html      mountPath: /var/htdocs  - image: nginx:alpine    name: web-server    volumeMounts:    - name: html      mountPath: /usr/share/nginx/html      readOnly: true    ports:    - containerPort: 80      protocol: TCP  volumes:  - name: html    emptyDir: {}
```

### 8.3 ConfigMap 을 통한 설정

### 8.3.1 ConfigMap 생성 및 확인

- configMap 생성

```
$ kubectl create configmap fortune-config --from-literal=sleep-interval=7
```

> ConfigMap 생성시 이름은 영문자,숫자, 대시, 밑줄, 점 만 포함 할 수 있습니다.
> 
> 만약 여러개의 매개변수를 저장 할려면 –from-literal=sleep-interval=5 –from-literal=sleep-count=10 와 같이 from-literal 부분을 여러번 반복해서 입력 하면 됩니다.

- configMap 확인

```
$ kubectl get cm fortune-configNAME             DATA   AGEfortune-config   1      15s$ kubectl get configmap fortune-config. -o yamlapiVersion: v1data:  sleep-interval: "7"kind: ConfigMapmetadata:  creationTimestamp: "2020-04-16T09:09:33Z"  name: fortune-config  namespace: default  resourceVersion: "1044269"  selfLink: /api/v1/namespaces/default/configmaps/fortune-config  uid: 85723e0a-1959-4ace-9365-47c101ebef82
```

### 8.3.2 ConfigMap을 환경변수로 전달한는 yaml 파일 작성

- Yaml 파일 생성

```
vi config-fortune-mapenv-pod.yaml
```

```
apiVersion: v1kind: Podmetadata:  name: fortune7sspec:  containers:  - image: dangtong/fortune:env    env:    - name: INTERVAL      valueFrom:        configMapKeyRef:          name: fortune-config          key: sleep-interval    name: html-generator    volumeMounts:    - name: html      mountPath: /var/htdocs  - image: nginx:alpine    name: web-server    volumeMounts:    - name: html      mountPath: /usr/share/nginx/html      readOnly: true    ports:    - containerPort: 80      protocol: TCP  volumes:  - name: html    emptyDir: {}
```

### 8.3.3 Pod 생성 및 확인

- Pod 생성

```
$ kubectl create -f ./config-fortune-mapenv-pod.yaml
```

- Pod 생성 확인

```
$ kubectl get po, cm$ kubectl describe cmName:         fortune-configNamespace:    defaultLabels:       <none>Annotations:  <none>Data====sleep-interval:----7Events:  <none>
```

- configMap 삭제

```
$ kubectl delete configmap fortune-config
```

### 8.4 ConfigMap Volume 사용 (파일숨김)

### 8.4.1 ConfigMap 생성

```
mkdir config-dircd config-dirvi custom-nginx-config.conf
```

파일명 : custom-nginx-config.conf

```
server {    listen              8080;    server_name     www.acron.com;    gzip on;    gzip_types text/plain application/xml;    location / {        root    /usr/share/nginx/html;        index   index.html index.htm;    }}
```

```
$ kubectl create configmap fortune-config --from-file=config-dir
```

### 8.4.2 configMap volume 이용 Pod 생성

- Yaml 파일 작성 : config-fortune-mapvol-pod.yaml

```
apiVersion: v1kind: Podmetadata:  name: nginx-configvolspec:  containers:  - image: nginx:1.7.9    name: web-server    volumeMounts:    - name: config      mountPath: /etc/nginx/conf.d      readOnly: true    ports:    - containerPort: 8080      protocol: TCP  volumes:  - name: config    configMap:      name: fortune-config
```

> 서버에 접속해서 디렉토리 구조를 한번 보는것이 좋습니다.
> 
> kubectl exec -it nginx-pod bash

```
$ kubectl apply -f ./config-fortune-mapvol-pod.yaml
```

### 8.4.3 ConfigMap Volume 참조 확인

```
$ kubectl get pod -o wideAME              READY   STATUS    RESTARTS   AGE     IP          NODEnginx-configvol   1/1     Running   0          9m25s   10.32.0.2   worker01.acorn.com
```

- Response 에 압축(gzip)을 사용 하는지 확인

```
$ curl -H "Accept-Encoding: gzip" -I 10.32.0.2:8080
```

- 마운트된 configMap 볼륨 내용확인

```
$ kubectl exec nginx-configvol -c web-server ls /etc/nginx/conf.dnginx-config.conf$ kubectl exec -it nginx-configvol sh
```

### 8.4.4 ConfigMap 동적 변경

- 사전 테스트

```
$ curl -H "Accept-Encoding: gzip" -I 10.32.0.2:8080HTTP/1.1 200 OKServer: nginx/1.17.9Date: Fri, 17 Apr 2020 06:10:32 GMTContent-Type: text/htmlLast-Modified: Tue, 03 Mar 2020 14:32:47 GMTConnection: keep-aliveETag: W/"5e5e6a8f-264"Content-Encoding: gzip
```

- configMap 변경

```
$ kubectl edit cm fortune-config
```

**gzip on** 부분을 **gzip off** 로 변경 합니다.

```
apiVersion: v1data:  nginx-config.conf: "server {\n  listen\t80;\n  server_name\tnginx.acorn.com;\n\n    \ gzip off;\n  gzip_types text/plain application/xml;\n  location / {\n    root\t/usr/share/nginx/html;\n    \   index\tindex.html index.htm;\n  }  \n}\n\n\n"  nginx-ssl-config.conf: '#\#Nginx SSL Config'  sleep-interval: |    25kind: ConfigMapmetadata:  creationTimestamp: "2020-04-16T15:58:42Z"  name: fortune-config  namespace: default  resourceVersion: "1115758"  selfLink: /api/v1/namespaces/default/configmaps/fortune-config  uid: 182302d8-f30f-4045-9615-36e24b185ecb
```

- 변경후 테스트

```
$ curl -H "Accept-Encoding: gzip" -I 10.32.0.2:8080HTTP/1.1 200 OKServer: nginx/1.17.9Date: Fri, 17 Apr 2020 06:10:32 GMTContent-Type: text/htmlLast-Modified: Tue, 03 Mar 2020 14:32:47 GMTConnection: keep-aliveETag: W/"5e5e6a8f-264"Content-Encoding: gzip
```

- nginx reload 명령으로 설정 갱신

```
$ kubectl  exec nginx-configvol -c web-server -- nginx -s reload
```

- 최종 테스트

```
$ curl -H "Accept-Encoding: gzip" -I 10.32.0.2:8080HTTP/1.1 200 OKServer: nginx/1.17.9Date: Fri, 17 Apr 2020 06:10:32 GMTContent-Type: text/htmlLast-Modified: Tue, 03 Mar 2020 14:32:47 GMTConnection: keep-aliveETag: W/"5e5e6a8f-264"
```

### 8.5 ConfigMap Volume(파일 추가)

### 8.5.1 configMap volume 이용 Pod 생성

- Yaml 파일 작성 : config-fortune-mapvol-pod2.yaml

```
apiVersion: v1kind: Podmetadata:  name: nginx-configvolspec:  containers:  - image: nginx:1.7.9    name: web-server    volumeMounts:    - name: config      mountPath: /etc/nginx/conf.d/default.conf      subPath: nginx-config.conf # 주의 : configmap 의 key 와 파일명이 일치 해야합니다.      readOnly: true    ports:    - containerPort: 8080      protocol: TCP  volumes:  - name: config    configMap:      name: fortune-config      defaultMode: 0660
```

> nginx 1.7.9 이상 버전, 예를 들면 nginx:latest 로 하면 /etc/nginx/conf.d 폴더내에 default.conf 파일만 존재 합니다. Example_cat tssl.conf 파일은 없습니다. 테스트를 위해서 nginx:1.7.9 버전으로 설정 한것입니다.

```
$ kubectl apply -f ./config-fortune-mapvol-pod.yaml
```

### 8.5.2 서비스 및 ConfiMap 확인

- 서비스 확인

```
$ curl -H "Accept-Encoding: gzip" -I 10.32.0.2:8080
```

- configMap 확인

```
$ kubectl exec nginx-configvol -c web-server ls /etc/nginx/conf.d
```

### 8.5.3 ConfigMap 추가

- configMap 추가

```
$ kubectl edit cm fortune-config
```

아래와 같이 nginx-ssl-config.conf 키값을 추가합니다.

```
apiVersion: v1data:  nginx-config.conf: "server {\n  listen\t80;\n  server_name\tnginx.acorn.com;\n\n    \ gzip on;\n  gzip_types text/plain application/xml;\n  location / {\n    root\t/usr/share/nginx/html;\n    \   index\tindex.html index.htm;\n  }  \n}\n\n\n"  nginx-ssl-config.conf: ""#\#Nginx SSL Config" # 이부분이 추가됨  sleep-interval: |    25kind: ConfigMapmetadata:  creationTimestamp: "2020-04-16T15:58:42Z"  name: fortune-config  namespace: default  resourceVersion: "1098337"  selfLink: /api/v1/namespaces/default/configmaps/fortune-config  uid: 182302d8-f30f-4045-9615-36e24b185ecb
```

### 8.5.4 ConfigMap 추가된 Pod 생성

- 설정 파일 추가 하기위해 yaml 파일 수정

파일명 : config-fortune-mapvol-pod3.yaml

```
kind: Podmetadata:  name: nginx-configvolspec:  containers:  - image: nginx:1.7.9    name: web-server    volumeMounts:    - name: config \#default.conf 파일 교체      mountPath: /etc/nginx/conf.d/default.conf      subPath: nginx-config.conf      readOnly: true    - name: config \#example_ssl.conf 파일 교체      mountPath: /etc/nginx/conf.d/example_ssl.conf      subPath: nginx-ssl-config.conf      readOnly: true    ports:    - containerPort: 8080      protocol: TCP  volumes:  - name: config    configMap:      name: fortune-config      defaultMode: 0660
```

```
$ kubectl apply -f ./ config-fortune-mapvol-pod3.yaml
```

### 8.5.6 추가된 ConfigMap 확인

- 서비스 확인

```
$ curl http://10.32.0.4
```

- configMap 확인

```
$ kubectl exec -it nginx-configvol bash$ cd /etc/nginx/conf.d$ ls -al
```

## 9. Secret

```
mkdir -p ./secret/certmkdir -p ./secret/configmkdir -p ./secret/kubetmp
```

### 9.1 Secret 생성 (fortune-https)

```
cd ./secret/cert
```

```
openssl genrsa -out https.key 2048openssl req -new -x509 -key https.key -out https.cert -days 360 -subj '/CN=*.acron.com'
```

```
kubectl create secret generic fortune-https --from-file=https.key --from-file=https.cert
```

### 9.2 SSL 용 niginx 설정 생성(fortune-config)

```
cd ./secret/configvi custom-nginx-config.conf
```

```
server {    listen              8080;  listen                443 ssl;    server_name     www.acron.com;    ssl_certificate     certs/https.cert;    ssl_certificate_key certs/https.key;    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;    ssl_ciphers     HIGH:!aNULL:!MD5;    gzip on;    gzip_types text/plain application/xml;    location / {        root    /usr/share/nginx/html;        index   index.html index.htm;    }}
```

```
vi sleep-interval7
```

```
kubectl create cm fortune-config --from-file=./config
```

### 9.3 POD 생성

- Pod 생성 : 8.3.2 의 yaml 파일 참조
- 파일명 : secret-pod.yaml

```
apiVersion: v1kind: Podmetadata:  name: fortune-httpsspec:  containers:  - image: dangtong/fortune:env    env:    - name: INTERVAL      valueFrom:        configMapKeyRef:          name: fortune-config          key: sleep-interval    name: html-generator    volumeMounts:    - name: html      mountPath: /var/htdocs  - image: nginx:alpine    name: web-server    volumeMounts:    - name: html      mountPath: /usr/share/nginx/html      readOnly: true    - name: config # 추가      mountPath: /etc/nginx/conf.d      readOnly: true    - name: certs # 추가      mountPath: /etc/nginx/certs/      readOnly: true    ports:    - containerPort: 80    - containerPort: 443 # 추가  volumes:  - name: html    emptyDir: {}  - name: config # 추가    configMap:      name: fortune-config      items:      - key: custom-nginx-config.conf        path: https.conf  - name: certs  #추가    secret:      secretName: fortune-https
```

```
kubectl apply -f ./secret-pod.yaml
```

### 9.4 서비스 확인

```
kubectl port-forward fortune-https 8443:443 &
```

```
curl https://localhsot:8443 -kcurl https://localhost:8443 -k -v
```

### [Exercise \#7]

### 1. Mysql 구성하기

### 1.1 서비스 구성

파일명 : mysql-svc.yaml

```
apiVersion: v1kind: Servicemetadata:  name: wordpress-mysql  labels:    app: wordpressspec:  ports:    - port: 3306  selector:    app: wordpress    tier: mysql  clusterIP: None
```

> 해드리스(Headless) 서비스를 만들면 명시적인 클러스터 IP 가 할당되지 않고 라우팅 됩니다. 헤드리스 서비스는 다른 네임스페이스의 서비스에 라우팅이 필요 할때도 유용 합니다. 헤드리스 서비스의 ExternalName 을 사용하 다른 네임스페이스의 서비스를 연결 할 수 있습니다. 원래 ExternalName은 K8s 외부의 서비스를 K8s 내부 서비스인 것처럼 참조할수 있게 해주는 기능 입니다.
> 
> - 셀렉터가 있는경우 : 엔드포인트 컨트롤러가 엔드포인트 레코드를 생성하고 DNS 를 업데이트 하여 파드를 가르키는 A레코드(IP주소) 반환 하여 라우팅이 가능 합니다.
> - 셀렉터기 없는 경우 : DNS 시스템이 CNAME레코드 구성 하거나 서비스 이름으로 구성된 엔트포인트 레코드 참조
> - A 레코드와 CNAME 의 차이
>     - A레코드 : 간단하게 도메인 이름에 IP Address 를 매핑하는 레코드
>         
>         ```
>         Name: www.acorn.comAddress: 10.0.24.3
>         ```
>         
>     - CNAME(Canonical Name) : 도메인의 별칭을 부여하는 방식. 도메인의 또 다른 도메인 이름, A 레코드는 직접적으로 IP가 할당 되기 때문에 IP가 변경되면 직접 도메인에 영향을 미치지만, CNAME은 도메인에 매핑되어 있기 때문에 IP 변경에 직접적인 영향을 받지 않음
>         
>         ```
>         www.acornstudy.com -> study.acorn.com (CNAME)study.acorn.com -> 211.231.99.250 (A record)
>         ```
>         
>         ```
>         mysql.acorn.com -> 211.231.99.250 (A record) 직접통신
>         ```
>         

### 1.2 볼륨 구성

파일명 : mysql-pvc.yaml

```
apiVersion: v1kind: PersistentVolumeClaimmetadata:  name: mysql-pv-claim  labels:    app: wordpressspec:  accessModes:    - ReadWriteOnce  resources:    requests:      storage: 20Gi
```

### 1.3 Pod 구성

- mysql 비번을 secret 을 생성하여 저장 합니다.

```
kubectl create secret generic mysql-pass  --from-literal=password='admin123'
```

파일명 : mysql-deploy.yaml

```
apiVersion: apps/v1 # 1.9.0 이전 버전은 apps/v1beta2 를 사용하세요kind: Deploymentmetadata:  name: wordpress-mysql  labels:    app: wordpressspec:  selector:    matchLabels:      app: wordpress      tier: mysql  strategy:    type: Recreate  template:    metadata:      labels:        app: wordpress        tier: mysql    spec:      containers:      - image: mysql:5.6        name: mysql        env:        - name: MYSQL_ROOT_PASSWORD          valueFrom:            secretKeyRef:              name: mysql-pass              key: password        ports:        - containerPort: 3306          name: mysql        volumeMounts:        - name: mysql-persistent-storage          mountPath: /var/lib/mysql      volumes:      - name: mysql-persistent-storage        persistentVolumeClaim:          claimName: mysql-pv-claim
```

```
kubectl apply -f ./mysql-deploy.yaml
```

### 2. WordPress 서비스 구성

### 2.1 LoadBalancer 구성

```
apiVersion: v1kind: Servicemetadata:  name: wordpress  labels:    app: wordpressspec:  ports:    - port: 80  selector:    app: wordpress    tier: frontend  type: LoadBalancer
```

### 2.2 PVC 구성

```
apiVersion: v1kind: PersistentVolumeClaimmetadata:  name: wp-pv-claim  labels:    app: wordpressspec:  accessModes:    - ReadWriteOnce  resources:    requests:      storage: 20Gi
```

### 2.3 WordPress 구성

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: wordpress  labels:    app: wordpressspec:  selector:    matchLabels:      app: wordpress      tier: frontend  strategy:    type: Recreate  template:    metadata:      labels:        app: wordpress        tier: frontend    spec:      containers:      - image: wordpress:4.8-apache        name: wordpress        env:        - name: WORDPRESS_DB_HOST          value: wordpress-mysql        - name: WORDPRESS_DB_PASSWORD          valueFrom:            secretKeyRef:              name: mysql-pass              key: password        ports:        - containerPort: 80          name: wordpress        volumeMounts:        - name: wordpress-persistent-storage          mountPath: /var/www/html      volumes:      - name: wordpress-persistent-storage        persistentVolumeClaim:          claimName: wp-pv-claim
```

## 10. StatefullSet (GCP 수행)

### 10.1 애플리케이션 이미지 작성

### 10.1.1 app.js 작성

```
const http = require('http');const os = require('os');const fs = require('fs');const dataFile = "/var/data/kubia.txt";const port  = 8080;// 파일 존재  유/무 검사function fileExists(file) {  try {    fs.statSync(file);    return true;  } catch (e) {    return false;  }}var handler = function(request, response) {//  POST 요청일 경우 BODY에 있는 내용을 파일에 기록 함  if (request.method == 'POST') {    var file = fs.createWriteStream(dataFile);    file.on('open', function (fd) {      request.pipe(file);      console.log("New data has been received and stored.");      response.writeHead(200);      response.end("Data stored on pod " + os.hostname() + "\n");    });// GET 요청일 경우 호스트명과 파일에 기록된 내용을 반환 함  } else {    var data = fileExists(dataFile) ? fs.readFileSync(dataFile, 'utf8') : "No data posted yet";    response.writeHead(200);    response.write("You've hit " + os.hostname() + "\n");    response.end("Data stored on this pod: " + data + "\n");  }};var www = http.createServer(handler);www.listen(port);
```

### 10.1.2 Docker 이미지 만들기

- Dockerfile 작성

```
FROM node:7ADD app.js /app.jsENTRYPOINT ["node", "app.js"]
```

- Docker 이미지 build 및 push

```
$ docker build dangtong/nodejs:sfs .$ docker login$ docker push dangtong/nodejs:sfs
```

### 10.2 Sotage Class 생성

```
apiVersion: storage.k8s.io/v1kind: StorageClassmetadata:  name: sc-standard-retainprovisioner: kubernetes.io/gce-pdreclaimPolicy: Retainparameters:  type: pd-ssd  zone: asia-northeast1-c
```

### 10.3 서비스 및 Pod 생성

### 10.3.1 로드밸런서 서비스 생성

```
apiVersion: v1kind: Servicemetadata:    name: nodesjs-sfs-lbspec:    type: LoadBalancer    ports:    - port: 80      targetPort: 8080    selector:        app: nodejs-sfs
```

### 10.3.2 Pod 생성

```
apiVersion: apps/v1kind: StatefulSetmetadata:  name: nodejs-sfsspec:  selector:    matchLabels:      app: nodejs-sfs  serviceName: nodejs-sfs  replicas: 2  template:    metadata:      labels:        app: nodejs-sfs    spec:      containers:      - name: nodejs-sfs        image: dangtong/nodejs:sfs        ports:        - name: http          containerPort: 8080        volumeMounts:        - name: data          mountPath: /var/data  volumeClaimTemplates:  - metadata:      name: data    spec:      resources:        requests:          storage: 1Mi      accessModes:      - ReadWriteOnce      storageClassName: sc-standard-retain
```

```
kubectl apply -f nodejs-sfs.yaml
```

```
kubectl get svcNAME                     TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)        AGEservice/kubernetes       ClusterIP      10.65.0.1      <none>         443/TCP        7h43mservice/nodesjs-sfs-lb   LoadBalancer   10.65.12.243   34.85.38.158   80:32280/TCP   120m
```

### 10.4 서비스 테스트

- 데이터 조회

```
curl http://34.85.38.158
```

- 데이터 입력

```
curl -X POST -d "hi, my name is dangtong-1" 34.85.38.158curl -X POST -d "hi, my name is dangtong-2" 34.85.38.158curl -X POST -d "hi, my name is dangtong-3" 34.85.38.158curl -X POST -d "hi, my name is dangtong-4" 34.85.38.158
```

> 데이터 입력을 반복하에 두개 노드 모드에 데이터를 모두 저장 합니다. 양쪽 노드에 어떤 데이터가 입력 되었는지 기억 하고 다음 단계로 넘어 갑니다.

### 10.5 노드 삭제 및 데이터 보존 확인

- 노드 삭제 및 자동 재생성

```
kubectl delete pod nodejs-sfs-0
```

노드를 삭제 한뒤 노드가 재생성 될때 까지 기다립니다.

- 데이터 보존 확인

```
curl http://34.85.38.158
```

> 노드가 삭제 되었다 재생성 되도 기존 디스크를 그대로 유지하는 것을 볼 수 있습니다. ReclaimPolicy 를 Retain 으로 했기 때문 입니다.

## 11. 리소스 제어

### 11.1 부하 발생용 애플리 케이션 작성

### 11.1.1 PHP 애플리 케이션 작성

파일명 : index.php

```
D
```

### 11.1.2 도커 이미지 빌드

```
FROM php:5-apacheADD index.php /var/www/html/index.phpRUN chmod a+rx index.php
```

```
docker build -t dangtong/php-apache .docker logindocker push dangtong/php-apache
```

### 11.2 포드 및 서비스 만들기

### 12.2.1 Deployment 로 Pod 새성

파일명 : php-apache-deploy.yaml

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: php-apache-dpspec:  selector:    matchLabels:      app: php-apache  replicas: 1  template:    metadata:      labels:        app: php-apache    spec:      containers:      - name: php-apache        image: dangtong/php-apache        ports:        - containerPort: 80        resources:          limits:            cpu: 500m          requests:            cpu: 200m
```

### 12.2.2 로드밸런서 서비스 작성

파일명 : php-apache-svc.yaml

```
apiVersion: v1kind: Servicemetadata:  name: php-apache-lbspec:  type: LoadBalancer  ports:  - port: 80    targetPort: 80  selector:    app: php-apache
```

```
kubectl apply -f ./php-apache-deploy.yamlkubectl apply -f ./php-apache-svc.yaml
```

### [연습문제] 10-1 Pod 리소스 제어하기

아래 요구 사항에 맞는 deploy 를 구현 하세요

1. Deploy name : nginx
2. image : nginx
3. cpu 200m
4. 메모리 : 300Mi
5. Port : 80

### 12.3 HPA 리소스 생성

### 12.3.1 HPA 리소스 생성

```
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=5
```

### 12.3.2 HPA 리소스 생성 확인

```
kubectl get hpa
```

### 12.4 Jmeter 설치 및 구성

### 12.4.1 Jmeter 설치를 위해 필요한 것들

- JDK 1.8 이상 설치 [오라클 java SE jdk 다운로드](https://www.oracle.com/java/technologies/javase-downloads.html)
- Jmeter 다운로드 [Jmeter 다운로드](https://jmeter.apache.org/download_jmeter.cgi)
    
    플러그인 다운로드 [Jmeter-plugin 다운로드](https://jmeter-plugins.org/install/Install/)
    
    - Plugins-manager.jar 다운로드 하여 jmeter 내에 lib/ext 밑에 복사 합니다. jpgc

### 12.4.2 Jmeter 를 통한 부하 발생

[![](https://www.notion.so./img/jmeter.png)](https://www.notion.so./img/jmeter.png)

jmeter

### 12.4.3 부하 발생후 Pod 모니터링

```
$ kubectl get hpa$ kubectl top podsNAME                          CPU(cores)   MEMORY(bytes)nodejs-sfs-0                  0m           7Minodejs-sfs-1                  0m           7Miphp-apache-6997577bfc-27r95   1m           9Mi$ kubectl exec -it nodejs-sfs-0 topTasks:   2 total,   1 running,   1 sleeping,   0 stopped,   0 zombie%Cpu(s):  4.0 us,  1.0 sy,  0.0 ni, 95.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 stKiB Mem:   3786676 total,  3217936 used,   568740 free,   109732 buffersKiB Swap:        0 total,        0 used,        0 free.  2264392 cached Mem    PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND      1 root      20   0  813604  25872  19256 S  0.0  0.7   0:00.17 node     11 root      20   0   21924   2408   2084 R  0.0  0.1   0:00.00 top
```

### [연습문제] 12-1 HPA 적용 실습

이미지를 nginx 를 생성하고 HPA 를 적용하세요

- Max : 8
- Min : 2
- CPU 사용량이 40% 가 되면 스케일링 되게 하세요

## 13. Kubernetes DashBoard 설치

### 13.1 Dashboard 설치

참조 URL : https://kubernetes.io/ko/docs/tasks/access-application-cluster/web-ui-dashboard/

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

### 13.2 DashBoard 인증서 생성

```
mkdir -p ./kubeSrc/dashboard/certsmkdir -p ./kubeSrc/dashboard/yamlcd ./kubeSrc/dashboard/certs
```

### 13.2.1 개인키 생성

```
openssl genrsa -des3 -out dashboard-private.key 2048Generating RSA private key, 2048 bit long modulus (2 primes).....................................................................................+++++...........................................+++++e is 65537 (0x010001)Enter pass phrase for dashboard-private.key:Verifying - Enter pass phrase for dashboard-private.key:# openssl rsa --noout -text -in dashboard-private.key
```

### 13.2.1 공개키 생성

```
# 동일한  pass phrase 를 입력 합니다.openssl rsa -in dashboard-private.key -out dashboard-public.key
```

### 13.2.2 CSR 생성

```
openssl req -new -key dashboard-public.key -out dashboard.csrYou are about to be asked to enter information that will be incorporatedinto your certificate request.What you are about to enter is what is called a Distinguished Name or a DN.There are quite a few fields but you can leave some blankFor some fields there will be a default value,If you enter '.', the field will be left blank.-----Country Name (2 letter code) [AU]:KRState or Province Name (full name) [Some-State]:SeoulLocality Name (eg, city) []:Guro-GuOrganization Name (eg, company) [Internet Widgits Pty Ltd]:acornOrganizational Unit Name (eg, section) []:eduCommon Name (e.g. server FQDN or YOUR name) []:*.acorn.comEmail Address []:dangtong@gmail.comPlease enter the following 'extra' attributesto be sent with your certificate requestA challenge password []:admin123An optional company name []:acorn
```

### 13.2.3 인증서 생성

```
openssl x509 -req -sha256 -days 3650 -in dashboard.csr -signkey dashboard-public.key -out kubernetes-dashboard.crt
```

### 13.3 DashBoard 생성

### 13.3.1 네임스페이스 및 Secret 생성

- 네임스페이스 생성

```
kubectl create namespace kubernetes-dashboard
```

- Secret 생성

```
kubectl create secret generic kubernetes-dashboard-certs --from-file=$HOME/kubeSrc/dashboard/certs -n kubernetes-dashboard
```

- Secret 확인

```
kubectl get secret -n kubernetes-dashboard-certs -n kubernetes-dashboard
```

### 13.3.2 DashBoard Deployment

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

### 13.3.3 ClusterIP 를 NodePort 방식으로 변경

```
kubectl edit service kubernetes-dashboard -n kubernetes-dashboard
```

```
...ports:  - nodePort: 30812 # 이부분 추가    port: 443    protocol: TCP    targetPort: 8443  selector:    k8s-app: kubernetes-dashboard  sessionAffinity: None  type: NodePort # 이부분 변경
```

### 13.3.4 서비스 확인

```
kubectl get service -n kubernetes-dashboard
```

### 13.2.

### 13.2 NodePort 서비스

```
spec:  clusterIP: 10.103.46.146  ports:  - port: 443    protocol: TCP    targetPort: 8443    nodePort: 30124  selector:    k8s-app: kubernetes-dashboard  sessionAffinity: None  type: NodePortstatus:  loadBalancer: {}
```

### 13.2 Client 인증서 만들기

- kubectl 의 key 복사

```
grep 'client-key-data' ~/.kube/config | head -n 1 | awk '{print $2}' | base64 -d >> kubecfg.key
```

- 인증서 생성

```
# CSR 생성openssl req -new -key  kubecfg.key -out kubecfg.csr -subj "/CN=admin-user"# 인증서 생성sudo openssl x509 -req -in kubecfg.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out kubecfg.crt -days 500
```

- 브라우저에서 사용 가능한 PKCS\#12 양식으로 변환 합니다

```
openssl pkcs12 -export -clcerts -inkey kubecfg.key -in kubecfg.crt -out kubecfg.p12 -name "admin-user"
```

- CA 인증서를 복사합니다.

```
cp /etc/kubernetes/pki/ca.crt .
```

```
kubeadmin@kmaster:~/kubeSrc/dashboard$ lsca.crt  kubecfg.crt  kubecfg.csr  kubecfg.key  kubecfg.p12
```

- Sftp 로 서버에 접속해서 kubecfg.crt / kubecfg.p12 / ca.crt 파일을 다운로드 받습니다.

### 13.3 인증서

### 13.3.1 윈도우 인증서 Import

- 윈도우 Root CA 인증서 Import

```
C:\Windows\system32>certutil.exe -addstore "Root" C:\Users\dangtong\Documents\ca.crtRoot "신뢰할 수 있는 루트 인증 기관"서명이 공개 키와 일치합니다."kubernetes" 인증서가 저장소에 추가되었습니다.CertUtil: -addstore 명령이 성공적으로 완료되었습니다.
```

- PKCS\#12 인증서 Imort

```
C:\Windows\system32>certutil.exe -p admin123 -user -importPFX
```

[![](https://www.notion.so../img/cert_capture.PNG)](https://www.notion.so../img/cert_capture.PNG)

cert_capture

```
C:\Users\dangtong\Documents\kubecfg.p12"kubernetes-admin" 인증서가 저장소에 추가되었습니다.CertUtil: -importPFX 명령이 성공적으로 완료되었습니다.
```

### 13.3.2 OSX 인증서 Import

- Root CA 인증서 Import

```
security add-trusted-cert  -r trustRoot -k "$HOME/Library/Keychains/login.keychain" /Users/dangtong/kube-dashboard/ca.crt
```

- PKCS\#12 인증서 Import

```
security import   /Users/dangtong/kube-dashboard/kubecfg.p12 -k "$HOME/Library/Keychains/login.keychain" -P admin123
```

### 13.4 서비스 어카운트 및 롤생성

### 13.4.1 서비스 어카운트 생성

파일명 : admin-user.yaml

```
apiVersion: v1kind: ServiceAccountmetadata:  name: admin-user  namespace: kubernetes-dashboard
```

### 13.4.2 롤생성

파일명 : admin-user-role.yaml

```
apiVersion: rbac.authorization.k8s.io/v1kind: ClusterRoleBindingmetadata:  name: admin-user-role-bindingroleRef:  apiGroup: rbac.authorization.k8s.io  kind: ClusterRole  name: cluster-adminsubjects:- kind: ServiceAccount  name: admin-user  namespace: kubernetes-dashboard
```

```
kubectl apply -f ./admin-user.yamlkubectl apply -f ./admin-user-role.yaml
```

### 13.4.3 인증 토큰 조회

아래 명령을 수행해서 인증 토큰을 복사 합니다.

```
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')Name:         admin-user-token-28l6nNamespace:    kubernetes-dashboardLabels:       <none>Annotations:  kubernetes.io/service-account.name: admin-user              kubernetes.io/service-account.uid: d3f6b3d6-63f3-4483-88f0-dc8e6f37ffd8Type:  kubernetes.io/service-account-tokenData====ca.crt:     1025 bytesnamespace:  20 bytestoken:      eyJhbGciOiJSUzI1NiIsImtpZCI6InEwaS1COEtFcjNwd0RBbE1KUHpaNTZuZlBlSmY4a1Nld1FZYjhIam5ZV2cifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLTI4bDZuIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJkM2Y2YjNkNi02M2YzLTQ0ODMtODhmMC1kYzhlNmYzN2ZmZDgiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZXJuZXRlcy1kYXNoYm9hcmQ6YWRtaW4tdXNlciJ9.JB1hGeIxJyU-0VbOmMGcp0b6B758nqGAXhgBFOj9eZMj73h7BNizO4aEiDMsJgH3tvtXRxChUHIMFT0kBeX6i7U-3GYvS_Uw7XNREYdhrYl8PVoPcqcXi7tL1tTrCEj-Rxy7HxyHXGGwb6wJYO11BgzuwCLJWjm-i-G3N3yrzi8KQ7MjWKVPisrxgPna_dKO6LgrCiq6oVO14FFVbwB6IqdiH8jpHrH2GSIKjFCAAziHSoHBLbc6qv5_9hle5CaO6LZxTlZCt4H7drWnUUyVmjz1sqkUZg08EVfewVzZCQXPig6Rj3DTGbFrzg1Zk3h3EBJq4VhjnCYj1ePNDw_-eg
```

### 13.5 DashBoard 접속

- 접속 URL

https://192.168.56.111:6443/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

[![](https://www.notion.so../img/dashboard_login.png)](https://www.notion.so../img/dashboard_login.png)

dashboard_login

토큰 을 선택하고 복사한 토큰을 **토큰입력** 란에 붙여넣고 **로그인** 합니다.

### 13.6 삭제

```
kubectl --namespace kube-system delete deployment,service kubernetes-dashboard
```

## 14. 계정 및 인증

### 14.1 User Account 및 RBAC 사용

### 14.1.1 유저 생성

```
# root 로 수행adduser devusersu - devuser# devuser 로 수행mkdir .certmkdir .kube
```

### 14.1.2 유저 인증서 생성(root 로 수행)

- 인증서 생성

```
cd ~mkdir devuser-certcd devuser-cert# 키생성openssl genrsa -out devuser.key 2048# CSR 생성openssl req -new -key devuser.key -out devuser.csr -subj "/CN=devuser/O=devuser"# 인증서 생성openssl x509 -req -in devuser.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out devuser.crt -days 500
```

- 인증서 복사

```
cp devuser.crt /home/devuser/.cert/cp devuser.key /home/devuser/.cert/chown -R devuser:devuser /home/devuser/.cert
```

### 14.1.3 kubectl 로 config 파일 만들기(devuser 로 수행)

- API 서버 접속 및 CA 인증서 설정

```
cd ~/.kubekubectl config --kubeconfig=config set-cluster devuser-cluster --server=https://192.168.56.111:6443 --certificate-authority=/etc/kubernetes/pki/ca.crt
```

```
cat ~/.kube/config
```

- 사용자 접속 정보 (인증서 정보 설정)

```
kubectl config --kubeconfig=config set-credentials devuser --client-certificate=/home/devuser/.cert/devuser.crt --client-key=/home/devuser/.cert/devuser.key
```

- 컨텍스트 설정

```
kubectl config --kubeconfig=config set-context devuser-context --cluster=devuser-cluster --namespace=dev --user=devuser
```

```
cat ~/.kube/config
```

- 테스트

```
kubectl get podThe connection to the server localhost:8080 was refused - did you specify the right host or port?kubectl --context=devuser-context get poError from server (Forbidden): pods is forbidden: User "devuser" cannot list resource "pods" in API group "" in the namespace "dev"
```

> 메시지가 왜 다른지 생각 해봅시다. config 를 열어서 current-context: “devuser-context” 로 수정한 후 다시 수행 합니다.

### 14.1.4 네임스페이스 생성 (root 로 수행)

```
kubectl create namespace dev
```

### 14.1.5 롤생성 (root 로 수행)

파일명 : dev-role.yaml

```
kind: RoleapiVersion: rbac.authorization.k8s.io/v1beta1metadata:  namespace: dev  name: dev-rolerules:- apiGroups: ["", "extensions", "apps"]  resources: ["deployments", "replicasets", "pods"]  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

```
kubectl apply -f ./dev-role.yaml
```

|   |   |
|---|---|
|Verb|설명|
|create|리소스 생성|
|get|리소스 조회|
|list|여러건 리소스 동시 조회|
|update|리소스 전체 내용 업데이트|
|patch|리소스 일부 내용 변경|
|delete|단일 리소스 삭제|
|Delete collection|여러 리소스 삭제|

> resource 뿐만 아니라 resourceNames 를 지정해서 특정 Pod 나 리소스에 대한 권한 으로 제한 할 수도 있음

### 14.1.6 롤바인딩 생성 (root 로 수행)

파일명 : dev-role-bind.yaml

```
kind: RoleBindingapiVersion: rbac.authorization.k8s.io/v1beta1metadata:  name: dev-role-binding  namespace: devsubjects:- kind: User  name: devuser  apiGroup: ""roleRef:  kind: Role  name: dev-role  apiGroup: ""
```

```
kubectl apply -f ./dev-role-bind.yaml
```

### 14.1.7 테스트 Pod 생성 및 테스트 (devuser 로 수행)

```
# 사용 가능한 context 확인kubectl config get-contexts# context로 전환kubectl config use-context devuser-context# 현재 사용중인 context 확인kubectl config current-context
```

```
kubectl --context=devuser-context run --generator=run-pod/v1 nginx --image=nginx --port=80 --dry-run -o yaml > nginx-pod.yaml
```

```
kubectl apply -f ./nginx-pod.yamlkubectl get pod -o wideNAME    READY   STATUS    RESTARTS   AGE     IP          NODE                 NOMINATED NODE   READINESS GATESnginx   1/1     Running   0          2m39s   10.40.0.8   worker01.acorn.com   <none>           <none>curl http://10.40.0.8
```

### 14.2 Service Account 및 RBAC

### 14.2.1 Service Account 생성

- Service Account 생성

```
kubectl create sa apiuser# kubectl create sa apiuser --dry-run -o yamlkubectl get sakubectl get sa --all-namespaces
```

- 토큰 Secret 생성

```
apiVersion: v1kind: Secretmetadata:  name: apiuser-secret  annotations:    kubernetes.io/service-account.name: apiusertype: kubernetes.io/service-account-token
```

```
kubectl describe secrets/apiuser-secret
```

### 14.2.2 롤 생성

- 롤 생성

```
kind: RoleapiVersion: rbac.authorization.k8s.io/v1metadata:  name: read-role  namespace: defaultrules:- apiGroups: [""]  resources: ["pods"]  verbs: ["get", "list"]
```

### 14.2.3 롤 바인딩

```
kind: RoleBindingapiVersion: rbac.authorization.k8s.io/v1metadata:  name: read-role-binding  namespace: defaultsubjects:- kind: ServiceAccount  name: apiuser  apiGroup: ""roleRef:  kind: Role  name: read-role  apiGroup: rbac.authorization.k8s.io
```

### 14.2.4 토큰 인코딩 및 서비스 테스트

- 토큰 인코딩

```
kubectl get secret apiuser-secret -o yamlapiVersion: v1kind: ServiceAccountmetadata:  creationTimestamp: "2020-04-30T19:48:15Z"  name: apiuser  namespace: default  resourceVersion: "1715742"  selfLink: /api/v1/namespaces/default/serviceaccounts/apiuser  uid: 3b658e4a-ec4f-4eda-b790-ba74d0607973secrets:- name: apiuser-token-llvcb  # 토큰
```

```
kubectl get secrets apiuser-token-llvcb -o json | jq -Mr '.data.token' | base64 -d
```

- Pod 리스트 조회

```
curl -k  https://192.168.56.111:6443/api/v1/namespaces/default/pods/  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6InEwaS1COEtFcjNwd0RBbE1KUHpaNTZuZlBlSmY4a1Nld1FZYjhIam5ZV2cifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImFwaXVzZXItdG9rZW4tbGx2Y2IiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiYXBpdXNlciIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjNiNjU4ZTRhLWVjNGYtNGVkYS1iNzkwLWJhNzRkMDYwNzk3MyIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmFwaXVzZXIifQ.qVfF3Tt9fAI6uC8CfyiuSBYnYU8z8sLQIRt3sH0B3zgpcToxPY9aAuoADmg-ULUQJBJjiBgbhWxy1ujpJwKy-jLb-0-MqIype2Z58dlNCOupaWnFDzYMJr9XLL33L6KD7SSVTp22CP89KGj_TxDdOrLj6jQi07WGBBqsliCPu5xy1p6SoiOdl-FxWpHqJxfhgtpVX1ntPA_DN6H_CiFPvvKoaWAy76HzH79aMbdmtR4NDpKYoCo1vbNmnWuQ2571lL4einBKkHj8bi0zeBa1cKvuqTUHftGV4KDYQ0nsX1R7Ispk3XNuMLgx7Lxl6idDUxfDkeRElZraMo3FgMYN0w"
```

- 특정 Pod 조회

```
curl -k https://192.168.56.111:6443/api/v1/namespaces/default/pods/goapp-deployment-5857594fbb-46ckf  -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6InEwaS1COEtFcjNwd0RBbE1KUHpaNTZuZlBlSmY4a1Nld1FZYjhIam5ZV2cifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImFwaXVzZXItdG9rZW4tbGx2Y2IiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiYXBpdXNlciIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjNiNjU4ZTRhLWVjNGYtNGVkYS1iNzkwLWJhNzRkMDYwNzk3MyIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmFwaXVzZXIifQ.qVfF3Tt9fAI6uC8CfyiuSBYnYU8z8sLQIRt3sH0B3zgpcToxPY9aAuoADmg-ULUQJBJjiBgbhWxy1ujpJwKy-jLb-0-MqIype2Z58dlNCOupaWnFDzYMJr9XLL33L6KD7SSVTp22CP89KGj_TxDdOrLj6jQi07WGBBqsliCPu5xy1p6SoiOdl-FxWpHqJxfhgtpVX1ntPA_DN6H_CiFPvvKoaWAy76HzH79aMbdmtR4NDpKYoCo1vbNmnWuQ2571lL4einBKkHj8bi0zeBa1cKvuqTUHftGV4KDYQ0nsX1R7Ispk3XNuMLgx7Lxl6idDUxfDkeRElZraMo3FgMYN0w"
```

### 14.3 클러스터롤 과 집계 클러스터롤

### 14.3.1 클러스터롤 생성

```
vi read-clusterrole.yaml
```

```
kind: ClusterRoleapiVersion: rbac.authorization.k8s.io/v1metadata:  name: read-cluster-rolerules:- apiGroups: [""]  resources: ["pods"]  verbs: ["get", "list"]
```

### 14.2.3 Aggregation 클러스터룰 생성

- 생성

```
kind: ClusterRoleapiVersion: rbac.authorization.k8s.io/v1metadata:  name: agg-cluster-roleaggregationRule:  clusterRoleSelectors:  - matchLabels:      kubernetes.io/bootstrapping: rbac-defaultsrules: []
```

- 확인

```
kubectl get clusterrole cluster-admin -o yamlapiVersion: rbac.authorization.k8s.io/v1kind: ClusterRolemetadata:  annotations:    rbac.authorization.kubernetes.io/autoupdate: "true"  creationTimestamp: "2020-04-30T06:02:28Z"  labels:    kubernetes.io/bootstrapping: rbac-defaults  name: cluster-admin  resourceVersion: "38"  selfLink: /apis/rbac.authorization.k8s.io/v1/clusterroles/cluster-admin
```

### 14.2.3 롤바인딩

- 테스트용 유저 생성

```
apiVersion: v1kind: ServiceAccountmetadata:  name: myuser  namespace: default
```

- 클러스터롤 바인딩

```
kind: ClusterRoleBindingapiVersion: rbac.authorization.k8s.io/v1metadata:  name: read-cluster-role-bindingsubjects:- kind: ServiceAccount  name: myuser  namespace: default  apiGroup: ""roleRef:  kind: ClusterRole  name: read-cluster-role  # 미리 만든 클러스터롤을 유저에 바인딩 합니다  apiGroup: rbac.authorization.k8s.io
```

- Aggregation 클러스터 롤 바인딩

```
kind: ClusterRoleBindingapiVersion: rbac.authorization.k8s.io/v1metadata:  name: agg-cluster-role-bindingsubjects:- kind: ServiceAccount  name: myuser  namespace: default  apiGroup: ""roleRef:  kind: ClusterRole  name: agg-cluster-role  # 미리 만든 클러스터롤을 유저에 바인딩 합니다  apiGroup: rbac.authorization.k8s.io
```

## 15. 테인트(Taint) 와 톨러레이션(Tolerations)

- tain : 노드마다 설정 가능 하며, 설정한 노드에는 Pod 가 스케줄 되지 않음
- Toleration: taint를 무시 할 수 있음
- Taint 에는 3가지 종류가 있음

|   |   |
|---|---|
|Taint|설명|
|NoSchedule|toleration이 없으면 pod이 스케쥴되지 않음, 기존 실행되던 pod에는 적용 안됨|
|PreferNoSchedule|toleration이 없으면 pod을 스케줄링안하려고 하지만 필수는 아님, 클러스터내에 자원이 부족하거나 하면 taint가 걸려있는 노드에서도 pod이 스케줄링될 수 있음|
|NoExecute|toleration이 없으면 pod이 스케줄되지 않으며 기존에 실행되던 pod도 toleration이 없으면 종료시킴.|

### 15.1 노드에 Taint 설정하기

### 15.1.1 Taint 설정

```
kubectl taint node worker01.acorn.com key=value1:NoSchedule
```

### 15.1.2 Taint 조회

```
kubectl describe node worker01.acorn.comnode/worker01.acorn.com taintedroot@master:~# kubectl describe node worker01.acorn.comName:               worker01.acorn.comRoles:              <none>Labels:             beta.kubernetes.io/arch=amd64                    beta.kubernetes.io/os=linux                    disk=ssd                    kubernetes.io/arch=amd64                    kubernetes.io/hostname=worker01.acorn.com                    kubernetes.io/os=linuxAnnotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock                    node.alpha.kubernetes.io/ttl: 0                    volumes.kubernetes.io/controller-managed-attach-detach: trueCreationTimestamp:  Wed, 18 Mar 2020 15:48:26 +0000Taints:             key=value1:NoScheduleUnschedulable:      falseLease:  HolderIdentity:  worker01.acorn.com
```

### 15.2 DaemonSet 생성

### 15.2.1 Yaml 파일 작성

파일명 : goapp-daemon.yaml

```
apiVersion: apps/v1kind: DaemonSetmetadata:  name: goapp-damonsetspec:  selector:    matchLabels:      app: goapp-pod  template:    metadata:      labels:        app: goapp-pod    spec:      containers:      - name: goapp-container        image: dangtong/goapp
```

```
kubectl apply -f ./goapp-daemon.yaml
```

### 15.2.2 DaemonSet 생성 확인

```
kubectl get po -o wideNAME                   READY   STATUS    RESTARTS   AGE   IP          NODE                 NOMINATED NODE   READINESS GATESgoapp-damonset-bmwfq   1/1     Running   0          18s   10.38.0.3   worker02.acorn.com   <none>           <none>
```

> Master 노드와 worker01 에는 스케줄링 되지 않았습니다.

- master 노드의 taint 확인

```
kubectl describe node master.acorn.com | grep -i taintTaints:             node-role.kubernetes.io/master:NoSchedule
```

- worker01 노드의 Taint 확인

```
kubectl describe node worker01.acorn.com | grep -i taintTaints:             key=value1:NoSchedule
```

- DaemonSet 삭제

```
kubectl delete ds goapp-damonset
```

### 15.2.3 Toleration 을 적용한 DaemonSet 생성

- Yaml 파일 작성 : goal-daemon-toler.yaml

```
apiVersion: apps/v1kind: DaemonSetmetadata:  name: goapp-damonsetspec:  selector:    matchLabels:      app: goapp-pod  template:    metadata:      labels:        app: goapp-pod    spec:      tolerations:      - key: key        operator: Equal        value: value1        effect: NoSchedule      containers:      - name: goapp-container        image: dangtong/goapp
```

```
kubectl apply -f ./goapp-daemon-toler.yaml
```

- DaemonSet 생성 확인

```
kubectl get po -o wideNAME                   READY   STATUS    RESTARTS   AGE   IP          NODE                 NOMINATED NODE   READINESS GATESgoapp-damonset-74klq   1/1     Running   0          35s   10.38.0.3   worker02.acorn.com   <none>           <none>goapp-damonset-dr4xz   1/1     Running   0          35s   10.40.0.2   worker01.acorn.com   <none>           <none>
```

> Pod 가 Taint 를 무시하고 worker01 에도 생성 된것을 확인 할 수 있습니다.

- 다음 테스트를 위해 DaemonSet 삭제

```
kubectl delete ds goapp-damonset
```

- Mater 노드에도 DaemonSet 생성을 위해 Yaml 파일 수정

```
kubectl describe no master.acorn.com | grep -i taintTaints:             node-role.kubernetes.io/master:NoSchedule
```

> master 노드의 Taint 에는 key 와 effect 만 존재함

- Master 노드에 스케줄링이 가능 하도록 Yaml 파일 작성 : goapp-damon-toler-all.yaml

```
apiVersion: apps/v1kind: DaemonSetmetadata:  name: goapp-damonsetspec:  selector:    matchLabels:      app: goapp-pod  template:    metadata:      labels:        app: goapp-pod    spec:      tolerations:      - key: key        operator: Equal        value: value1        effect: NoSchedule      - key: node-role.kubernetes.io/master        operator: Exists        effect: NoSchedule      containers:      - name: goapp-container        image: dangtong/goapp
```

```
kubectl apply -f ./goapp-daemon-toler-all.yaml
```

- 다음 테스트를 위해 DaemonSet 삭제

```
kubectl delete ds goapp-daemonset
```

### 15.2.4 untaint 명령으로 노드의 Taint 해지하고 스케줄링 하기

- Untaint 설정 하기

```
kubectl describe no worker01.acorn.com | grep -i taintTaints:             key=value1:NoSchedule
```

```
kubectl taint nodes --all key-
```

> 키값 뒤에 “-” 를 붙여 taint 를 해지 합니다. (untaint)

- daemonSet yaml 파일 재수행

```
kubectl apply -f ./goapp-daemonset.yaml
```

- 확인

```
kubectl get po -o wideNAME                   READY   STATUS    RESTARTS   AGE   IP          NODE                 NOMINATED NODE   READINESS GATESgoapp-damonset-lj2z7   1/1     Running   0          14s   10.40.0.2   worker01.acorn.com   <none>           <none>goapp-damonset-vhlq2   1/1     Running   0          15s   10.38.0.3   worker02.acorn.com   <none>           <none>
```

## 16. 노드 패치및 업그레이드를 위한 Cordon 과 Drain 사용하기

### 16.1 Cordon 사용하기

```
kubectl get nodesNAME                                       STATUS   ROLES    AGE   VERSIONgke-cluster-1-default-pool-20e07d73-4ksw   Ready    <none>   28h   v1.14.10-gke.27gke-cluster-1-default-pool-20e07d73-6mr8   Ready    <none>   28h   v1.14.10-gke.27gke-cluster-1-default-pool-20e07d73-8q3g   Ready    <none>   28h   v1.14.10-gke.27
```

- cordon 설정

```
kubectl cordon gke-cluster-1-default-pool-20e07d73-6mr8
```

```
kubectl get nodesNAME                                       STATUS                     ROLES    AGE   VERSIONgke-cluster-1-default-pool-20e07d73-4ksw   Ready                      <none>   28h   v1.14.10-gke.27gke-cluster-1-default-pool-20e07d73-6mr8   Ready,SchedulingDisabled   <none>   28h   v1.14.10-gke.27gke-cluster-1-default-pool-20e07d73-8q3g   Ready                      <none>   28h   v1.14.10-gke.27
```

- 실제로 스케줄링 되지 않는지 확인해보기

```
kubectl run nginx --image=nginx:1.7.8 --replicas=4 --port=80
```

- 스케줄링 확인

```
kubectl get po -o wideNAME                     READY   STATUS    RESTARTS   AGE   IP          NODE   NOMINATED NODE   READINESS GATESnginx-84569d7db5-cgf95   1/1     Running   0          9s    10.4.0.17   gke-cluster-1-default-pool-20e07d73-8q3g   <none>           <none>nginx-84569d7db5-d7cgg   1/1     Running   0          8s    10.4.1.8    gke-cluster-1-default-pool-20e07d73-4ksw   <none>           <none>nginx-84569d7db5-gnbnq   1/1     Running   0          9s    10.4.1.9    gke-cluster-1-default-pool-20e07d73-4ksw   <none>           <none>nginx-84569d7db5-xqn7b   1/1     Running   0          9s    10.4.0.16   gke-cluster-1-default-pool-20e07d73-8q3g   <none>           <none>
```

- uncodon 설정

```
kubectl uncordon gke-cluster-1-default-pool-20e07d73-6mr8
```

```
kubectl get nodes
```

### 16.2 Drain 사용하기

- Drain 설정

```
kubeclt get po -o wideNAME                     READY   STATUS    RESTARTS   AGE     IP          NODE     NOMINATED NODE   READINESS GATESnginx-84569d7db5-cgf95   1/1     Running   0          3m44s   10.4.0.17   gke-cluster-1-default-pool-20e07d73-8q3g   <none>           <none>nginx-84569d7db5-d7cgg   1/1     Running   0          3m43s   10.4.1.8    gke-cluster-1-default-pool-20e07d73-4ksw   <none>           <none>nginx-84569d7db5-gnbnq   1/1     Running   0          3m44s   10.4.1.9    gke-cluster-1-default-pool-20e07d73-4ksw   <none>           <none>nginx-84569d7db5-xqn7b   1/1     Running   0          3m44s   10.4.0.16   gke-cluster-1-default-pool-20e07d73-8q3g   <none>           <none>
```

- Pod 가 존재 하는 노드를 drain 모드로 설정하기

```
kubectl drain gke-cluster-1-default-pool-20e07d73-8q3gerror: unable to drain node "gke-cluster-1-default-pool-20e07d73-8q3g", aborting command...There are pending nodes to be drained: gke-cluster-1-default-pool-20e07d73-8q3gerror: cannot delete DaemonSet-managed Pods (use --ignore-daemonsets to ignore): kube-system/fluentd-gcp-v3.1.1-t6mnn, kube-system/prometheus-to-sd-96fdn
```

- daemonset 이 존재 하는 노드일 경우 옵션 추가 해서 drain 시킴

```
kubectl drain gke-cluster-1-default-pool-20e07d73-8q3g --ignore-daemonsets
```

- empty-dir 삭제 허용

```
kubectl drain gke-cluster-1-default-pool-20e07d73-8q3g --delete-emptydir-data
```

- drian 확인

```
kubectl get po -o wideNAME                     READY   STATUS    RESTARTS   AGE     IP         NODE    NOMINATED NODE   READINESS GATESnginx-84569d7db5-8qrd2   1/1     Running   0          59s     10.4.2.7   gke-cluster-1-default-pool-20e07d73-6mr8   <none>           <none>nginx-84569d7db5-d7cgg   1/1     Running   0          8m20s   10.4.1.8   gke-cluster-1-default-pool-20e07d73-4ksw   <none>           <none>nginx-84569d7db5-gnbnq   1/1     Running   0          8m21s   10.4.1.9   gke-cluster-1-default-pool-20e07d73-4ksw   <none>           <none>nginx-84569d7db5-s6xsm   1/1     Running   0          59s     10.4.2.9   gke-cluster-1-default-pool-20e07d73-6mr8   <none>           <none>
```

> –delete-local-data –force 등의 추가 옵션 있음

- uncordon

```
kubectl drain gke-cluster-1-default-pool-20e07d73-8q3g
```

## 17. Helm 차트 구성 및 사용

### 17.1 Helm 차트 다운로드 및 설치

### 17.1.1 윈도우 설치

- Chocolatey 설치 : PowerShell 을 열어서 아래 명령을 수행 합니다. 이미 google cloud sdk 를 설치 하면서 설치 되었을수 있습니다.

```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

- Helm 설치

```
choco install kubernetes-helm
```

### 17.1.2 Mac 설치

- 수동 설치 방법

```
# helm 다운로드curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3# 실행권한 변경chmod 700 get_helm.sh# helm 설치./get_helm.sh# 버전 확인helm version# Helm Repository 추가helm repo add stable https://charts.helm.sh/stable# Repository 업데이트helm repo update
```

- Brew 설치

```
brew install helm
```

### 17.2 mysql Helm 차트 다운로드 및 구성

- mysql helm 검색

```
helm search repo stable/mysqlNAME                CHART VERSION   APP VERSION DESCRIPTIONstable/mysql        1.6.3           5.7.28      Fast, reliable, scalable, and easy to use open-...stable/mysqldump    2.6.0           2.4.1       A Helm chart to help backup MySQL databases usi...
```

- 피키지 메타 정보 보기

```
helm show chart stable/mysqlapiVersion: v1appVersion: 5.7.28description: Fast, reliable, scalable, and easy to use open-source relational database  system.home: https://www.mysql.com/icon: https://www.mysql.com/common/logos/logo-mysql-170x115.pngkeywords:- mysql- database- sqlmaintainers:- email: o.with@sportradar.com  name: olemarkus- email: viglesias@google.com  name: viglesiascename: mysqlsources:- https://github.com/kubernetes/charts- https://github.com/docker-library/mysqlversion: 1.6.3
```

- mysql helm 차트 설치 및 Deployment

```
helm install stable/mysql --generate-nameAME: mysql-1588321002LAST DEPLOYED: Fri May  1 08:16:55 2020NAMESPACE: defaultSTATUS: deployedREVISION: 1NOTES:MySQL can be accessed via port 3306 on the following DNS name from within your cluster:mysql-1588321002.default.svc.cluster.localTo get your root password run:    MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default mysql-1588321701 -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)To connect to your database:1. Run an Ubuntu pod that you can use as a client:    kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il2. Install the mysql client:    $ apt-get update && apt-get install mysql-client -y3. Connect using the mysql cli, then provide your password:    $ mysql -h mysql-1588321701 -pTo connect to your database directly from outside the K8s cluster:    MYSQL_HOST=127.0.0.1    MYSQL_PORT=3306    # Execute the following command to route the connection:    kubectl port-forward svc/mysql-1588321002 3306    mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}
```

```
helm lsNAME                    NAMESPACE       REVISION        UPDATED                                 STATUS         CHART            APP VERSIONmysql-1588321701        default         1               2020-05-01 17:28:25.322363879 +0900 +09 deployed       mysql-1.6.3      5.7.28
```

- helm 차스 uninstall

```
heml listNAME                    NAMESPACE       REVISION        UPDATED                                 STATUS         CHART            APP VERSIONmysql-1588321701        default         1               2020-05-01 17:28:25.322363879 +0900 +09 deployed       mysql-1.6.3      5.7.28helm uninstall mysql-1588321701release "mysql-1588321701" uninstalled
```

## 18. ArgoCD

### 18.1 ArgoCD 클러스터 설치

### 18.1.1 Push

[![](https://www.notion.soimg/img.png)](https://www.notion.soimg/img.png)

img

### 18.1.2 Pull

[![](https://www.notion.soimg/img_pull.png)](https://www.notion.soimg/img_pull.png)

img

- argocd 설치

```
kubectl create namespace argocdkubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yamlkubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

- argocd cli 설치 (Cloud shell 에서 수행)

```
VERSION=$(curl --silent "https://api.github.com/repos/argoproj/argo-cd/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')sudo curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64sudo chmod +x /usr/local/bin/argocd
```

- 패스워드 알아내기

```
# kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f2kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

- 패스워드 재설정

```
kubectl port-foward <argocd-server-pod-name> -n argocd 8080argocd login localhost:<PORT>argocd account update-passwordkubectl get svc -n argocd
```

> jenkinX , Flux,
> 
> argocd path 는 git repository 내에서 디렉토리 지정 최상위 디렉토리 이면 그냥 .

### 18.2 ArgoCD 유용한 명령어 모음

```
# ArgoCD 로그인 하기argocd login <ArgoCD_URL># Application 리스트 보기argocd app list# Repository 리스트 보기argocd repo list# 연결된 클러스터 리스트 보기argocd cluster list
```

## 20. 최종 연습문제

### 20.1 PHP GuestBook with Redis

### 20.1.1 과제 개요

- 레디스 마스터 구성
- 레디스 슬레이브 구성
- Guestbook 프론트앤드 구성
- 프로트앤드 애플리케이션 서비스로 노출 시키기
- 리포지토리 만들고 ArgoCD 연동하기

### 20.1.2 레디스 마스터 Deployment 로 구성

|   |   |
|---|---|
|항목|값|
|파일명|redis-master-deployment.yaml|
|Deployment name|redis-master|
|Deployment Label|app: redis|
|deployment Selector / pod metadata|app: redis / role: master / tier: backend|
|replica|1|
|Image|k8s.gcr.io/redis:e2e|
|Port|6379|

> kubectl logs -f POD-NAME

### 20.1.3 레디스 마스터 서비스 구성

|   |   |
|---|---|
|항목|값|
|Type|ClusterIP|
|Service 이름|redis-master|
|Service Label|app: redis / role: master / tier: backend|
|port / targetPort|6379 / 6379|
|selector|app: redis / role: master / tier: backend|

### 20.1.4 레디스 슬레이브 Deployment 구성

|   |   |
|---|---|
|항목|값|
|파일명|redis-slave-deployment.yaml|
|Deployment Name|redis-slave|
|Deployment Label|app: redis|
|Deployment matchLabel|app: redis / role: slave / tier: backend|
|Replicas : 2||
|Pod Label|app: redis / role: slave / tier: backend|
|Container name|slave|
|Image|gcr.io/google_samples/gb-redisslave:v3|
|환경변수 : GET_HOSTS_FROM|마스터 노드의 주소로 지정 하되 value=dns 로 설정|
|containerPort|6379|

- 아래와 같이 소스 코드를 보면 GET_HOSTS_FROM 에는 master 의 주소가 들어가야함

```
 $host = 'redis-master';  if (getenv('GET_HOSTS_FROM') == 'env') {    $host = getenv('REDIS_MASTER_SERVICE_HOST');  }
```

### 20.1.5 레디스 슬레이브 서비스 구성

|   |   |
|---|---|
|항목|값|
|Type|ClusterIP|
|Service 이름|redis-slave|
|Service Label|app: redis / role: slave / tier: backend|
|port / targetPort|6379 / 6379|
|selector|app: redis / role: slaver / tier: backend|

### 20.1.6 GuestBook 애플리케이션 Deployment 생성

|   |   |
|---|---|
|항목|값|
|파일명|frontend-deployment.yaml|
|Deployment Name|frontend|
|Deployment Label|app: guestbook|
|Deployment matchLabel|app: guestbook / tier: frontend|
|Replicas : 3||
|Pod Label|app: guestbook / tier: frontend|
|Container name|php-redis|
|Image|gcr.io/google-samples/gb-frontend:v4|
|환경변수 : GET_HOSTS_FROM|마스터 노드의 주소로 지정 하되 value=dns 로 설정|
|containerPort|80|

### 20.1.7 GuestBook 애플리케이션 LoadBalancer 서비스 구성

|   |   |
|---|---|
|항목|값|
|Service name|frontend|
|service Label|app: guestbook / tier: frontend|
|Type|LoadBalancer|
|Port|80|
|selector|app: guestbook / tier: frontend|

### 20.1.8 FrontEnd 앱 스케일링 해보기

replica 를 5개로 스케일링 해보기

## 21. Kata 연습 문제

### 1. 단일 Pod 생성

prodnamespace 라는 이름으로 네임스페이스를 만들고 해당 네임스페이스 내에 nginx 단일 Pod 를 생성하세요 (명령어로)

```
kubectl create namespace prodnamespacekubectl run nginx --image=nginx  -n prodnamespace
```

### 2. 명령어 이용 다중 Pod 생성

nginx:1.7.9 이미지로 복제본이 2개이고 컨테이너 포트가 80으로 개방된 nginx 라는 이름의 Deployment 를 생성 하세요

```
kubectl run nginx-deployment --image=nginx:1.7.8 --replicas=2 --port=80
```

### 2.1 하나의 Pod 를 선택해서 yaml 파일을 출력하세요

```
kubectl get pokubectl get po nginx-8bfd472d32-gjzp8 -o yaml
```

### 2.2 nginx 이미지를 1.7.9 로 업데이트 하고, 업데이트 내용의 이력을 남기세요 (–record)

```
kubectl set image deploy nginx-deployment nginx-deployment=nginx:1.7.9 --record혹은kubectl edit deploy nginx-deloyment
```

### 2.3 nginx 1.7.9 에서 에러가 발생 했다고 가정하고 rollback 하세요

```
kubectl rollout undo deployment/nginx-deployment
```

### 2.4 rollout 히스토리를 확인 하세요

```
kubectl rollout history deployment/nginx-deployment
```

### 2.5 rivision 번호를 이용해 1.7.9 로 rollout undo 하세요

```
kubectl rollout undo deployment/nginx-deployment --to-revision=2
```

### 2.6 nginx Pod 에 app=v1 이라는 라벨을 입력 하세요

```
kubectl label deploy nginx-deployment app=v1
```

### 3. BGD(Blue Green Deployment) 구현하기

### 3.1 nginx 앱을 로드밸런싱 해줄 Loadbalancer 를 구현 하세요

```
apiVersion: v1kind: Servicemetadata:  name: nginx-lbspec:  selector:    app: v1  ports:    - protocol: TCP      port: 80      targetPort: 80  type: LoadBalancer
```

### 3.2 nginx 의 Loadbalancer External-IP 를 확인하고 curl 명령어로 테스트 해보세요

```
kubectl get svcNAME         TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)        AGEkubernetes   ClusterIP      10.8.0.1     <none>           443/TCP        30hnginx-lb     LoadBalancer   10.8.7.22    35.184.195.100   80:30833/TCP   38s
```

```
curl http://35.184.195.100
```

# Appendix

## Appendix 0. VirtualBox 이미지 구성

### 1. VirtualBox 다운로드 및 설치

다운로드 : [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

[![](https://www.notion.so./img/virtualbox-down-3549200.png)](https://www.notion.so./img/virtualbox-down-3549200.png)

virtualbox-down

### 2. 가상 이미지 가져오기

- 구글 드라이버 URL

https://drive.google.com/open?id=1qAv-FyIWbQVIxNn3MKfjIwbVdvo4rxVl

### 3. 가상 이미지 가져오기

버추얼 박스 실행 후에  파일  > 가상이미지 가져오기 클릭

[![](https://www.notion.so../../sas_book/img/vbox-choose-image.png)](https://www.notion.so../../sas_book/img/vbox-choose-image.png)

vbox-choose-image

### 4. VM 설정hisotyr

vm 이름, CPU 개수, MAC 주소 정책 을 아래와 같이 수정 합니다.

[![](https://www.notion.so./img/vbox-config.png)](https://www.notion.so./img/vbox-config.png)

vbox-config

### 5. VM 복제

오른쪽마우스 -> 복제 클릭

[![](https://www.notion.so./img/vbox-copy.png)](https://www.notion.so./img/vbox-copy.png)

vbox-copy

VM이름 및 MAC주소정책 변경 후 복제 클릭

[![](https://www.notion.so./img/vbox-copy-config.png)](https://www.notion.so./img/vbox-copy-config.png)

vbox-copy-config

동일한 방법으로 worker02 VM 또 생성 합니다. 최종적으로 아래와 같이 3개의 VM 이 보이면 됩니다.

[![](https://www.notion.so./img/vbox-overall.png)](https://www.notion.so./img/vbox-overall.png)

vbox-overall

복제된 vm 에 접속하여 /etc/netplan/00-installer-config.yaml 파일을 수정 합니다.

```
network:  ethernets:    enp0s3:      dhcp4: no      addresses: [10.0.2.17/24]  ## IP 수정 (VM 마다 하나씩 IP 증가)      gateway4: 10.0.2.2      nameservers:              addresses: [8.8.8.8, 8.8.4.4]    enp0s8:      dhcp4: no      addresses: [192.168.56.110/24] ## IP 수정 (VM 마다 하나씩 IP 증가)  version: 2
```

수정 후에는 netplan 명령어로 변경된 IP 로 적용

```
netplan apply
```

## Appendix 1. 쿠버네티스 설치

### 1.설치 사전 작업

### 1.1 Nftable disable

```
sudo update-alternatives --set iptables /usr/sbin/iptables-legacysudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacysudo update-alternatives --set arptables /usr/sbin/arptables-legacysudo update-alternatives --set ebtables /usr/sbin/ebtables-legacy
```

### 1.2 호스트명 변경

```
# hostnamectl set-hostname [FQDN-HOST-NAME] --static# 마스터 노드에서 수행hostnamectl set-hostname kmasterhostnamectl set-hostname kmaster.acorn.com --static# 첫번째 Worker 노드에서 수행hostnamectl set-hostname kworker01hostnamectl set-hostname kworker01.acorn.com --static# 두번째 Worker 노드에서 수행hostnamectl set-hostname kworker02hostnamectl set-hostname kworker02.acorn.com --static
```

### 1.3 각노드에 호스트 파일 등록

모든노드의 /etc/hosts 파일에 아래와 같이 등록 되어 있을것

```
192.168.56.111 kmaster.acorn.com kmaster192.168.56.112 kworker01.acorn.com kworker01192.168.56.113 kworker02.acorn.com kworker02
```

### 1.4 방화벽 점검

### 1.4.1 Control Plane 노드

|   |   |   |   |   |
|---|---|---|---|---|
|Protocol|Direction|Port Range|Purpose|Used By|
|TCP|Inbound|6443*|Kubernetes API server|All|
|TCP|Inbound|2379-2380|etcd server client API|kube-apiserver, etcd|
|TCP|Inbound|10250|Kubelet API|Self, Control plane|
|TCP|Inbound|10251|kube-scheduler|Self|
|TCP|Inbound|10252|kube-controller-manager|Self|

### 1.4.2 worker 노드

|   |   |   |   |   |
|---|---|---|---|---|
|Protocol|Direction|Port Range|Purpose|Used By|
|TCP|Inbound|10250|Kubelet API|Self, Control plane|
|TCP|Inbound|30000-32767|NodePort Services**|All|

### 2. Runtime 설치

### 2.1 도커 설치 (all nodes)

/var/lib/dpkg/lock-frontend 락이 발생 한다면 현재 백그라운드에서 자동으로 보안 업데이트가 수행 중이기 때문입니다. 잠시 (10분) 기다렸다 다시 시도 하면 됩니다.

```
sudo apt-get updatesudo apt-get install docker.io
```

> apt repository 가 유효하지 않다는 메시지가 뜬다면, timezone 문제 일 가능성이 많습니다. 아래와 같이 하드웨어 시간으로 부터 시스템 시간을 설정 한뒤 다시 시도해 보세요
> 
> ```
> sudo hwclock --hctosysrdate -s time.bora.nethwclock -w
> ```
> 
> 사용자 계정에서 docker 명령 사용시 sudo 를 쓰고 싶지 않을 때는 아래와 같이 docker 그룹에 사용자 계정을 추가 해주면 됩니다. 로그인을 다시해야 정상적으로 동작 합니다.
> 
> ```
> sudo usermod -aG docker kubeadmin
> ```

### 2.2 kubernetes 설치 (all nodes)

설치 스크립트 [클릭](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#before-you-begin)

```
# Ubuntu 패키지 업데이트 및 apt-transport-https 및 curl 설치sudo apt-get update && sudo apt-get install -y apt-transport-https curl# 구글에서 제공하는 k8s 패키지 설치를 위해 apt key 추가curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -# 쿠버네티스 리포지토리를 apt 소스 리스트에 추가cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.listdeb https://apt.kubernetes.io/ kubernetes-xenial mainEOF# Ubuntu 패키지 업데이트 (의존성 추가 확인)sudo apt-get update# K8s 설치sudo apt-get install -y kubelet kubeadm kubectl# apt 마크 수행함. 패키지가 자동으로 설치/업그레이드/삭제 되지 않게 함sudo apt-mark hold kubelet kubeadm kubectl
```

### 3. Post Installation

### 3.1 SWAP 기능 해제(all nodes)

```
sudo swapoff -a # 현재 일시적용sudo sed -i '/swap/s/^/#/' /etc/fstab # 재가동 후에도 swapoff 할려면 수행 해야함
```

### 3.2 도커 시스템서비스 등록 하기 (all nodes)

도커 데몬을 system 서비스에 등록하지 않으면 kubernetes 설치시에 warning 발생 하기 때문에 아래와 같이 등록해줌

```
sudo vi /etc/docker/daemon.json
```

아래와 같이 입력 후 저장 합니다.

```
{  "exec-opts": ["native.cgroupdriver=systemd"],  "log-driver": "json-file",  "log-opts": {    "max-size": "100m"  },  "storage-driver": "overlay2"}
```

```
sudo mkdir -p /etc/systemd/system/docker.service.d# Restart docker.sudo systemctl enable docker.servicesudo systemctl daemon-reloadsudo systemctl restart docker
```

### 3.3 kubeadm을 사용하여 마스터 설정 (master node)

네트워크 인터페이스가 여러개일 경우 가장 우선 순위가 가장 높은 인터페이스가 자동 선정 되기 때문에 ‘–apiserver-advertise-address’ 옵션을 사용해서 ip 혹은 인터페이스 명을 지정해야 합니다.

```
sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=192.168.56.111# kubeadmin init 시에 여러 가지 옵션들# --pod-network-cidr=192.168.0.0/16# --control-plane-endpoint=192.168.56.111# --apiserver-cert-extra-sans=192.168.56.111# --service-cidr 10.96.0.0/12# --service-dns-domain "k8s"# --apiserver-advertise-address __YOUR_IP_HERE___# 잘못 설치한 경우 master 및 worker 노드에 수행 하면 모든 설정을 초기화함# kubeadm reset cleanup-node
```

### 4. 워커 노드 등록

### 4.1 워커 노드 등록 (worker node only)

> 반듯이 kubeadm init 을 통해 나온 로그에 표시된 ip 와 token 으로 worker 노드에서만 수행

```
sudo kubeadm join 192.168.56.111:6443 --token dro5or.x9ujk49k0aadiz8s \    --discovery-token-ca-cert-hash sha256:b6372f30c5733a5c88d0a6b6095efbf4cb495329cbcd19df33e7a894a96ec53d
```

### 4.2 마스터 노드 kube client 설정 (master node)

```
mkdir -p $HOME/.kubesudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/configsudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 4.3 클러스터 상태 확인(master node)

결과가 모두 not ready 로 나옴 (시간이 좀 걸림)

```
kubectl get nodes
```

### 4.4 CNI PlugIn 설치 (master node)

CNI PlugIn 설치 후에는 모든 노드가 ready 로 나와야 함

```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"kubectl get nodes
```

> CNI (Container Network Interface) 는 K8s 클러스터내에 Pod 들간의 네트워크를 만들기 위한 인터페이스 입니다. CNI 를 이용한 다양한 PlugIn 들이 존재 합니다. 여기서는 여러 PlugIn 중의 하나인 WeaveNet 을 사용합니다.
> 
> PlugIn 리스트 참조 : https://github.com/containernetworking/cni
> 
> CNI 란 무엇인가? : https://www.youtube.com/watch?v=zmYxdtFzK6s

### 4.5 kubectl 명령어 자동완성 패키지 설치(master node)

- linux (bash)

```
# 자동 완성을 bash 셸에 영구적으로 추가한다echo "# k8s auto completion and alias" >> ~/.bashrcecho "source <(kubectl completion bash)" >> ~/.bashrcecho "alias k=kubectl"  >> ~/.bashrcecho "complete -F __start_kubectl k" >> ~/.bashrc
```

- macOS(zsh)

```
source <(kubectl completion zsh)echo 'alias k=kubectl' >>~/.zshrcecho 'complete -F __start_kubectl k' >>~/.zshrc
```

- Windows(powerShell)

### 5. etcd 설치(master node)

기본적으로 내장 etcd 가 있지만, etcd-io 의 릴리즈로 설치 합니다.

다운로드 url : [클릭](https://github.com/etcd-io/etcd/releases)

```
# 다운로드wget https://github.com/etcd-io/etcd/releases/download/v3.3.18/etcd-v3.3.18-linux-amd64.tar.gz# 압축 풀기tar -xf etcd-v3.3.18-linux-amd64.tar.gz# 설치 디렉토리로 이동cd etcd-v3.3.18-linux-amd64# 설치 수행sudo ETCDCTL_API=3 ./etcdctl --endpoints 127.0.0.1:2379 \--cacert /etc/kubernetes/pki/etcd/ca.crt \--cert /etc/kubernetes/pki/etcd/server.crt \--key /etc/kubernetes/pki/etcd/server.key \get / --prefix --keys-only
```

## Appendix 2. 구글 GKE 사용하기

### 1. 회원가입

- $300 무료 크레딧 제공
- 신용카드 필요 (한도를 넘으도 자동 과금 안됨)
- http://cloud.google.com 에서 회원 가입

### 2. 프로젝트 생성

- 프로젝트 선택 을 클릭 합니다.

[![](https://www.notion.so./img/GKE-project-int.png)](https://www.notion.so./img/GKE-project-int.png)

프로젝트 선택

- 새 프로젝트 를 클릭 합니다.

[![](https://www.notion.so./img/GKE-project-choose.png)](https://www.notion.so./img/GKE-project-choose.png)

- 프로젝트 이름에 원하는 값을 입력하고 클릭
    
    만들기
    

[![](https://www.notion.so./img/GKE-new-project.png)](https://www.notion.so./img/GKE-new-project.png)

GKE-new-project

### 3. 쿠버네티스 클러스터 생성

- 홈메뉴 에서 클릭
    
    Kubernetes Engine
    

[![](https://www.notion.so./img/GKE-ngine-enter.png)](https://www.notion.so./img/GKE-ngine-enter.png)

GKE-ngine-enter

- 프로젝트를 선택하기 위해 클릭
    
    프로젝트 선택
    

[![](https://www.notion.so./img/GKE-choose-project-for-create-cluster.png)](https://www.notion.so./img/GKE-choose-project-for-create-cluster.png)

GKE-choose-project-for-create-cluster

- 팝업창이 뜨면 프로젝트 gke-first 를 선택하고 클릭
    
    열기
    

[![](https://www.notion.so./img/GKE-open-project.png)](https://www.notion.so./img/GKE-open-project.png)

GKE-open-project

- 1분정도 기다린 후에 클릭
    
    클러스터 만들기
    

[![](https://www.notion.so./img/GKE-create-cluster.png)](https://www.notion.so./img/GKE-create-cluster.png)

- 클러스터 기본사항 에서 영역을 “asia-east1-a” 로 선택 한후, 클릭
    
    만들기
    

[![](https://www.notion.so./img/GKE-create-cluster-1.png)](https://www.notion.so./img/GKE-create-cluster-1.png)

GKE-create-cluster-1

- 아래와 같이 k8s 클러스터를 생성 중인 화면이 나오면 정상 적으로 진행이 된 것입니다.

[![](https://www.notion.so./img/GKE-cluster-complete.png)](https://www.notion.so./img/GKE-cluster-complete.png)

GKE-cluster-complete

### 4. 쿠버네티스 클러스터 접속 테스트

- GKE 클러스터가 생성되면 클릭
    
    연결
    

[![](https://www.notion.so./img/GKE-cluster-connect-1.png)](https://www.notion.so./img/GKE-cluster-connect-1.png)

GKE-cluster-connect-1

- 팝업창이 뜨면 클릭
    
    Cloud Shell에서 실행
    

[![](https://www.notion.so./img/GKE-cluster-connect-2.png)](https://www.notion.so./img/GKE-cluster-connect-2.png)

GKE-cluster-connect-2

## Appendix 4. Google Cloud SDK 설치 하기

### 1. GCloud SDK 다운로드

[![](https://www.notion.so./img/cloud_sdk_1.png)](https://www.notion.so./img/cloud_sdk_1.png)

cloud_sdk_1

### 2. Cloud SDK 설치

[![](https://www.notion.so./img/cloud_sdk_2.png)](https://www.notion.so./img/cloud_sdk_2.png)

cloud_sdk_2

### 3. 로컬 머신에서 Cloud Shell 연결 명령어 복사

[![](https://www.notion.so../img/cloud_sdk_3.png)](https://www.notion.so../img/cloud_sdk_3.png)

[![](https://www.notion.so./img/cloud_sdk_4.png)](https://www.notion.so./img/cloud_sdk_4.png)

cloud_sdk_4

### 4. 로컬 CMD 또는 SHELL 에서 연결 하기

웹에서 복사한 내용을 그대로 붙여놓고 실행

[![](https://www.notion.so./img/cloud_sdk_5.png)](https://www.notion.so./img/cloud_sdk_5.png)

cloud_sdk_5

## Appendix 3. Git 계정 생성 및 Sync

### 1.Git 계정생성

www.github.com 에서 계정 생성

### 2. Git 리포지토리 생성

- GitHub.com 에 자신의 계정으로 로그인하여 아래 화면과 같이 리포지토리 생성

[![](https://www.notion.so./img/git_repo_create.png)](https://www.notion.so./img/git_repo_create.png)

- 리포지토리 이름을 아래와 같이 입력하고. 생성 합니다. (Repository name : k8s-web)

[![](https://www.notion.so./img/git_repo_create_spec.png)](https://www.notion.so./img/git_repo_create_spec.png)

git_repo_create_spec

### 3. 소스 파일 작성

```
$ mkdir -p ./gitvolumn/html$ mkdir -p ./gitvolumn/kubetmp$ cd ./gitvolumn/html$ vi index.html
```

index.html 이름으로 아래 페이지를 작성 합니다.

```
<!DOCTYPE html><html><body><h1>K8s Landing Page</h1><p>Hello Kubernetes !!!</p></body></html>
```

### 4. Git 리포지토리 생성 및 초기화

```
$ git init$ git add .$ git commit -a -m "first commit"$ git remote add origin https://github.com/<계정명>/k8s-web.git$ git remote -v$ git push origin master$ git status
```