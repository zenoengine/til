# Function Pointer 사용하기.

C++에서 포인터는 주소를 가르킨다. 흔히 우리가 보는 형태는 다음과 같았을 것이다.

```c
int * p = new int;
*p = 10;
```

이는 변수의 주소를 담는 포인터다.

함수를 담는 포인터를 Function Pointer라고 한다.

함수 포인터는 일반 변수를 담는것과는 약간 다르게 선언하는데 아래와 같이 선언한다.

```
반환형 (* 포인터변수이름) (함수의 매개 변수들);
```

 다음과 같이 선언한다.

```c
#include <iostream>

using namespace std;
int MyFunc()
{
	return 1;
}

int main()
{
	int(*p)(void);
	p = MyFunc;

	cout << p();

	return 0;
}
``` 

# 응용 예 - Jump Table

이를 이용해서 다양한 테크닉에 사용 할 수잇다.

예를 들면 함수 jump table을 만들 수 있다.

switch vs if에도 다룰 내용이지만,

컴파일러는 switch 구문이 특정한 조건을 충족하면 아래 코드와 같은 jump table을 생성해서 만들기도하고 안만들기도 한다. 

switch가 if처럼 O(N)번의 비교를 통해 해당 코드로 jump하기도 할 수 있다는 뜻이다.

때문에 빈번하게 사용되는 패킷처리나 이벤트 처리에 명시적으로 jump table을 구현하고싶을때 유용한 테크닉이다.

```c
#include <iostream>

using namespace std;
void Func1()
{
	cout << "my func 1";
}

void Func2()
{
	cout << "my func 2";
}

void Func3()
{
	cout << "my func 3";
}

void(*funcs[3])() = { Func1, Func2, Func3 };

int main()
{
	int cmd = 0;

	cin >> cmd;
	
	if (cmd <= 3)
	{
		funcs[cmd-1]();
	}

	return 0;
}
```

# Function Map

jump table의 경우 정적이지만 O(1)의 시간복잡도로 원하는 코드로 jump 할 수 있다는 점이 장점이있다.

그러나 정적이며 필요하지 안흔 공간을 많이 차지할 수도 있는 단점을 가지고 있다.

또한, 프로그래밍을 하다보면 특정한 상황일 때 해당 코드로 점프를 하고싶지 않을때가 있다. 예를 들면, 버튼이 비활성화 된다면 클릭 이벤트와 관련된 코드 처리를 하고싶지 않을 때 가 있을것이다.

bool 변수로 하나 추가해서 체크해도 되겠지만, Activate/Deactivate 될 때 관련 function pointer들을 Map에 erase, insert하는 방식으로 구현할 수 도있다.

즉, 동적인 Function Event Map 만들수 있는 것이다.

아래의 코드 예는, bUseFunc3 입력된 bool 변수의 값에 따라 Func3에 대에 동적으로 처리하는 코드를 넣을지 말지에 대한

간단한 예이다.

```c
#include <iostream>
#include <map>
using namespace std;
void Func1()
{
	cout << "my func 1";
}

void Func2()
{
	cout << "my func 2";
}

void Func3()
{
	cout << "my func 3";
}

typedef void(*func)();
typedef int command;
map<command, func> commandMap;

int main()
{
	int cmd = 0;


	commandMap.insert(make_pair(0, Func1));
	commandMap.insert(make_pair(1, Func2));

    bool bUseFunc3 = true;
    cin >> bUseFunc3;

    if(bUseFunc3)
    {
        commandMap.insert(make_pair(2, Func3));
    }
	

	cin >> cmd;
	
	auto it = commandMap.find(cmd);
	if (it != commandMap.end())
	{
		it->second();
	}

	return 0;
}
```
