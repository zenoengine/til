# Uniform initalize

옛 C++의 초기화 문법은 다음과 같이 다양했다.

C++ 11부터는 모두 중괄호 앞에 평등하다 (...)

변수, 객체, 배열 등 모두 중괄호로 초기화가 가능하다.

```C
int i = 3;
int arr[] = { 1,2,3,4,5 };

std::vector<int> v;
for(int i=0; i<5; i++) { v.push_back(arr[i]); }

std::set<int> s;
for(int i=0; i<5; i++) { s.insert(arr[i]); }

std::map<int, std::string> m;
m[0] = "zero";
m[1] = "one";
m[2] = "two";

vector<int> v;
v.push_back(10);
v.push_back(20);
v.push_back(30);
v.push_back(40);
int total = totalElementsInVector(v)
```


```c
int i = {1};
int arr[]          { 1,2,3,4,5 };
std::vector<int> v { 1,2,3,4,5 };
std::set<int> s    { 1,2,3,4,5 };
std::map<int,std::string> m { {0,"zero"}, {1,"one"}, {2,"two"} };

int total = totalElementsInVector({10,20,30,40});
```

특히 이 문법에 마음에 드는 포인트는 다음이다.

기존에 C++에서 실수 할 수 있는 부분을 막아준다.

```c
int a1 = 3.4; // float->int로 암시적 형변환, 소수점 데이터 손실
char c1 = 512; // overflow 0

int a2 = {3.4}; // error
char c2 = {512}; // error
```

