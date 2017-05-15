# 1. Download

https://github.com/Microsoft/malmo/releases

이 링크에 가서 운영체제에 맞는 파일을 다운로드 받는다.

필자의 경우 Malmo-0.21.0-Windows-64bit.zip 파일을 받았다.


# 2. install dependencies 

디펜던시란, 프로그램을 빌드하는데 필요한 외부 종속된 라이브러리 같은 것들을 분리해 놓은 것이다.

우리는 windows 10 에 내장되어있는 power shell을 사용해서 디펜던시들을 설치할것이다.

명령창을 열고 다음의 명령어들을 입력한다.
```
powershell 
Set-ExecutionPolicy -Scope CurrentUser Unrestricted
```

다음은 설치된 경로로들어가는 것이다. ($env:HOMEPATH\Malmo-0.21.0-Windows-64bit\scripts)

기본적으로 cd [경로명을통해서 들어갈 수 있다]

```
cd Malmo-0.21.0=Windows-64bit\scripts
.\malmo_install.ps1
```

이제 디펜던시들이 설치된다.

# 3. Minecraft 실행

자, 이제 Minecarft를 한번 실행해보자.

(당연한 얘기지만, 마인크래프트가 돌아가는 컴퓨터 머신이여야 실행이된다.)

아래의 경로를 찾아서 들어가보자
```
\Malmo-0.21.0-Windows-64bit\Minecraft
```

그리고 명령창을 열어서 다음과 같이 입력한다.

```
launchClient
```

그럼 마인크래프트가 실행이 되고 메인 메뉴화면이 보일 것이다.

### 주의 사항 

1. 여기서 게임을 생성하지 않고 메인메뉴에서 기다리도록 한다.
2. 기본 port 번호는 10000번이다. 다음 진행할 pre built된 예제들은 port 10000번을 사용한다.

# Agent 실행하기

```
/Cpp_Examples
/CSharp_Examples
/Java_Examples
/Python_Examples
```

어떤 Example 폴더든 상관 없다.

들어가서 미리 빌드된 실행파일을 돌리면 된다.

예로 cpp_example 폴더로 들어가  run_mission.exe를 실행시키면 
자동으로 게임이 시작되 플레이어가 회전하는 모습을 볼 수 있다.

![img](https://jvkgfw.dm2302.livefilestore.com/y4m-iHLl30XkQ5WDwDYbhkfFx2spdErmInnPNZOY4ib1Ja3z5puI4PJtQwf8Gsml_VLBriaEkXZEhz2d1vFGagPqLkOOYe4OG2ZCR4wGyoNnt3GCBph5cgSw5rA9e1YQsUb2-Z6DvbK-KEdj_ma2qJdVad7NFUVrolZoI0etw7IOnyuee1gDq6E3T6xV6hmBl-Q9Zj5yG4LO5JeF0EWRQG73Q/Malmo_tutorial.bmp?psid=1)

