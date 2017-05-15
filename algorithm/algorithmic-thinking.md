# 알고리즘

알고리즘이란 어떠한 문제를 해결하기 위한 여러 모임동작들의 모임이다.

여기서 문제란, 전삭학적 문제뿐만아닌 모든 일상 생활속에 접할 수 있는 문제도 포함한다.

회사에서 집을 가기 위해 버스,택시 선택지가 있다고 가정하면

버스 30분 1200
택시 10분 12000원 
 
회사에서 집을 가기 위해 버스와 택시를 탈때 가격을 중요시한다면, 버스, 시간이 중요하다면 택시를 탈것이다.

즉, 알고리즘이란 상황에 따라 문제를 해결하는 방법이 달라질 수 있다.

# 알고리즘 사이트

알고스팟
Coderforce
topcoder

# 읽어보면 좋은 글

알고리즘 문제 해결 전략 책의 서문

# 공부하는 방법

```
1. 먼저 알고리즘이나 문제 푸는 방법을 이해

2. 관련 문제를 풀어본다.

3. 1,2번에서 이해가 잘 안가는 부분이 있따면 질문한다.

4. 다시 알고리즘을 이해해보고 문제를 다시 풀어본다.
```

# 가장 중요한 점

프로그래밍을 많이 하는것도 중요하지만, 가장 중요한 것은 생각을 많이 하는 것

# 프로그래밍 언어

많이 사용하는 언어 C++ > C > JAVA

C언어를 사용하는경우 C++을 추천,

C++을 사용하는 경우 C++11, stl, scanf/printf를 사용하는 것이 좋다.

 
 # 시간 복잡도
 
 작성한 코드 입력값에 따라 시간이 얼마나 걸릴지 예상할 수 있다.
 
 표기법 : 대문자 O (영어로는 Big O Notation)
 
 # 시간 복잡도 계산
 
 Big O Notation에서 상수는 버린다.
 
 두가지 항이 있을 때 변수가 같으면 큰 것만 빼고 다 버린다.
 
 두 항이 있는데, 변수가 다르면 놔둔다.
 
 # 입/출력
 
 # EOF
 
fgetc, getchar 등 함수가 파일의 끝에 도달하는 경우 반환

End of file의 약자. 파일의 끝을 표현하기 위한 상수

콘솔의 경우 Ctrl-Z가 파일의 EOF를 의미

eof check 방벙
```
scanf() == -1

(cin >> a) == false
```
 
 # stdin, stdout, stderr, stream
 
실행중인 프로그램은 여러 가지 장치들과 데이터를 주고받기위해 stream이라는 소프트웨어적 다리를 놓는다.

stdin과 stdout은 콘솔과 데이터를 주고 받는 표준 스트림이다. 

|이름|용도|
|---|---|
|stdin| 표준 입력 스트림|
|stdout| 표준 출력 스트림|

