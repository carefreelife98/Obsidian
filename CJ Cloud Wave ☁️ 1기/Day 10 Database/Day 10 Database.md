  

# **DML**

## **SQL 명령어 종류**

- DML (Data Manipulation Language) : INSERT(입력) , UPDATE(변경) , DELETE(삭제) , MERGE(병합)
- DDL (Data Definition Language) : CREATE (생성) , ALTER (수정) , TRUNCATE (잘라내기) , DROP (삭제)
- DCL (Data Control Language) : GRANT (권한 주기) , REVOKE (권한 뺏기)
    - TCL (Transaction Control Language): COMMIT (확정) , ROLLBACK (취소)
    - SELECT : 어떤 분류에서는 DQL (Data Query Language) 라고 하기도 합니다.

  

- DML : insert 명령어
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-05_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.33.43.png]]
    

  

- DML : update
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-05_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.40.44.png]]
    
      
    
- DML : delete : 해당 테이블의 데이터를 삭제. rollback으로 복구가 가능해진다.
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-05_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.43.05.png]]
    
      
    
    - trunkcate table ~ : 테이블은 남아 있으나 내부 데이터만 전체 삭제, rollback 으로 되살릴 수 없다.
        
        ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-05_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.43.56.png]]
        
          
        
    - drop : 테이블 자체를 삭제 (완전히 삭제)

  

  

# DDL

- create : 테이블 생성하기
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-05_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.47.41.png]]
    
    - 테이블 생성 시 주의사항
        
        1. 테이블 이름은 반드시 문자로 시작해야 합니다**.** 즉 숫자로 시작할 수는 없고 숫자가 포함되는 것은 가능합니다**.** 특수문자도 가능하지만 테이블 생성시 **“ (**겹따옴표**)** 로 감싸야 하며 권장하지 않습니다**.**
        
          
        
        1. 테이블 이름이나 컬럼 이름은 최대 **30 bytes** 까지 가능합니다**.** 즉 한글로 테이블 이름을 생성하실 경우 최대 **15**글자 까지만 가능하다는 뜻입니다**.**
        
          
        
        1. 테이블 이름은 한 명의 사용자가 다른 오브젝트들의 이름과 중복으로 사용할 수 없습니다**.  
            **예를 들어 **scott** 사용자가 테이블명을 **test** 로 생성한 후 인덱스 이름을 **test** 로 동일하게 사용할 수 없다는 것입니다**.**그러나 **scott** 사용자가 **test** 테이블 만들어도 다른 사용자인 **hr** 사용자는 **test**라는 테이블 이름을 사용할 수 있습니다**.**
        
          
        
        1. 테이블 이름이나 오브젝트 이름을 오라클이 사용하는 키워드를 사용하지 않기를 권장합니다**.** 오라클 키워드라 함은 오라클에서 사용하는 미리 정해진 **SELECT ,FROM** 등과 같은 단어들을 말합니다**.**생성이 안되는 것은 아니지만 사용시에 아주 불편하고 위험 할 수도 있기에 절대로 사용하지 말기를 권장합니다**.**
        
          
        
- Sequence(시퀀스) 생성하기
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-05_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.56.29.png]]
    
      
    

  

  

## PLSQL

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.29.03.png]]

  

  

프로시저(Procedure)

- 하나의 프로시저에 여러 개의 쿼리문을 작성해서 처리할 수 있음.
- 여러개의 쿼리문이 필요한 서비스를 프로시저로 작성해서 호출하여 사용.
- 형식
    
    `create [or replace] procedure 프로시저명 [(매개변수)] -> 없으면 만들고, 있으면 대체하라.`
    
    `IS 또는 AS --> 둘 다 같은 의미. 아무거나 사용.`
    
    `변수선언`
    
    `begin`
    
    `본문`
    
    `[exception - 예외 처리]`
    
    `end;`
    
    작성된 프로시저는 execute 프로시저(); 형식으로 실행함.
    
      
    

## PL/SQL

- PL/SQL
    - 오라클에서 만든 문법이고 프로그래밍 처리가 가능함.
- 형식

```
DECLARE	변수 선언BEGIN	수행할 PL/SQLEND;
```

