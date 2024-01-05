# Foreground Process & Background Process
## 무한 루프 Process 중지시키기
> Server 실행
> `$ yes > /dev/null` 명령 입력.
> 	- 무한 루프를 도는 단순한 프로세스 생성


> `ps -ef | grep yes` 를 통해 Process ID (PID) 확인.
> 	- 프로세스 소유자 / PID / 부모 Process ID / ... 순서 확인 가능.
> `kill (-9) (PID)` 명령어를 통해 Signal 을 보내 Process 중지 가능.


> 작동 중인 **Foreground Process 종료**
> 	- `Cmd + c`


> `jobs` 
> 	- 현재 작업들의 List 확인
> `fg`
> 	- job 중 Background 에서 돌아가고 있는 Process 를 Foreground 로 전환.
> `bg`
> 	- 사용 전 `Ctrl + z` 를 사용하여 현재 Foreground 에서 작동하고 있는 대상 Process 정지.
> 	- job 중 Foreground 에서 돌아가고 있는 Process 를 Background 에서 동작하도록 전환.


# Service
## Daemon
> **데몬(Daemon) = Service**
> 	- 눈에 보이지 않지만 현재 시스템에서 동작 중인 Process.
> 	- Background Process 의 일종.
> 	- Server 유형과도 밀접한 관련이 있다.
> 		- `Service = Daemon = Server Process` 라고 이해해도 무방.
