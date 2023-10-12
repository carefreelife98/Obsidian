
# Network Configuration & CMD
## TCP / IP
- TCP
	- 통신의 전송 및 수신을 다룬다.
- IP
	- 데이터 통신을 다룬다.

## Host name & Domain name
- Host name
	- 각각의 컴퓨터에 지정된 이름
	- 실제 하드웨어의 주소인 MAC Address 와 Mapping 된다.
- Domain name
	- IP 주소를 사용자가 쉽게 사용하기 위해 알파벳으로 구성된 주소를 적절한 IP로 변환시켜주는 이름.
- 파일 위치
	- /etc/resolv.conf
		- DNS 서버의 정보와 호스트 이름 저장되어 있는 파일.
	- /etc/hosts
		- 현재 컴퓨터의 호스트 이름과 FQDN 이 들어있는 파일.

- 예시
	- **Host name** : admin
	- **Domain name** : inu.ac.kr
	- **FQDN(Fully Qualified Domian Name) 은 admin.inu.ac.kr** 이 된다.


# File 압축
## xz
- 확장명 xz 로 압축하거나 풀기
- 비교적 최신 압축 명령