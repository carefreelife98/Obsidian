  

## 복습

```
~/.ssh/authorized_key -> Public Key (대칭 키)~/.ssh/known_host
```

  

## 공동인증서 원리

  

**은행 : Public 인증서 보유**

  

|암호화 통신 = Key Matching|

  

**개인 : Private 인증서 보유**

  

## AWS ssh pem key 인증 원리

  

AWS : Public key 보유 및 유저의 Pem key 생성

  

|암호화 통신 = Key Matching|

  

개인 : pem K

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-04_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.18.07.png]]

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-04_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.43.50.png]]

  

  

  

```
ansible 명령에는 -i inventory 옵션이 포함되어 있습니다.이는 ansible이 시스템 /etc/ ansible/hosts 인벤토리 파일 대신 현재 작업 디렉터리에서 inventory 파일을 만들도록 합니다.
```

- -i 옵션
    - 인벤토리 파일을 특정 디렉토리나 현재 디렉토리를 참조하여 찾겠다

  

  

# Playbook 구현

# Ansible Playbook

## 실패 사례

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.09.20.png]]

- 처음 hosts 파일을 작성 및 적용한 후 Ping 했을 시 정상 작동.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.13.43.png]]

- yaml 파일 작성의 편리성을 위해 Cloud 9 환경 아래로 Ansible 디렉토리를 옮겨 주었다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.09.11.png]]

- 하지만 이후 기존에 연결되어 있던 hosts 파일과의 연결이 끊기게 되었다.

  

![[%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.10.54.png]]

- 현상을 알아보니 default hosts 파일은 기존 경로 였던 /etc/ansible/hosts 경로에 존재해야 한다고 한다.