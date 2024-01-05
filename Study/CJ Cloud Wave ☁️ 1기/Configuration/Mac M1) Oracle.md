  

> 콜리마 먼저 실행 -> 맥 m1 버전
> 
> `colima start --memory 4 --arch x86_64`

- 기존의 컨테이너 실행하려면
    
    `docker start 컨테이너이름`
    
- 기존 컨테이너 목록 확인
    
    `docker context ls`
    
- 현재 실행되고 있는 컨테이너
    
    `docker ps`
    
- 죽어있는 컨테이너 까지 모두 확인
    
    `docker ps -a`
    
- 기존의 오라클 컨테이너 삭제
    
    `docker rm <컨테이너이름>`
    
- 기존의 오라클 볼륨 파일 제거 (생성한 볼륨 폴더가 있는 곳에서)
    
    `Sudo rm -rf <컨테이너이름>`
    
- 새로운 컨테이너 생성 - 타임존까지 설정
    
    `docker run --name 컨테이너이름 -p 1521:1521 -e ORACLE_PASSWORD=원하는비밀번호 -e TZ=Asia/Seoul -d gvenzl/oracle-xe`
    
- sys 계정 접속하기
    
      
    
- 생성시, 재시작해도 콜리마만 키면 바로 연결되게 하는 법 :
    
    run 뒤에 `--restart unless-stopped` 추가
    
- 같은 포트(1521)에서 다른 컨테이너 사용하고 싶을 땐
    
    `docker stop 컨테이너이름` 으로 종료 후 연결(docker start 컨테이너 이름)
    
      
    
      
    
    > Docker sql 접속하기
    
    1. `docker exec -it 컨테이너이름 bash`
    2. `sqlplus`
    3. 유저네임 - `system` -> 따로 설정했다면 그거 넣기
    4. 비번 - `pass`
    
    ---
    
    > 볼륨지정하고 컨테이너 올리기
    
    `mkdir ~/Documents/원하는폴더이름`
    
    `docker run -d --name 컨테이너이름 -v ~/Documents/만든폴더이름:/opt/oracle/oradata -p 1521:1521 -e TZ=Asia/Seoul -e ORACLE_PASSWORD=pass gvenzl/oracle-xe`
    
    `docker logs -f oracle`
    
      
    
    ### 사용자 이름 변경
    
    [![](https://velog.velcdn.com/images/serringg/post/0946fbe2-0e17-4a16-b512-40e5df23e9a1/image.png)](https://velog.velcdn.com/images/serringg/post/0946fbe2-0e17-4a16-b512-40e5df23e9a1/image.png)
    
    - 위의 username, 사용자 이름을 변경하는 방법
    - 위의 명령어로 도커 sql먼저 접속
    
    1. `docker exec -it 컨테이너이름 bash`
    2. `sqlplus`
    3. 유저네임 - `system` -> 따로 설정했다면 그거 넣기
    4. 비번 - `pass`
    
    ---
    
    - `CREATE USER [유저이름] IDENTIFIED BY [비밀번호];`
    - `GRANT RESOURCE, CONNECT TO 유저이름;`
    - `grant create view to 유저이름`
    
    ---
    
    - 유저 삭제하며 관련 테이블 모두 drop
        - `drop user {유저아이디} cascade;`
    - 암호 기간 만료 비활성화
        - `alter profile default limit password_life_time unlimited;`
    - 최대 접속 가능 process 조회 및 변경
        - `show parameter processes;`
    - `alter system set processes = 200 scope=spfile;`
    
      
    
    # Oracle DB 시작 방법
    
    도커 엔진을 콜리마로 실행 시켜주셔야 합니다.
    
    아마 컴퓨터를 재시작해서 종료되셨나봐요. 평소에 사용안할때는 `colima stop`으로 종료해두고 필요할때만 새로 켜서 사용하시는 편이 좋을거에요.
    
    ```
    colima start --memory 4 --arch x86_64
    ```
    
    콜리마 실행시킨 후에는 컨테이너를 띄워야하는데, restart 옵션을 걸어두셨었다면 colima 실행시 자동으로 실행될것이고, 그렇지 않다면 `docker ps -a` 한번 하고 종료된 컨테이너 이름 찾아서 `docker restart {컨테이너명}` 해서 실행해주심 됩니다