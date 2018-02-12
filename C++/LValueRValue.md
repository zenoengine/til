# LValue와 RValue

lvalue와 rvalue를 구분하는 가장 기초적인 방법은 등호(=)를 기준으로 좌변, 우변의 차이에 따라 구분하는 것이다. (Left, Right Value)

아래의 코드에서 x는 lvalue, 10은 rvalue이다.

```c
int x = 0;
x = 10;
```

다음의 코드를 살펴보자.

```c
int x = 0;
int y = 5;

x = y;
```

x=y라는 코드에서 x는 lvalue, y는 rvalue라고 불러야 할까?

사실 x, y 둘다 lvalue이다.

따라서 가장 기초적인 구분에서 더 나아가 구분하는 방법을 살펴보려한다..

lvalue rvalue를 구분하는 특징은 다음과 같다.

### lvalue의 특징
```
1. lvalue는 좌변에 놓일수도 있고, 우변에 놓일수도 있다.
2. lvalue는 이름을 가진다.
3. 사용하는 식 외에서도 유효하다.
```

### rvalue의 특징
```
1. rvalue는 우변에만 놓인다.
2. rvalue는 이름이 없다.
3. rvalue를 사용하는 식에서만 유효하다. (임시 객체, 임시 값)
```

이제 다음 코드를 살펴보자.

```c
int x = 0;
int y = 5;

x = y;
y = x;
```

x,y 는 lvalue의 모든 특징을 가진다.

1. x,y는 둘다 좌변에도 우변에도 올 수 있다.
2. x,y라는 이름이 지정되어있다.
3. x =y;, y=x; 라인 외에서도 유효하게 사용 가능하다.

이제 다음 코드를 살펴보자.

```c

struct Goo
{
    int x;
}

int Foo() 
{
     return 1; 
}

int main()
{
    int y = 5;

    y = Foo(); // 1
    Foo() = y; // 2

    Goo a;
    a.x = 10; // 3
    Goo().x = 5 // 4

    return 0;
}
```

1번에서 Foo() 함수를 통해서 반환한 값은, 해당 라인에서만 유효하며 이름이 지정되어있지 않다.
따라서 2번 라인을 컴파일하면 error 가 난다.

3번 Goo 구조체를 생성하면서 이름을 a로 부여하고, 멤버 변수 x의 값에 접근이 가능했지만,

4번에서 임시 객체 Goo를 생성하게 되면, 이 임시객체 Goo는 rvalue가 된다.  

이름이 없으며, 해당 라인에서만 유효하고, 우변에만 올 수 있다.

# RValue Reference

RValue에 사용하는 임시 메모리를 한 라인에서 밖에 사용하는 것이 비효율적일 수 도 있다.

(예를 들면, 큰 객체를 임시 객체로 생성하는 경우)

이 RValue값을 재사용할 수 없을까?

Rvalue Reference를 사용하면 재사용이 가능하다.

RValue Reference는 일반 LValue Reference에 & 를 하나 더 붙이는 방법으로 선언한다.
```c
#include <iostream>
using namespace std;

int main()
{
    int a = 10;
    int& b = a; // lvalue reference
    int&& c = 5; // rvalue reference
    int&& d = a // error, rvalue reference는 lvalue를 받지 못한다.

    a = c;
    cout << a;
    return 0;
}
```
**Rvalue Refernece를 사용하게 되면, RValue의 값의 생명 주기(유효성)는 rvalue reference와 동일해진다.**

그리고 재미있는 점은, Rvalue Reference는 lvalue일까? rvalue일까?

int&& c는 이름을 가지며, 좌변에도, 우변에도 올 수 있다. 또한 유효성도 한라인 이상을 유지한다.

따라서 Rvalue Reference는 lvalue이다.