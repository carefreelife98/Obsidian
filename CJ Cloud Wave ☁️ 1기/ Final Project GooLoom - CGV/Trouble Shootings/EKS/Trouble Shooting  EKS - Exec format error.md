  

원인 : Docker Image Build 시 사용된 운영체제와 이미지를 사용하는 운영체제 간의 충돌

현재 m1의 과도기 상황에서 m1으로 빌드한 이미지를  
서버가 arm 운영체제인 상황에서 돌리려고 할 떄 나는 에러입니다.

이 경우 이미지 빌드시 멀티 플랫폼 빌드로 이미지를 빌드하여 해결할 수 있습니다.

  

해결:

  

1. 사용 가능성 확인
    
    ```
    docker buildx
    ```
    
      
    
2. docker buildx 를 통해 멀티 플랫폼 환경에서 Docker build.
    
    ```
    $ docker buildx build --platform=linux/amd64,linux/arm64
    ```