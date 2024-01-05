  

```
curl -O http://archive.apache.org/dist/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.41-src.tar.gz
```

  

```
tar -zxvf tomcat-connectors-1.2.41-src.tar.gz
```

  

```
cd tomcat-connectors-1.2.41-src/native
```

  

```
/usr/local/apache2/tomcat-connectors-1.2.41-src/native # ./configure --with-apxs=/usr/local/apache2/bin/apxs
```

- cd 압축해제위치/native

  

```
make && make install
```

  

에러 : `configure: error: You must specify a valid --with-apxs path`

- apk add perl
- apk add gcc
- apk add apache2-dev
- apk add wget
- apk add build-base

  

configure: error: C compiler cannot create executables

  

httpd.conf 수정

```
vi /usr/local/apache2/conf/httpd.conf  # mod_js.so 위치LoadModule jk_module modules/mod_jk.so # mod_jk.c 설정<IfModule mod_jk.c># 다음과 같은 경로는 tomcat으로 연결JKMount /* tomcatJKMount /*.jsp tomcatJkMount /jkmanager/* jkstatus JkMountCopy All<Location /jkmanager/>        JkMount jkstatus        Require ip 127.0.0.1</Location> # workers.properties 위치JkWorkersFile "apache/conf/workers.properties"# 로스 위치JkLogFile "| apache/bin/rotatelogs -l apache/mod_jk.log.%y%m%d 86400 "# 로그 레벨JkLogLevel error# 로그 포멧JkLogStampFormat "[%a %b %d %H:%M:%S %Y] "JkShmFile  "apache/mod_jk.shm"\#JkRequestLogFormat     "%w %V %T"</IfModule>
```

  

**workers.properties** 추가

```
vim /usr/local/apache2/conf/workers.properties  worker.list=tomcat worker.tomcat.type=ajp13worker.tomcat.host=127.0.0.1worker.tomcat.port=8001worker.tomcat.retries=1 worker.tomcat.socket_timeout=10worker.tomcat.connection_pool_timeout=10 worker.list=jkstatusworker.jkstatus.type=status
```

  

  

  

  

# 최종

1. Docker 에서 httpd(apache) image 생성
2. `docker run -d -p 80:80 --platform=linux/amd64 --name gooloom-apache-image httpd`
3. docker container 접속
    
    ```
    $ docker exec -it (name) /bin/bash
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.02.06.png]]
    
      
    
4. 필수 패키지 설치
    - `apt-get update`
    - `apt-get install curl`
    - `apt-get install perl`
    - `apt-get install apache2-dev`

  

  

1. mod_jk 설치
    
    - `curl -O http://archive.apache.org/dist/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.41-src.tar.gz`
        
          
        
        ```
        tar -zxvf tomcat-connectors-1.2.41-src.tar.gz
        ```
        
          
        
        ```
        cd tomcat-connectors-1.2.41-src/native
        ```
        
          
        
        ```
        /usr/local/apache2/tomcat-connectors-1.2.41-src/native # ./configure --with-apxs=/usr/local/apache2/bin/apxs
        ```
        
        - cd 압축해제위치/native
        
          
        
        ```
        make && make install
        ```
        
    
      
    
      
    
      
    
      
    

## Apache 2 configuration

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.45.01.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.45.19.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.49.54.png]]

usr/local/apache2/conf/extra/httpd-vhosts.conf

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.05.49.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-29_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.28.17.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.05.48.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-29_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.33.17.png]]

## Docker commit

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.16.19.png]]

  

  

  

  

성공..

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.58.11.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.24.34.png]]

  

  

  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.06.27.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.10.23.png]]

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.10.54.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.30.05.png]]

  

  

  

  

# [Trouble Shooting] - DB

  

```
aws rds describe-db-parameters \    --db-parameter-group-name mydbpg
```

- DB 파라미터 그룹 변경 및 확인

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.55.44.png]]