- 변수에 저장된 값을 출력하기 위해 서버 출력을 ON 해야함.

```
SET SERVEROUTPUT
```

  

- 변수 선언 하기
    - 대입연산자 (:=) 를 사용함.
- 종류
    - `1) 스칼라 변수 : 타입을 직접 지정.`
    - `2) 참조 변수 : 특정 칼럼의 타입을 사용.`
    - `3) 레코드 변수 : 2개 이상의 칼럼을 합쳐서 하나의 타입으로 지정.`
    - `4) 행 변수 : 행 전체 데이터를 저장.`

  

  
1. 스칼라 변수 : 직접 타입 명시.

```
DECLARE	NAME varchar2(10 byte);	grade number(3);BEGIN	name := '홍길동';	grade := 4;	DBMS_OUTPUT.PUT_LINE('이름은' || name || '입니다.');	DBMS_OUTPUT.PUT_LINE('학년은' || grade || '입니다.');END;
```

  

1. 참조변수

```
-- 참조변수 DECLARE	SNAME STUDENT.NAME%TYPE;	sgrade student.grade%TYPE;	sheight student.height%TYPE;BEGIN	SELECT name, grade, height		INTO sname, sgrade, sheight		FROM student		WHERE studno = 9611;		dbms_output.put_line(sname || '은 ' || sgrade || '학년이고, 키는 ' || sheight || '입니다. ');END;
```

  

## 프로시저(Procedure)

- 하나의 프로시저에 여러 개의 쿼리문을 작성해서 처리할 수 있음.
- 여러개의 쿼리문이 필요한 서비스를 프로시저로 작성해서 호출하여 사용.
- 형식

```
create [or replace] procedure 프로시저명 [(매개변수)] --> 없으면 만들고, 있으면 대체하라.IS 또는 AS --> 둘 다 같은 의미. 아무거나 사용.	변수선언begin	본문	[exception - 예외 처리]end;
```

- 작성된 프로시저는 execute 프로시저(); 형식으로 실행함.
- call 프로시저(); 형식도 가능?

- 예시
    
    - 예시 1
    
    ```
    CREATE OR REPLACE PROCEDURE proc1(s_studno IN student.STUDNO%TYPE) -- IN : 함수 호출 시 받겠다	student.STUDNO&TYPE : 참조변수 IS 	s_name student.NAME%TYPE;	s_grade student.grade%TYPE;	s_height student.height%TYPE;BEGIN	SELECT name, grade, height		INTO s_name, s_grade, s_height		FROM student		WHERE studno = s_studno; --매개변수 전달 		dbms_output.put_line(s_name || '은 ' || s_grade || '학년이고, 키는 ' || s_height || '입니다. ');	EXCEPTION		WHEN OTHERS THEN			dbms_output.put_line('예외발생');END;/-- 실행 및 매개변수 입력 --EXECUTE proc1(9515);-- 실행 및 매개변수 입력CALL proc1(9515);
    ```
    
      
    
    - 예시 2
    
    ```
    CREATE OR REPLACE PROCEDURE proc2(sname OUT student.name%type) ISBEGIN	SELECT name		into sname		FROM student	WHERE studno = 9515END;-- proc2 에서 return된 학생의 이름을사용.CREATE OR REPLACE PROCEDURE PROC3IS	sname student.NAME%TYPE; BEGIN 	proc2(sname);	dbms_output.put_line(sname || '학생입니다.');END;CALL PROC3;
    ```
    
      
    
- **PL/SQL 함수 호출 사용 (프로시저)**
    
    ```
    --procedure 호출 시 네 개의 데이터를 입력받아 해당 데이터를 테이블에 저장한다.CREATE OR REPLACE PROCEDURE proc4(s_gno IN gogak.gno%TYPE,									s_gname IN gogak.gname%TYPE, 									s_jumin IN gogak.jumin%TYPE,									s_point IN gogak.point%TYPE								)IS BEGIN 	INSERT INTO gogak(gno, gname, jumin, point)			VALUES (s_gno,s_gname,s_jumin,s_point);	dbms_output.put_line('고객 데이터가 입력되었습니다.');END;SELECT * FROM GOGAK;CALL proc4('20230706','채승민','9801161111111',0);
    ```
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.35.09.png]]
    

  

