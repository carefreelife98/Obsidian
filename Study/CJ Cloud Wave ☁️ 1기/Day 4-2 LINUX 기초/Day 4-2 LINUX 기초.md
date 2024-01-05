  

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.28.51.png]]

- 위 IP가 같은 와이파이 사용자 내에서 전부 같은 이유
    - 같은 NATGate 를 사용하므로.

  

- 절대 경로
    - 최상위 디렉토리 ‘ / ’ 부터 전체 경로를 모두 적는 방식
    - 절대 경로 사용 시 자신의 현재 위치와 상관 없다.
    - ex) /var/www/html
- 상대 경로
    - 현재 디렉토리를 기준으로 경로를 적는 방식
    - . : 현재 디렉토리
    - .. : 상위 디렉토리
        - 현재 위치가 /var/www 일 경우
            
            → /var/www/html 디렉토리 이동시 상대경로 사용
            
            - cd html (o)
            - cd /html (x)
        - 현재 위치가 /var/www 일 경우
            
            → /var/log 로 이동시 상대경로 사용
            
            - cd ../log (o)
            - cd log (x)
- 소유권 (Ownership) / 허가권 (Permission)
    - group을 나누는 이유 → 추후에 도커 사용시 각각의 그룹을 나눠 sudo 명령없이 manage 할 수 있다?
        - 권한 설정을 각각의 그룹마다 한번에 설정해주기 쉬워짐.
    - 소유권 : user / group / other
        - ls -l → 소유자:소유그룹
    - 허가권 : user / group / other
        - ls -l rwx rwx rwx → read write execute
            
            rw- r— r—
            
            - execute
                - case 1. 파일 : 실행 할 수 있는 권한
                    - text 파일이더라도 script이면 실행 할 수 있다.
                - case 2. 디렉토리 : 디렉토리에 cd로 접근할 수 있는 권한
            - root
                - 파일 생성 : rw- r— r— 로 권한 설정 (= 옥탈: 644)
                - 디렉토리 생성 : rwx r-x r-x 로 권한 설정 (= 옥탈: 755)
            - 일반 사용자
                - 파일 생성 : rw- rw- r— 로 권한 설정 (=옥탈: 664)
                - 디렉토리 생성 : rwx rwx r-x 로 권한 설정 (=옥탈: 775)

  

옥탈 방식? : rwx를 이진수로 바꾸어 표현

r = 2^2 = 4

w = 2^1 = 2

x = 2^0 = 1

  

- 0 : 권한 없음
- 1 : 실행 권한
- 2 : 쓰기 권한
- 4 : 읽기 권한

  

- 예 : chmod 400 키파일
    
    - r— —- —- (4 0 0)
    
      
    

## 특수 퍼미션

1. setuid(4 - - - )(정상적인 파일인지 확인하는 절차가 존재)
    - 파일을 실행하는 동안 소유자의 권한이 부여됨
    - user의 x 부분에 s가 표시됨.
2. setgid(2 - - - )(잘 쓰이지 않는다)
    - 파일을 실행하는 동안 소유그룹의 권한이 부여됨.
    - group의 x 부분에 s가 표시됨
3. stickybit(1 - - - )
    - stickybit가 설정된 디렉토리에 생성된 파일은 소유자만 삭제 가능.
    - other의 마지막 부분에 t가 표시됨.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.16.40.png]]

  

- ls -l /etc/shadow
    - 위 파일에 변경된 비밀번호를 저장해야 한다.
    - 하지만 해당 파일은 root만 접근가능.
- setuid가 걸려있으면 root의 권한으로 실행이 되어 바뀐 passwd를 저장 할 수 있게 된다.
- linux는 현재 디렉토리에 있는 파일을 실행시키려면 ./를 붙여야 한다.

  

- tar : 파일을 테이프 장치에 저장하기 위한 용도로 사용하는 프로그램  
    여러 파일을 묶어 하나의 파일로 만드는 프로그램
- 압축프로그램 : gzip, bzip2, zip, xz, compress 등이 있다.

> tar와 압축[해제]를 동시에 할때  
> x[c]fz bin.tar.gz => tar -> gzip  
> x[c]fj bin.tar.bz2 => tar -> bzip2  
> x[c]fZ bin.tar.Z => tar -> compress  
> x[c]fJ bin.tar.xz => tar -> xz

/work 에 lab 디렉토리 생성  
mkdir /work/lab

/sbin/s* 파일을 묶고, xz으로 압축. 파일명 sbin-s.tar.xz  
cd /work/lab  
tar cf sbin-s.tar /sbin/s*  
xz sbin-s.tar

tar cfJ sbin-s.tar.xz /sbin/s*

2./home 디렉토리 및 하위 모든 파일을 묶어서 압축(bzip2) home.tar.bz2  
tar cfj home.tar.bz2 /home

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.28.30.png]]

  

— Wordpress 설치  

1. wordpress 파일 다운로드
    1. wget https://~~~~~
2. 압축, tar 해제
    1. gunzip 파일명.tar.gz
    2. tar xf 파일명.tar
3. sudo mv wordpress /var/www/html
4. sudo su -
5. apt install php mariadb-server
6. service apache2 restart
7. service mysql start

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.49.50.png]]

  

- mariadb의 bind address 를 0.0.0.0 으로 변경하여 외부에서 db 변경이 가능하도록 설정해준다.

  

중요한 것

- 각각의 파일이나 디렉토리에 허가권을 부여하여 접근 권한을 설정 할 수 있다.
- rwx의 기능

## 명령어

- ls : 현재 디렉토리 내의 파일 출력
- ls -a : 숨김 파일 까지 전부 출력
- ls -l : 전체 파일 자세히 출력
- pwd : 전체 경로 보기
- su - : 루트 사용자가 PW 인증 후 사용하도록 해줌
- sudo su - : 루트 패스 몰라도 루트 사용자로 변환시켜줌
- sudo nano sshd_config : 편집기를 열어 설정 변경을 도와줌
- sudo passwd ec2-user : 관리자 권한으로 비밀번호 변경
- wget (url) : 해당 링크의 다운로드 파일을 다운로드 한다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-06-29_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.48.08.png]]

- passwd 로 로그인 가능하게 변경 후 key가 아닌 pwd를 사용하여 로그인 하는 모습.

  

## 파일

- sshd_config : 설정 파일

  

  

## LINUX

- 루트가 아닌 사용자 : 입력창에 $ 가 붙음.
- 리눅스를 대부분의 설정 파일을 편집기를 사용하여 편집할 수 있도록 한다.

  

Rocky linux

  

Redhat 계열 linux : yum 사용

Ubuntu 계열 linux :

  

특정 단어와 같은 내용을 찾는다?

ctrl + F 와 같은 느낌?  
- netstat -an | grep :80

  

vi는 nano가 없다고 하더라도 작동을 한다.

편리한 편집 툴?