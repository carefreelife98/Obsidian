# Grub Boot Loader
> RAM 은 휘발성 메모리.
> - **BIOS 를 실행하기 위한 코드는 ROM 에 저장되어 있음.**
> - **해당 코드가 Boot Loader (GRUB)**
> 	- Boot Strap
> - GRUB 의 설정 파일은 `/boot/grub/grub.cfg` 에 존재.
> <br><br>
> **실행 순서**
> 1. BIOS 에 존재하는 Boot Strap이 ROM 에 존재하는 Boot Loader 코드를 실행.
> 2. Boot Loader 가 OS 를 실행.

# GRUB Boot Loader 변경 및 비밀번호 설정
> GRUB 전용 사용자와 비밀번호 생성하기
> - `vi /etc/grub.d/00_header` 명령으로 파일 Open.
> - 다음 네 행을 추가 및 파일 저장 후 종료.

```bash
cat << EOF

# grubuser = 새로운 GRUB 사용자의 이름
set superusers="grubuser" 

# GRUB사용자 새 비밀번호 형식으로 비밀번호 설정.
password grubuser 1234

EOF
```

> - 변경한 내용을 적용하기 위해 `update-grub` 명령 입력.
> - `reboot` 명령으로 시스템 재부팅.
- 주의! 
	- 이후부터는 재부팅 시 위에서 적용한 사용자 및 비밀번호를 입력해야 Boot Loader(GRUB) 을 통해 부팅이 됨.
	- 비밀번호 등을 까먹게 되면 부트로더 자체를 재설치 해야하나 기존 데이터 복구가 상당히 어려워짐.




