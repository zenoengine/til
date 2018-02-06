# if statement

C++ 17부터 추가되는 좋은 기능을 소개하고자 한다.

짧게 코드로 설명하겠다.

다음과 같은 코드를

```C

bool ret = foo();

if(ret)
{
    cout << "success";
}

```

이렇게 사용 할 수 있다.

```c

if(auto ret = foo(); ret) // (변수 할당; 상태 체크)
{
    cout << "success";
}

```

생성된 ret은 if의 scope에서만 존재한다.

이 문법이 마음에 드는 이유는

1. 스택을 더 효율적으로 사용 가능 하다는 점
2. 스코프가 줄어들면서 ret을 재사용하면서 생기는 실수를 방지한다는 점이다.
