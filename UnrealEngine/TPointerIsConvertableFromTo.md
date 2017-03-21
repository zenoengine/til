# 컴파일 타임에 Dynamic Cast 가능 여부 체크

Unreal engine 4 source code를 보다가 재미있는 방법이 있어서 소개드립니다.

컴파일 타임에 A객체와 B객체가 부모, 자식 관계로 캐스팅이 가능/불가능한지 컴파일 타임시에 체크하는 방법입니다.


```c
template<typename From, typename To>
struct TPointerIsConvertableFromTo
{
prviate:
static uint8 Test(...);
static uint16 Test(To*);

public:
enum { Value = sizeof(Test((From*)nullptr)) - 1);
}
```

위 코드를 분석해보면

만약 From과 To가 부모/자식 관계라면

enum { Value = sizeof(Test((From*)nullptr)) - 1); 

이 코드에서 static uint16 Test(To*) 이 함수의 sizeof 연산 결과 enum Value의 값이 1이 됩니다. (2-1)



```c
template<typename From, typename To>
struct TPointerIsConvertableFromTo
{
prviate:
static uint8 Test(...);
static uint16 Test(To*);

public:
enum { Value = sizeof(Test((From*)nullptr)) - 1); //이 코드에서 static uint16 Test(To*) 이 함수의 반환 값
}
```

만약 From과 To가 부모 자식 관계가 아니라면 static uint8 Test(...)의 sizeof를 계산해 1-1 = 0 이 되어

enum Value의 값이 false가 됩니다.

이를 이용해서 코드에 다음과 같이 사용 할 수 있습니다.

```
static_assert(TPointerIsConvertibleFromTo<T, const UActorComponent>::Value, "'T' template parameter to FindComponentByClass must be derived from UActorComponent");
```

참 재미있고 기발한 방법인것 같습니다. :)