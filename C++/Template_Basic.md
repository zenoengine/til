# template

예로부터 많은사람들에게 C++의 Template 릿이라고 하면 뭔가 복잡하고 읽기 힘들다는 이미지가 있다.

필자 또한 메타 템플릿 프로그래밍이 유행하던 시절 템플릿을 마개조해서 사용하느라 읽기 힘들고 유지보수하기도 어렵다느 소리를 들어왔기에

템플릿에 대한 부정적인 이미지를 가지고 있었지만, 템플릿이란 존재를 언제까지 외면할 수 없을 뿐더러 꼭 사용해야하는 기능들도 있기에 템플릿에 대해 알아보기로 하였다.


# 오버로딩


코드를 짜면서 제곱 수를 반환하는 Square라는 함수를 만들었다.


```c
int Square(int n)
{
    return n*n;
}

int main()
{
    Square(1);
    return 0;
}
```

그런데, Square의 argument가 double형이길 바란다면? 어떻게 해야 할까?

쉽게 생각하면, 우리가 알고있는 함수 오버로딩을 사용해서 double형을 받아 반환하는 함수를 만들수 있다.


```
double Square(double n)
{
    return n*n;
}

int Square(int n)
{
    return n*n;
}

int main()
{
    Square(1);
    return 0;
}
```

그런데, float 형이나 char형으로 반환하고싶다면? 또 만들어줘야 할 것 이다.

조금 번거로우므로 더 좋은 방법을 생각해보면, 

define을 사용해서 함수를 만드는 틀을 정의해두고 코드를 생성할수 있을것이다.


```
#define MAKE_SQUARE_FUNCTION(X) \
X Square(X n) \ 
{ \
    return n*n; \
} \

MAKE_SQUARE_FUNCTION(int)
MAKE_SQUARE_FUNCTION(double)
MAKE_SQUARE_FUNCTION(float)
MAKE_SQUARE_FUNCTION(char)

int main()
{
  return 0;
}
```

자, 이제 전처리가 대신 코드를 생성해 줄 것이고 코드가 조금 간결(?) 해졌다.

그런데 미리 정의해두고 함수를 사용 안했는데도 미리 코드를 만들어두어야한다.

비효율적이지 않은가? 조금 더 나은 방법은 없을까?

이제 언어에서 제공해주는 template을 사용해서 코드를 만들어보자.

# Template

template은 일종의 코드를 생성하는 틀이라고 생각 할 수있다.
템플릿을 사용하면 다음과 같이 사용할 수 있다.

```
template<typename T>
T Square(T n)
{
 return n*n;
}

int main()
{
	Square(4);
	return 0;
}

```

여기서 눈여겨 볼 점은 템플릿을 사용하면, 전처리기가 아닌 컴파일러가 코드를 생성한다.

따라서 매크로를 이용해서 생성한 것과는 차이점을 가진다.

1. Square의 매개변수를 보고 타입을 **추론**해서 생성한다.(Template Type Deduction)
2. 사용하는 곳이 있어야 템플릿 코드를 생성한다. (Template Instantiation)

오늘은 간략하게 템플릿에 대해 정리하였다.

마지막에 등장한 Template Type Deduction과 Template Instantiation에 대한 것은 다음에 또 다뤄보도록 하자.