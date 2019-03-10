# 개요
Vtune Ampiler는 Intel에서 제공하는 상업용 소프트웨어 성능 분석 소프트웨어 입니다. 
GUI와 커맨드라인을 모두 사용가능하며, Windows 와 Linux 운영체제에서 동작합니다. 
다양한 기능이 있으며 AMD, Intel 칩셋에서도 동작합니다.

# 코드 최적화
VTune Amplifier는 스택 샘플링, 스레드 프로파일링, 하드웨어 이벤트 샘플링을 비롯한 다양한 종류의 코드 프로파일링을 지원 합니다.
프로파일링 결과는 각 서브 루틴에 소요되는 시간을 보여줍니다.

# 사전 준비

1. 프로젝트를 우클릭하여 Project > Properties(속성) 창을 연다.
2. Configuration Properties > General(일반) 탭을 선택한다. (이때, 기본 구성이 Release 계열인지 확인한다.)
3. Debug Information Format이 /(debug:full)로 되어 있는지 확인한다.
4. Optimization(최적화) 에서 MaximizeSpeed로 설정한다.
5. Linker(링커) 에서 Generate Debug Info(디버그 정보 생성) 옵션을 Yes로 설정한다.
6. 확인, 적용 후 프로젝트를 빌드한다.
7. IDE 어태치 하지 않고 프로그램 실행

# Vtune Amplifer Project 생성

1. Vtune Amplifier version standalone GUI를 연다.
2. New > Project를 통해 새로운 프로젝트를 생성한다.
3. 프로파일링 타입을 선택한다.(핫스팟, 스레딩 등..)
4. Configuration 에서 *.pdb 정보 경로를 설정한다.
5. 실행한 프로그램의 Process ID나 ProcessName 을 설정한다.
6. Play 버튼을 눌러 프로파일링을 시작한다.

# Reference

https://software.intel.com/en-us/articles/intel-vtune-amplifier-tutorials

https://software.intel.com/en-us/get-started-with-vtune
