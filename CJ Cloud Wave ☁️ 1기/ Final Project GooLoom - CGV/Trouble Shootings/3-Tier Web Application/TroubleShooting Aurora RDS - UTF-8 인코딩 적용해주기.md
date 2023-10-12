  

## 3 Tier Architecture Web Application 구축 후 Aurora DB 연결

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.22.32.png]]

- 위 사진과 같이 Web App과 DB가 통신/연결은 되는 것 같으나 Post 요청 에러 및 UTF-8 인코딩 에러 발생.
    - `Request method 'POST' is not supported`
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.06.33.png]]
        
        - Spring boot 에서 `spring.mvc.hiddenmethod.filter.enabled=true` 를 [`application.properties`](http://application.properties) 에 추가하여 해결
    - SQL Error: 1366, SQLState: HY000
        - 한글 입력 시 인코딩 에러. 아래에서 해결

  

## UTF-8 인코딩 에러 해결

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.20.54.png]]

- `character_set` 을 `utf8` 로 설정한 DB Cluster Parameter Group 생성.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.18.35.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.18.03.png]]

- `DB Cluster 수정` 에서 새로 생성한 DB Parameter Group을 할당 및 `즉시 적용`

  

## AWS CLI 를 통해 DB Reboot 가 필요한지 확인

```
$ aws rds describe-db-clusters --db-cluster-identifier prod-cgv-rds-db-1(= DB Cluster 이름)
```

- 위 명령어를 사용하여 `DBClusterMembers` → `DBClusterParameterGroupStatus` 부분을 확인.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.29.26.png]]

- `pending-reboot` 상태이므로 DB Cluster를 재시작 해주어야 적용이 된다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.31.29.png]]

- DB Instance 재부팅 해주자.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.34.23.png]]

- 재부팅 후 `in-sync` 로 상태가 변경된 것을 확인.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.03.56.png]]

```
mysql -h (DB Primary instance EndPoint) -P 3306 -u admin -pmysql -h prod-cgv-rds-db-1-instance-1.cje7wp54eror.ap-northeast-2.rds.amazonaws.com -P 3306 -u admin -p
```

- 자신의 Aurora DB 접속하는 방법

```
show variables like 'c%';
```

- DB에 적용된 파라미터 확인하기

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.37.30.png]]

- 다른 부분은 `utf-8` 로 변경이 되었으나, `database` 부분은 변경이 되지 않았다?

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-26_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.43.44.png]]

```
show full columns from (테이블 명) ;
```

- 위 쿼리로 테이블의 character_set 확인.

  

```
alter table (테이블 명) convert to character set utf8;
```

- table의 character_set을 utf8 로 변경.

  

## 수정된 Spring boot Web Application 을 Docker image 로 Build 후 재배포 하기.