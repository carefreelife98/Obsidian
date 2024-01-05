---
Author: CarefreeLife98
Date: 2023-10-12T20:10:00
Agenda: 
tags:
  - Linux
---
> 공간 할당
> - 디스크가 꽉 차면 시스템 전체가 가동되지 않을 수 있음.
> - 위와 같은 상황을 미연에 방지하기 위해 사용자 별로 할당된 공간만 사용하도록 용량을 제한.

1. 사용자 생성
2. /etc/fstab  파일 편집하기
	- `$ vi editor`
		- `$ vi /etc/fstab`
	- 
	- `usrquota, grpquota`
		- Linux 2.0 대에서 사용하던 과거 명령어
	- `usrjquota`
		- **j = Journal**
			- CS 에서 Journaling 이란,Logging 과 같음.
				- 변경사항들을 기록해 놓는 것.
3. quota on/off 및 파일 생성
4. 사용자 별로 공간 할당하기


> 쿼터
> - 각 사용자가 가용할 수 있는 파일의 용량을 제한하는 것.