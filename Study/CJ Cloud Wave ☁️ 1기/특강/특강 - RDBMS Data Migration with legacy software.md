  

- **목적**
    - 실 서비스 환경에서 data migration의 needs 가 상당.
- **장점**
    - 상용 소프트웨어 사용 제한 및 비용의 이슈
    - 서드 파티 어플리케이션 보다 좀 더 native 성능, 빠른 속도
    - 비용 제약이 없음
- **단점**
    - 각 레거시 소프트웨어의 사용법 숙지 / 테스트 필요
    - 부분적인 제약이 존재
    - 런타임 오류에 대한 디버깅이 어려울 수 있다.
        - 오류에 대한 메세지 부재
- **Migration process**
    1. 요구사항 분석
    2. 계획 및 설계
    3. 데이터 추출
    4. 데이터 변환 및 매핑
    5. **데이터 로드 → Target program for today**
        1. 변환된 데이터를 대상 DB에 로드
        2. SQL * Loader, insert문, 외부 테이블 등을 사용하여 데이터 로드
        3. 로드 과정에서 데이터의 유효성 검사, 제약 조건등을 수행
    6. 테스트 및 검증
    7. 적응 및 전환
    8. 모니터링 및 최적화

  

- 주의사항
    - **데이터 일관성 유지**
    - **데이터 변환과 매핑**
    - 데이터 **유실 및 중복 방지**
    - **성능 및 용량** 고려
    - 테스트 및 백업
    - 감사 및 모니터링
    - 보안 고려
    - 계획과 **문서화**

  

## Migration Tool - Oracle

- Oracle Data Pump
    - 데이터 베이스 간의 데이터 및 객체 이동을 위한 강력한 도구
    - 데이터 압축 전송
    - 원본에서 대상으로 전체 데이터 베이스 migration 또는 선택적 테이블/스키마 migration 지원
    - 병렬 처리를 활용하여 더욱 빠른 이관 가능
- Oracle SQL Loader
    - 대량 데이터 로드 유틸리티
    - txt, CSV 파일 등 외부 소스에서 데이터 로드 가능
    - 매우 빠른 속도로 대량의 데이터를 처리할 수 있는 기능 제공, 데이터 로드 과정에서 데이터 변환, 유효성 검사 및 오류 처리를 수행가능.
- SQLPlus
- UTL_FILE 패키지
    - 테이블 데이터를 파일로 출력 가능.
    - DB 서버에서 파일을 읽거나 쓸 수 있는 기능을 제공.
    - PL/SQL 블록 내에서 UTL_FILE 패키지를 사용하여 데이터를 파일로 출력가능.
- EXTERNAL TABLE (확장 테이블)
- DBLINK (Oracle 제공)
    - 소스 DB에서 다른 DB로 접속하여 쿼리 및 데이터 핸들링 기능.
    - 하나의 데이터 베이스에서 다른 데이터 베이스의 테이블에 접근 갱신 가능
    - 병렬 처리 및 DDL을 통한 fast-load 가능, JOIN 가능
    - 쓸모가 없어진 DB LINK는 즉시 삭제. → 해당 데이터가 다른 어플리케이션에 연결되게 되면 삭제가 어려워짐

  

  

## Migration Tool - MSSQL

- 백업 및 복원 (Backup and Restore)
    - **압축 기능 지원** → 점검 시간 상당히 단축. 꼭 사용!
- 데이터베이스 내보내기 및 가져오기 (Export and Import)
- SQL Server Integration Services (SSIS)
- BCP (Bulk Copy Program) AND BULK Insert
    - 대량의 데이터를 테이블과 파일 간에 복사하는 기능
    - Window / Linux 버전 존재
- SQLCMD
- Export Wizard (SQL Server Management Studio)
    - 타 Database로 이관이 가능
- OPENROWSET 및 BULK INSERT
- External Table
- LINKED SERVER
    - DBLINK(Oracle)와 동일한 기능 수행.
    - 대부분 많이 사용.

  

  

## Migration Tool - MySQL , MariaDB

- MySQL Workbench
    
- SELECT INTO OUT FILE
- MYSQLDUMP
    - 백업 프로그램
- MYSQLIMPORT
- CSV Engine
- CONNECT Engine (Only MariaDB)
    - DBLINK 와 같은 기능
- FEDERATED Engine (Only MySQL)
    - DBLINK 와 같은 기능

  

## Migration Tool - PostgreSQL

- pg_dump / pg_restore
- 데이터베이스 복제
- Foreign Data Wrapper (FDW)
    - DBLINK 와 같은 기능
- COPY
- psql

  

  

## Migration Tool - Tibero → Oracle과 대부분 동일

- T-UP 유틸리티
- Tibero DB LINK
- Tbexport & Tbimport
- tbsql spool
- tbMigrator 2.0
- tbLoader
    - **병렬 처리 가능**

  

  

  

## 질의 응답

- **아키텍쳐 및 버전에 따른 테스트 환경 구축(Version, New feature)**
    
    → DB 다루는 것에 있어 매우 중요하다.
    
- RDBMS vs NoSQL ? → 둘은 목적이 다르다.
    - NoSQL
        - Cache 용도.
        - 빠른 응답
        - RDBMS 와 같은 트랜잭션 처리 불가.
            - 대체 불가
        - RDBMS 학습 시 NoSQL 학습은 상당히 쉽다.
    - RDBMS 를 뒷단에, 가벼운 데이터는 NoSQL 사용하여 앞 단에 구현하는 Architecture가 트렌드.
- Major 한 DB를 학습해라.
- 클라우드 관련 직무 → 현재 Database 관련 수요 많음