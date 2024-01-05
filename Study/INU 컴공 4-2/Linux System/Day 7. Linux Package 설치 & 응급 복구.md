> `dpkg`
> 	- Debian 에서 제작한 패키지 설치 유틸.
> 	- dpkg 가 확장된 것이 `apt-get` , `apt`
> 
> **패키지**
> 	- 프로그램 설치 후 바로 실행할 수 있는 설치 파일.
> 	- 확장명 : `*.deb`

# apt-get 의 작동 방식과 설정 파일
![[스크린샷 2023-09-25 오후 7.05.47.png]]
1.  `apt install (패키지 명)` 명령을 입력.

2.  자동으로 /etc/apt/ 디렉터리의 핵심 파일인 **sources.list**를 확인

3.  설치할 패키지와 관련된 목록 요청

4. 설치할 패키지와 관련된 목록만 다운로드

5. 사용자는 패키지 목록을 확인한 후 설치 의향 있으면 ‘y’를 입력하여 실제 패키지 다운로드를 요청

6.  패키지 파일(deb 파일)이 다운로드되어 자동으로 설치됨

**`apt -y install (패키지 명)` 명령을 실행하면 과정 2~7 이 한번에 이루어짐**

## main, universe, restricted, multiverse 의 의미.
- `main`
	- Ubuntu 에서 공식적으로 지원하는 무료 소프트웨어.
- `universe`
	- Ubuntu 에서 지원하지 않는 무료 소프트웨어
- `restricted`
	- Ubuntu에서 공식적으로 지원하는 유료 소프트 웨어
- `multiverse`
	- Ubuntu 에서 지원하지 않는 유료 소프트 웨어.

# GRUB Boot Loader

