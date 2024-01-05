# Node.js 및 NPM 설치 가이드

## 1. Node 및 NPM 설치

### 1.1 Mac

- node 설치

```
brew updatebrew install node
```

- 설치 확인

```
node -vnpm -v
```

- 업그레이드

```
brew upgrade node
```

- 삭제

```
brew uninstall node
```

### 1.2 Windows

- node / npm 설치 : https://nodejs.org/ko/download/

[![](https://www.notion.so/Users/dangtongbyun/Dropbox/05.Lecture/01.Kubernetes/reactWithNodes/img/image-20210610190515021.png)](https://www.notion.so/Users/dangtongbyun/Dropbox/05.Lecture/01.Kubernetes/reactWithNodes/img/image-20210610190515021.png)

image-20210610190515021

## 2. Project Setup

### 2.1 React Project Setup

```
# 수행하기 전에 vscode restart 해야함mkdir cloudnativecd cloudnativenpx create-react-app frontendnpm install axios# 테스트npm start
```

Bootstrap 사용을 위해 Public 디렉토리 밑에 index.html 에 다음 내용을 Copy

```
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
```

### 2.2 Node.js(Typescript) Project Setup

```
# cloudNative 디렉토리 밑에서 ....mkdir getmemmkdir nodecheckmkdir sayservicecd getmemnpm init -ynpm install typescript -gnpm install typescript ts-node-dev express @types/express cors @types/cors# vscode restart 해야함tsc --initcd nodechecknpm init -ynpm install typescript ts-node-dev express @types/express cors @types/corstsc --initcd sayservicenpm init -ynpm install typescript ts-node-dev express @types/express cors @types/corstsc --init
```

## 3. Source Code 작성

### 3.1 getmem 서비스

```
# cloudNative/getmem 디렉토리에서mkdir srcmkdir -p src/module
```

### 3.1.1 src/Index.ts 작성

```
import express from 'express';import cors from 'cors';import { getmemRouter } from './module/getmem';const app = express();app.use(express.json());app.use(cors());app.use(getmemRouter);app.all('*', async (req, res) => {  res.send({});});app.listen(3001, () => {  console.log('getcpu app started. listen on 3001 port.');})
```

### 3.1.2 src/module/getmem.ts 작성

```
import express, { Request, Response }from 'express';import os from 'os';const router = express.Router();const meminfo = os.totalmem();router.get('/api/getmem', (req: Request, res: Response) => {  console.log(meminfo);  res.send({meminfo});});export { router as getmemRouter }
```

### 3.1.3 Package.json scripts 부분 수정

```
"start": "ts-node-dev src/index.ts"
```

### 3.2 nodecheck 서비스

```
# cloudNative/nodecheck 디렉토리에서mkdir srcmkdir -p src/module
```

### 3.2.1 Src/index.ts 작성

```
import express from 'express';import cors from 'cors';import { checkstatusRouter } from './module/nodecheck';const app = express();app.use(express.json());app.use(cors());app.use(checkstatusRouter);app.all('*', async (req, res) => {  res.send({});});app.listen(3002, () => {  console.log('checkstatus app started. listen on 3002 port.');})
```

### 3.2.2 src/module/nodecheck.ts 작성

```
import express, { Request, Response }from 'express';import os from 'os';const router = express.Router();const message = "this is app1. you've hit " + os.hostname() + "\n";router.get('/api/nodecheck', (req: Request, res: Response) => {  console.log(message);  res.send({message});});export { router as checkstatusRouter }
```

### 3.2.3 Package.json scripts 부분 수정

```
"start": "ts-node-dev src/index.ts"
```

### 3.3 sayservice 작성

```
# cloudNative/sayservice 디렉토리에서mkdir srcmkdir -p src/module
```

### 3.3.1 src/index.ts 작성

```
import express from 'express';import cors from 'cors';import { sayhelloRouter } from './module/sayservice';const app = express();app.use(express.json());app.use(cors());app.use(sayhelloRouter);app.all('*', async (req, res) => {  res.send({});});app.listen(3003, () => {  console.log('sayhello app started. listen on 3003 port.');})
```

### 3.3.2 src/module/sayservice.ts 작성

```
import express, { Request, Response }from 'express';import os from 'os';const router = express.Router();const message = "hi my name is sayhello application \n";router.get('/api/sayservice', (req: Request, res: Response) => {  console.log(message);  res.send({message});});export { router as sayhelloRouter }
```

### 3.3.3 Package.json scripts 부분 수정

```
"start": "ts-node-dev src/index.ts"
```

### 3.4 frontend 서비스 작성 (React)

### 3.4.1 src 밑의 모든 파일 삭제

### 3.4.2 src/Index.js

```
import React from 'react';import ReactDOM from 'react-dom';import App from './App';ReactDOM.render(  <App />,  document.getElementById('root'));
```

### 3.4.2 src/GetMem.js

```
import React, { useState, useEffect } from 'react';import axios from 'axios';export function GetMem ()  {  const [memInfo, setMemInfo] = useState([]);  const getmemUrl = process.env.REACT_APP_GETMEM_URL || 'http://localhost:3001/api/getmem';  const fetchMemInfo = async () => {    const res = await axios.get(getmemUrl);    setMemInfo(res.data);  };  useEffect(() => {    fetchMemInfo();  }, []);  const renderedMemInfo = Object.values(memInfo).map((mem,i) => {    console.log(mem);    return (    <div      className="card"      style={{ width: '30%', marginBottom: '20px'}}      key={i}    >      <div className='card-body'>        <h3>{mem / 1024 / 1024 /1024} GB</h3>      </div>    </div>    );    });  return (    <div className="d-flex flex-row flex-wrap justify-content-between">      {renderedMemInfo}    </div>  );};
```

### 3.4.3 src/NodeCheck.js

```
import React, { useState, useEffect } from 'react';import axios from 'axios';export function NodeCheck ()  {  const [nodeInfo, setNodeInfo] = useState([]);  const nodecheckUrl = process.env.REACT_APP_NODECHECK_URL || 'http://localhost:3002/api/nodecheck';  console.log('REACT_APP NodeCheckURL: ' + process.env.REACT_APP_NODECHECK_URL);  const fetchNodeInfo = async () => {    const res = await axios.get(nodecheckUrl);    setNodeInfo(res.data);  };  useEffect(() => {    fetchNodeInfo();  }, []);  const renderedNodeInfo = Object.values(nodeInfo).map((node,i) => {    console.log(node);    return (    <div      className="card"      style={{ width: '30%', marginBottom: '20px'}}      key={i}    >      <div className='card-body'>        <h3>{node}</h3>      </div>    </div>    );    });  return (    <div className="d-flex flex-row flex-wrap justify-content-between">      {renderedNodeInfo}    </div>  );};
```

### 3.4.4 src/SayService.js

```
import React, { useState, useEffect } from 'react';import axios from 'axios';export function SayService ()  {  const [sayInfo, setSayInfo] = useState([]);  const sayserviceUrl = process.env.REACT_APP_SAYSERVICE_URL || 'http://localhost:3003/api/sayservice';  const fetchSayInfo = async () => {    const res = await axios.get(sayserviceUrl);    setSayInfo(res.data);  };  useEffect(() => {    fetchSayInfo();  }, []);  const renderedSayInfo = Object.values(sayInfo).map((say,i) => {    console.log(say);    return (    <div      className="card"      style={{ width: '30%', marginBottom: '20px'}}      key={i}    >      <div className='card-body'>        <h3>{say}</h3>      </div>    </div>    );    });  return (    <div className="d-flex flex-row flex-wrap justify-content-between">      {renderedSayInfo}    </div>  );};
```

### 3.4.5 src/App.js

```
import React from 'react';import { GetMem } from './GetMem';import { NodeCheck } from './NodeCheck';import { SayService } from './SayService';const App = () => {  return <div>    <h1>MemoryInfo</h1>    <GetMem />    <h1>node status</h1>    <NodeCheck />    <h1>Say Service</h1>    <SayService />  </div>;};export default App;
```

## 4. Dockerfile Setup

### 4.1 getmem

- Dockerfile

```
FROM node:alpineWORKDIR /appCOPY package.json .RUN npm installCOPY . .CMD ["npm", "start"]
```

- .dockerignore 파일을 아래와 같이 작성

```
node_modules
```

- .gitignore

```
node_modules
```

### 4.2 nodecheck

- Dockerfile

```
FROM node:alpineWORKDIR /appCOPY package.json .RUN npm installCOPY . .CMD ["npm", "start"]
```

- .dockerignore 파일을 아래와 같이 작성

```
node_modules
```

- .gitignore

```
node_modules
```

### 4.3 sayservice

- Dockerfile 작성

```
FROM node:alpineWORKDIR /appCOPY package.json .RUN npm installCOPY . .CMD ["npm", "start"]
```

- .dockerignore 파일을 아래와 같이 작성

```
node_modules
```

- .gitignore

```
node_modules
```

### 4.4 frontend

- Dockerfile 작성

```
FROM node:alpineWORKDIR /appENV PATH /app/node_modules/.bin:$PATHCOPY package.json .COPY package-lock.json .RUN npm install --silentCOPY . .CMD ["npm","start","dev"]
```

- .dockerignore 작성

```
node_modulesbuild
```

- .gitignore

```
node_modulesbuild
```

## 5. Yaml 파일 작성

```
mkdir -p /xinfra/kubernetes
```

- getmem Service (getmem-deploy.yaml)

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: getmem-deployspec:  replicas: 1  selector:    matchLabels:      app: getmem  template:    metadata:      labels:        app: getmem    spec:      containers:      - name: getmem-container        image: dangtong76/getmem---apiVersion: v1kind: Servicemetadata:  name:  getmem-srvspec:  selector:    app: getmem  ports:  - name: getmem    protocol: TCP    port:  3001    targetPort:  3001
```

- Node check Service (nodecheck-deploy-yaml)

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: nodecheck-deployspec:  replicas: 1  selector:    matchLabels:      app: nodecheck  template:    metadata:      labels:        app: nodecheck    spec:      containers:      - name: nodecheck-container        image: dangtong76/nodecheck---apiVersion: v1kind: Servicemetadata:  name:  nodecheck-srvspec:  selector:    app: nodecheck  ports:  - name: nodecheck    protocol: TCP    port:  3002    targetPort:  3002
```

- Say service (sayservice-deploy.yaml)

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: sayservice-deployspec:  replicas: 1  selector:    matchLabels:      app: sayservice  template:    metadata:      labels:        app: sayservice    spec:      containers:      - name: sayservice-container        image: dangtong76/sayservice---apiVersion: v1kind: Servicemetadata:  name: sayservice-srvspec:  selector:    app: sayservice  ports:  - name: sayservice    protocol: TCP    port:  3003    targetPort:  3003
```

- frontend Service (frontend-deploy.yaml)

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: frontend-deplspec:  replicas: 1  selector:    matchLabels:      app: frontend  template:    metadata:      labels:        app: frontend    spec:      containers:        - name: frontend-container          image: dangtong76/frontend          env:            - name: REACT_APP_GETMEM_URL              value: 'http://www.acorn.com/api/getmem'            - name: REACT_APP_NODECHECK_URL              value: 'http://www.acorn.com/api/nodecheck'            - name: REACT_APP_SAYSERVICE_URL              value: 'http://www.acorn.com/api/sayservice'---apiVersion: v1kind: Servicemetadata:  name: frontend-srvspec:  selector:    app: frontend  ports:    - name: frontend      protocol: TCP      port: 3000      targetPort: 3000
```

- Ingress Service (ingress.yaml)

```
apiVersion: networking.k8s.io/v1kind: Ingressmetadata:  name: ingress-servicespec:  rules:    - host: www.acorn.com      http:        paths:          - path: /api/getmem            pathType: ImplementationSpecific            backend:              service:                name: getmem-srv                port:                  number: 3001          - path: /api/nodecheck            pathType: ImplementationSpecific            backend:              service:                name: nodecheck-srv                port:                  number: 3002          - path: /api/sayservice            pathType: ImplementationSpecific            backend:              service:                name: sayservice-srv                port:                  number: 3003          - path: /*            pathType: ImplementationSpecific            backend:              service:                name: frontend-srv                port:                  number: 3000
```

## 6. SKaffold Setup

skaffold dev : continuous build & deploy on code changes

skaffold run : build & deploy once

### 6.1 Manifest 침조 링크

https://skaffold.dev/docs/references/yaml/\#deploy-helm

### 6.2 MAC(setup)

```
# Brew installbrew install skaffold# For macOS on AMD64curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-darwin-amd64 && \sudo install skaffold /usr/local/bin/# For macOS on ARM64curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-darwin-arm64 && \sudo install skaffold /usr/local/bin/# MacPorts installsudo port install skaffold
```

### 6.3 Windows(setup)

다운로드 링크 : https://storage.googleapis.com/skaffold/releases/latest/skaffold-windows-amd64.exe

> 다운로드 후에 시스템 PATH 환경변수에 scaffold.exe 로 이름을 변경해서 파일의 경로를 넣어 주어야 함.
> 
> 예) C:
> 
> 파일의 이름도 skaffold.exe 로 변경 하는 것이 직관성을 위해 좋습니다.

- choco 를 이용한 설치

```
choco install -y skaffold
```

### 6.4 skaffold yaml 파일 작성

> https://skaffold.dev/docs/references/yaml/

```
apiVersion: skaffold/v2beta28kind: Config# 쿠버네티스 배포 설정deploy:  kubectl:    manifests:      - ./xinfra/kubernetes/*    defaultNamespace: devel# 도커 이미지 빌드 관련 설정build:  local:    push: true  # googleCloudBuild:  #   projectId: kubernetes-315817  artifacts:    - image: dangtong76/getmem      context: getmem      docker:        dockerfile: Dockerfile    - image: dangtong76/nodecheck      context: nodecheck      docker:        dockerfile: Dockerfile    - image: dangtong76/sayservice      context: sayservice      docker:        dockerfile: Dockerfile    - image: dangtong76/frontend      context: frontend      docker:        dockerfile: Dockerfile      # 소스 싱크 관련 설정      sync:        manual:          - src: 'src/**/*.ts'            dest: .
```

> https://skaffold.dev/docs/references/yaml/\#deploy-helm 참조

### 6.5 skaffold 수행

### 6.5.1 네임스페이스 생성

```
kubectl create ns devel
```

### 6.5.2 skaffold 적용

```
skaffold dev
```

## 7. /etc/hosts 파일 변경

```
kubectl get ingress# macsudo vi /etc/hosts34.56.123.234 acorn.com# windowshosts 파일 수정34.56.123.234 acorn.com
```

## 8. git setup

### 8.1 Git Repo 연동

```
git init
```

```
# git ignore 목록buildnode_modulesnode_modulesxinfra.DS_Store.vscode
```

### 8.2 Git Workflow Setup

### 8.2.1 Docker 계정 정보를 Git 에 설정

Settings -> Secret -> New repository Secret 메뉴에서 생성

DOCKER_PASSWORD / DOCKER_USERNAME 두가지 생성

### 8.2.1 Git Action 설정

```
on:  push:    branches:      - mybranch    paths:      - 'getmem/**'jobs:  build:    runs-on: ubuntu-latest    steps:      - uses: actions/checkout@v2      - run: cd getmem && docker build -t dangtong76/getmem .      - run: docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD        env:          DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}          DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}      - run: docker push dangtong76/getmem
```