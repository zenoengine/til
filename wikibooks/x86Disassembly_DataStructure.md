# x86 Disassembly/Data Structures

stack overflow에 관련된 자료를 찾아보던 중, 어셈블리를 공부하지 않았던 저같은 초보자에게도 좋은 자료가 있어 한국어 [페이지](https://ko.wikibooks.org/wiki/X86_Disassembly/Data_Structures)를 만들었습니다. 

# 자료 구조

어떤 프로그램들은 단순한 메모리 구조상에서 동작합니다. 그러나 많은 수의 프로그램들은 복잡한 데이터 타입과 객체를 사용합니다. 이 페이지에서는 어떻게 컴파일러가 복잡한 객체가 사용된 코드들을 어셈블리로 만드는지에 대해, 또 어떻게 리버스 엔지니어링 하는 사람들이 어셈블리화된 코드를 읽을 수 있는지에 대해 살펴보겠습니다

# 배열
아시다시피 배열은 단순히 여러 객체의 저장소입니다. 데이터 객체들은 순차적으로 저장되며, 시작 지점부터의 Offset을 이용해 접근하기도 합니다.
다음 C 코드를 살펴보죠.

```c
x = array[25];
```

위의 코드는 asm 코드로 바꾸면 다음과 같습니다.

```c
mov ebx, $array;
mov eax, [ebx + 25]
mov $x, eax
```

이제 아래 예제를 살펴봅시다.

```c
int MyFunction1()
{
    int array[20];
}
```

아래는 위의 코드를 간결한 수도 asm 코드로 바꾼것입니다.
```
:_MyFunction1
push ebb
mov ebp, esp
sub esp, 80 ;전체 배열이 stack에 생성되었습니다!!
lea $array, [esp+0] ; 배열 변수에 배열 포인터가 저장되었습니다.
```

배열의 시작점이 스택에 생성되었고, 배열의 시작 주소가 변수 array에 저장되었습니다.
최적화 옵션을 켠 컴파일러는 마지막 명령을 무시하고 (이 예제에서는) esp에 저장된 값에 Offset 연산을 이용해서 참조하기만 할수도 있습니다.

마찬가지로 다음 예제를 살펴봅시다.

```c
void MyFunction2()
{
    char buffer[4];
}
```
위 코드는 아래 asm 수도 코드로 변환됩니다.

```c
:_MyFunction2
push ebp
mov ebp, esp
sub esp, 4
lea $buffer, [esp + 0]
```

보기에는 문제가 없어보입니다만 만약, 프로그래머가 실수로 buffer[4]에 접근하면 어떻게 될까요? buffer[5]는요? buffer[8]은요? 이는 Buffer overflow의 원인이 되며 보안 취약점에 대해 이야기 할 것이 있습니다. 하지만 보안과 관련된 글이 아니므로 넘어가고 data structre에 대해서만 초점을 맞추도록 하죠.

# 스택에서 배열 참조
스택에서 배열 참조는, 즉 스택에 할당된 거대한 Local 저장소에서 배열의 값을 찾는것은(아래의 예제 참조),  데이터의 시작 지점을 esp에 저장하고 offset을 이용해서 접근합니다.

예를 들면 :
```c
 :_MyFunction3
 push ebp
 mov ebp, esp
 sub esp, 256
 lea ebx, [esp + 0x00]
 mov [ebx + 0], 0x00
 ```
스택에 배열을 만드는 좋은 예제입니다. 물론 최적화 옵션을 켠 컴파일러는 esp 대신 오프셋을 원할 수 있습니다.


 # 메모리에 있는 배열 참조
 메모리에 있는 배열(전역 변수 배열과 같은), 혹은 초기 데이터가 있는 배열(초기화 된 데이터의 메모리는 .data 섹션에서 생성 됨)과 같은 메모리에 있는 배열은 메모리의 하드 코드된 주소에서 Offset으로으로 접근합니다.

 ```c
 :_MyFunction4
 push ebp
 mov ebp, esp
 mov esi, 0x77651004
 mov ebx, 0x00000000
 mov [esi + ebx], 0x00
 ```

구조체와 클래스가 유사한 방식으로 접근 할 수 있다는 것을 명심해야합니다. 따라서 리버서는 배열의 모든 데이터 객체가 동일한 유형이고 순차적이며 자주 처리된다는 것을 기억해야합니다 어떤 종류의 고리에서. 또한 (가장 중요한 부분 일 수 있습니다.) 배열의 각 요소 는 기본 변수의 오프셋 변수 로 액세스 할 수 있습니다 .
대부분의 경우 배열은 상수가 아닌 계산 된 인덱스를 통해 액세스되기 때문에 컴파일러는 다음을 사용하여 배열 요소에 액세스합니다.

```
 mov [ebx + eax], 0x00
```

배열이 1바이트보다 큰 Element를 보관하는 경우, index element에 사이즈가 곱해져 다음과 같은 코드가 생성 되야 할것입니다. 

```
 mov [ebx + eax * 4], 0x11223344   # DWORD 배열에 접근 arr[i] = 0x11223344
 ...
 mul eax, $20                      # 구조체 배열에 접근 (구조체 당 20bytes) 
 lea edi, [ebx + eax]              # e.g.     ptr = &arr[i]
```
이 패턴을 이용해 배열에 대한 액세스와 구조체 데이터 멤버에 대한 액세스를 구별하는 데 사용할 수 있습니다.

# 구조체

모든 C 프로그래머는 다음 문법에 익숙합니다.

```c
 struct MyStruct 
 {
    int FirstVar;
    double SecondVar;
    unsigned short int ThirdVar;
 }
```
이것은 구조체로 불립니다.(파스칼 프로그래머들은 아마도 Record의 개념과 유사한것으로 알고 있을 것입니다.) 구조체는 아주 작을수도 있고, 아주 클 수도 있습니다. 그리고 아마도 다른 데이터 타입들을 포함하고 있을 것입니다.
하지만 몇 가지 키 포인트만 알아두면 됩니다. 구조체 속에 데이터는 같은 데이터 타입일 필요가 없으며, 대부분 4바이트로 (딱 붙지 않게)정렬됩니다. 그리고 각 구조체 내부의 요소들은 고유의 offset을 가지고 있습니다. 그러므로 offset base 기반의 접근을 만드는것에 대해 이해하기가 어렵지 않습니다.

아래 구조체의 정의를 살펴보죠.

```c
 struct MyStruct2
 {
    long value1;
    short value2;
    long value3;
 }
```

이 구조체의 시작 포인터가 ebx에 로드되었다고 가정하면, 다음 두가지 방법중 하나로 멤버 변수에 접근 할 수 있습니다.


1번 방법
```c
 ;data 는 32 bit 정렬
 [ebx + 0] ;value1
 [ebx + 4] ;value2
 [ebx + 8] ;value3
```

2번 방법
```c
 ;"packed"된 데이터 (#Pragma Pack)
 [ebx + 0] ;value1
 [ebx + 4] ;value2
 [ebx + 6] ;value3
 ```

첫 번째 방법에서 보여지는 구조체의 Alignment가 일반적이지만, offset 6바이트에서 남은 2바이트는 전혀 사용 할 수 없습니다. (4바이트씩 정렬하기 때문에 short로 2바이트를 채우고 남은 공간을 사용할 수 없습니다.)
때때로 프로그래머가 각 데이터 멤버의 오프셋을 수동으로 지정 할 수록 있도록 허용하기도 하지만, 항상 그러지는 않습니다. 두번째 방법 예제는 리버스 엔지니어링 하는 사람이 쉽게 데이터 멤버의 크기가 다른 사이즈라는 것을 알 수 있다는 차이점이 있습니다.

이제 다음 함수를 살펴봅시다.
```c
 : _MyFunction 
 push  ebp 
 mov  ebp ,  esp 
 lea  ecx ,  SS : [ ebp  +  8 ] 
 mov  [ ecx  +  0 ],  0x0000000A 
 mov  [ ecx  +  4 ],  ecx 
 mov  [ ecx  +  8 ],  0x0000000A 
 mov  esp ,  ebp 
 pop  ebp
```

이 함수는 첫 번째 Argument로 자료 구조에 대한 포인터를 명확하게 취합니다. 그런데 각 데이터 멤버는 같은 크기 (4 바이트)이므로 배열이나 구조체인지 어떻게 알 수 있을까요? 

이 질문에 답하기 위해 구조와 배열 사이의 중요한 차이점을 기억해야합니다. 배열의 Element 모두 같은 데이터 타입이고 구조체는 Element 같은 데이터 타입일 필요는 없습니다. 이 규칙을 감안할 때이 구조의 요소 중 하나는 포인터 (자료구조 자체의 시작 지점을 가리키며)이며 다른 두 필드는 16 진수 값 0x0A (십진수 10)로 로드됩니다. 확실히 내가 사용했던 모든 시스템에서 유효한 포인터가 아닙니다. 그러므로 우리는 다음 아래의 구조체와 함수 코드를 부분적으로 다시 만들 수 있습니다

```c
 struct MyStruct3
 {
    long value1;
    void *value2;
    long value3;
 }

 void MyFunction2(struct MyStruct3 *ptr)
 {
    ptr->value1 = 10;
    ptr->value2 = ptr;
    ptr->value3 = 10;
 }
```

주의 할 점은 이 함수는 eax에 아무 것도로드하지 않으므로 값을 반환하지 않는다는 것입니다.

# 구조체 심화

아래의 함수를 살펴봅시다.

```c
  :MyFunction1
 push ebp
 mov ebp, esp
 mov esi, [ebp + 8]
 lea ecx, SS:[esi + 8]
 ...
 ```
 무슨일이 일어난 걸까요? 첫번째 esi가 함수의 첫번째 parameter로 로드되었습니다.
 그런다음, ecx가 esi에 + 8 offset 을 더한 구조체 포인터가 로드되었습니다. 보기에는 2개의 포인터가 같은 데이터 구조를 갖고있는 것으로 보이는군요!

 위의 함수를 다음 2가지 원형으로 추론할 수 있습니다.

 ```c
  struct  MyStruct1 
 { 
   DWORD  value1 ; 
   DWORD  값 2 ; 
   struct  MySubStruct1 
   { 
      ... 
 ``` 

 ```c
  struct  MyStruct2 
 { 
    DWORD  value1 ; 
    DWORD  값 2 ; 
    DWORD  배열 [ 길이 ]; 
    ...
 ```

구조체에서 다른 포인터로부터 Offset 된 포인터는 종종 복잡한 데이터 구조를 의미합니다. 
그러나 구조체와 배열의 나올 수 있는 조합 너무 많기에 너무 시간을 할애하지 않을 것입니다.



# 구조체와 배열의 식별

배열 Element와 구조체 필드는 모두 배열 / 구조체 포인터의 오프셋으로 액세스됩니다. 디스어셈블리 할 때,  어떻게 배열과 구조체를 를 구별 할 수 있을까요? 다음은 구별 할 수 있는 몇 가지 요소들입니다.

1. 배열 요소는 개별적으로 액세스 할 수 없습니다. 배열 요소는 일반적으로 가변 오프셋을 사용하여 액세스됩니다.
2. 배열은 루프에서 자주 액세스됩니다. 배열은 일반적으로 일련의 비슷한 데이터 항목을 보유하므로 일반적으로 모두에 액세스하는 가장 좋은 방법은 루프입니다. 특히 for(x = 0; x < length_of_array; x++)스타일 루프는 배열을 액세스하는 데 종종 사용되지만 다른 배열이있을 수도 있습니다.
3. 배열의 모든 요소는 동일한 데이터 유형을가집니다.
4. 구조체 필드는 일반적으로 상수 오프셋을 사용하여 액세스합니다.
5. 구조체 필드는 일반적으로 순서대로 액세스하지 않으며 루프를 사용하여 액세스하지도 않습니다.
6. 구조체 필드는 일반적으로 모두 동일한 데이터 유형 또는 동일한 데이터 폭이 아닙니다.

# Linked List 와 Binary Tree

프로그래밍 할 때 자주 사용되는, 또 널리 알려진 자료구조인 링크드 리스트와 이진 트리입니다. 이 두 자료 구조는 여러 가지면에서 만드는 데 더 복잡해질 수 있습니다. 아래 그림은 연결된 목록 구조와 이진 트리 구조의 예입니다.

![linked-list-img](https://upload.wikimedia.org/wikipedia/commons/1/1b/C_language_linked_list.png)

![binary-tree-img](https://upload.wikimedia.org/wikipedia/commons/thumb/4/45/Tree-data-structure.svg/1024px-Tree-data-structure.svg.png)

Linked list 또는 Binary Tree에 각 노드에는 일정량의 데이터와 다른 노드에 대한 포인터가 (또는 포인터들이) 들어 있습니다. 다음 asm 코드 예제를 살펴보죠.

```c
loop_top : 
cmp  [ ebp  +  0 ],  10 
je  loop_end 
mov  ebp ,  [ ebp  +  4 ] 
jmp  loop_top 
loop_end :
```

각 루프 반복에서 [ebp + 0]의 데이터 값은 값 10과 비교됩니다. 두 값이 같으면 루프가 종료됩니다. 그러나 둘이 같지 않으면 ebp의 포인터가 ebp의 오프셋에서 포인터로 업데이트되고 루프가 계속됩니다. 이것은 고전적인 Linked list 순회 탐색 기술입니다. 이것은 다음과 같은 C 코드와 유사합니다.

```c
struct  node  
{ 
  int  data ; 
  struct  node  * next ; 
};


struct node *x;
... 
while ( x -> data  ! =  10 ) 
{ 
  x  =  x -> next ; 
}
```

이진 트리는 두 개의 다른 포인터 (오른쪽 및 왼쪽 분기 포인터)가 사용된다는 점을 제외하면 동일합니다.