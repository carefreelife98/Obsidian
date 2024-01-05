  

## 질문 내용

안녕하십니까 교수님.

201702797 채승민 과제 제출입니다.

코드 작성 중 에러가 해결되지 않아 하나의 헤더 파일에 기능을 전부 집어넣게 되었습니다.

혹시 시간 여유가 되신다면 해당 에러 발생 원인을 알고 싶습니다.

  

[프로젝트 구조 요약]

우선 Root에 존재하는 MyAvengers.h 파일은 MyAvengers 네임스페이스와 그 내부의 클래스인 CarefreeHero, CarefreeHeroPick 이 존재합니다.

두 개 클래스 중 CarefreeHero 클래스는 Private Resource 와 Public 메소드가 존재하며 CarefreeHeroPick 클래스는 Public 메소드만 존재합니다.

Avengers Project 내에서 세 개의 폴더 (fly_carefree, hero_carefree, weapon_carefree) 에는 MyAvengers::CarefreeHero 클래스 내부에 정의된 Private Resources 를 Public 메소드에 반환해주는 .cpp 파일이 존재합니다.

  

[에러 내용]

헤더 파일에는 메소드를 선언만 하여 모듈화 한 후에 적절한 곳에서 헤더 파일에 선언된 메소드들을 오버라이딩 하여 사용하는 것으로 알고 있습니다.

따라서 헤더 파일에 선언된 CarefreeHero 클래스의 Public 으로서 정의된 메소드들은 선언만 해두었습니다.

이후 해당 프로젝트 컴파일 시 아래와 같은 에러가 발생했습니다.

```
Undefined symbols for architecture arm64:  "MyAvengers::CarefreeHeroPick::selectHero()", referenced from:      _main in Avengers_main-c851fd.o  "MyAvengers::CarefreeHeroPick::setHero(int)", referenced from:      _main in Avengers_main-c851fd.old: symbol(s) not found for architecture arm64clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

  

현재 작업 환경은 MacBook M1 실리콘 칩 사용 중 입니다.

현재 상황은 기존에 분리해두었던 코드들을 MyAvengers.h 파일에 전부 옮겨 두었고, 이 경우에는 잘 실행이 되고 있습니다.

GPT 및 구글링 통해서 정보를 얻기가 어려워 이렇게 질문 드리게 되었습니다. 감사합니다.

  

## 원인

  

  

## 해결