- 결과
    
    ![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.36.04.png]]
    
      
    

## 사용자 함수

1. 어떤 값을 반환할 때 사용하는 데이터베이스 객체
2. 실제로 함수를 만들어서 사용하는 개념.
3. return 값이 존재함.
4. 함수의 결과값을 확인 할 수 있도록 "select 문"에서 많이 사용.  
    -> 조회된 값(return)을 사용해서 다른 곳에 사용하기 위함이 큼.
5. 형식
    
    ```
    create or replace function 함수명[(매개변수)]return 반환타입is	변수 선언begin	함수 본문	예외 처리end;
    ```
    
6. 예시
    
    ```
    CREATE OR REPLACE func1RETURN varchar2	-- 반환 타입에는 크기를 명시하지 않음.IS BEGIN 	RETURN 'hello world';END;/SELECT func1 FROM dual;
    ```
    

  

  

## TRIGGER

1. DML(insert, update, delete) 작업 후 자동으로 실행되는 데이터 베이스 객체
2. 행(row) 단위로 트리거가 동작함
3. 종류
    - 1) before 트리거 : DML 동작 전에 수행.
    - 2) after 트리거 : DML 동작 후에 수행.

4. 형식

```
create or replace trigger 트리거명	before | after -> 둘 중에 하나 옵션.	insert or update or delete -> or 사용하여 세개 전부 사용 할 수 있다.	on 테이블명	for each rowbegin	본문end;
```

1. 예시

- gogak 테이블에 update 될 때 trigger가 동작한다.

```
CREATE OR REPLACE TRIGGER trig1	AFTER 	UPDATE 	ON GOGAK 	FOR EACH ROW BEGIN 	dbms_output.put_line('update complete');END;/UPDATE GOGAKSET POINT = 3000WHERE GNAME = '채승민';/SELECT * FROM GOGAK;
```

  

- gogak 테이블에 insert / update / delete 될 때 trigger가 동작한다.

```
CREATE OR REPLACE TRIGGER trig2	AFTER	INSERT OR UPDATE OR DELETE	ON gogak	FOR EACH ROW BEGIN 	IF inserting THEN 		dbms_output.put_line('data added');	ELSIF updating THEN		dbms_output.put_line('data updated');	ELSIF deleting THEN		dbms_output.put_line('data deleted');	END IF;END;/DELETE FROM GOGAK	WHERE GNAME = '채승민';
```

  

1. 예제

```
-- 트리거를 이용해서 고객테이블(gogak)과 탈퇴고객 테이블(retire_gogak)테이블 제어-- 고객이 회원 탈퇴 시 탈퇴한 고객정보를 CREATE TABLE retire_gogakASSELECT * FROM gogak WHERE 1=2;/SELECT * FROM retire_gogak;/ALTER TABLE retire_gogak ADD retire_id NUMBER NOT NULL;ALTER TABLE retire_gogak ADD retire_date DATE;/SELECT * from retire_gogak;-- primary key 추가 ALTER TABLE RETIRE_GOGAK 	ADD CONSTRAINT pk_retire_gogak PRIMARY key(retire_id);	-- primary key에 입력할 sequence 생성.CREATE SEQUENCE r_gogak_seq nocache;	CREATE OR REPLACE TRIGGER trig_gogak	AFTER 	DELETE 	ON gogak	FOR EACH ROW BEGIN 	INSERT INTO retire_gogak(retire_id, gno, gname, jumin, point, retire_date)		VALUES (r_gogak_seq.nextval, :OLD.gno, :OLD.gname, :OLD.jumin, 0, sysdate); --OLD : 지워지기 전 데이터를 나타냄 (기존데이터)END;/DELETE FROM GOGAK	WHERE gno = 20110001;	SELECT * FROM retire_gogak;
```

  

- 결과 (retire_gogak 테이블에 삭제된 고객의 정보가 저장된 모습)

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.15.30.png]]

  

- 결과 (gogak 테이블에서 20110001 고객이 삭제된 모습)

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-07-06_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.16.44.png]]