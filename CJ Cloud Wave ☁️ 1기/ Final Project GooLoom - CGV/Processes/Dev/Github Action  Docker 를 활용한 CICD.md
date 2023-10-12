  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-27_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.18.34.png]]

  

```
name: Gooloom-CGV-WAS - CI with Gihub Actions, Dockeron:  push:    paths:      - 'gooloom-was/**'      - 'gooloom-web/**'    branches: [ main ]jobs:  deploy:    runs-on: ubuntu-latest    steps:      - name: 저장소 Checkout        uses: actions/checkout@v3      - name: Set up JDK 19        uses: actions/setup-java@v3        with:          java-version: '19'          distribution: 'oracle'      - name: 스프링부트 애플리케이션 빌드 # (1)        run: |          cd gooloom-was          sudo chmod +x gradlew          ./gradlew build        shell:          bash                # - name: Set Date as Image Tag      #   id: set_tag      #   run: echo "IMAGE_TAG=$(date +'%Y%m%d%H%M%S')" >> $GITHUB_ENV      - name: Docker Hub 로그인 # (3)        uses: docker/login-action@v2        with:          username: ${{ secrets.DOCKERHUB_USERNAME }}          password: ${{ secrets.DOCKERHUB_TOKEN }}            - name: 도커 이미지 빌드 # (2)        run: |          cd gooloom-was          docker build -t csm4903/gooloom-was:latest .      - name: Docker Hub 퍼블리시 # (4)        run: docker push csm4903/gooloom-was:latest
```

  

  

# 전체 CI / CD 과정

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-30_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.41.31.png]]

- CI / CD 진행 전 초기 홈페이지의 모습. “CICD Test”

# CI

- Git Push - Github Action - Docker build - Docker image Push

  

## 1. Git Push

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-30_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.29.52.png]]

  

## 2. Github Action - Docker image build & Docker Hub Push

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-30_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.31.10.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-30_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.32.20.png]]

  

# CD

- Git push - K8s Manifest - ArgoCD - EKS

## 1. K8s Manifest 작성 및 Git Push

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-30_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.33.12.png]]

- Docker Image의 버전을 `latest` 로 설정했기 때문에 `ImagePullPolicy` 를 `"Always"` 로 설정하여 매 Pod가 뜰 때마다 Docker Hub에서 `latest` tag를 가진 이미지를 새로 다운 받도록 해준다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-30_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.37.53.png]]

- Github 에 Push 하게 되면 해당 repository를 보고 있던 ArgoCD가 Webhook 을 통해 k8s manifest 파일의 변경사항을 확인하고 최신 이미지를 모든 Pod에 Rolling Update 방식으로 무중단 배포한다.

  

## CI / CD 후 변동 사항에 대해 정상적으로 배포된 애플리케이션의 모습

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-30_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.44.10.png]]