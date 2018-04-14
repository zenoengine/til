# typedef

이전부터 우리는 type을 다른 별명으로 호출하기 위해서 typedef를 많이 사용 해왔다.

예를들면 다음과 같은 코드들이 있다.

```c
typedef int INT;
typedef unsigned long       DWORD;
typedef int                 BOOL;
typedef unsigned char       BYTE;
typedef unsigned short      WORD;
```

# using 

using이라는 키워드는 선언 사용, using 지시문, 그리고 별칭 및 typedef에 사용 할 수 있다.

특히 C++ 11에서의 using은 기존 typedef처럼 Alias를 부여할 수 있다.

```c
#include <iostream>
using namespace std;

using INT = int;

int main()
{
	INT a;
	a = 0;
	
	return 0;
}
```

사실 보기에는 typedef와는 똑같아보인다. 무엇이 다를까?

# 차이점

차이점은 기존의 typedef는 type만 Alias를 부여 할 수 있다.

다음의 코드를 살펴보자.

```c
#include <iostream>
using namespace std;

using INT = int;

template<typename T, typename U>
struct Pair
{
	T first;
	U second;
};

typedef Pair<int, int> MyIntIntPair;

int main()
{
	MyIntIntPair a;
	
	return 0;
}
```

위 코드는 Pair라는 Template 구조체를 생성해서, typedef를 이용해 Pair의 first - int,  second - int라는 결정된 **타입** 에 대해서 Alias를 부여하였다.

그렇다면, 첫번째 값은 int 타입,  두번째 값은 Template 형태로 만들고싶다면 다음과 같이쓰면 될까?

```c
template<typename V>
typedef Pair<int, V> MyIntTemplatePair;

int main()
{
	MyIntTemplatePair<float> a;
	
	return 0;
}
```

이렇게 쓰면 다음과 같은 에러가 출력된다.

```
error C2823 : a typedef template is illegal
```

이제 using Keyword를 써서 다음과 같이 Template Alias를 사용 할 수 있다.

```c
template<typename V>
using MyIntTemplatePair = Pair<int, V>;

int main()
{
	MyIntTemplatePair <float> a;
	
	return 0;
}
```
