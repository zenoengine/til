# Memory Allocation API

Windows 운영체제에서 메모리 할당 API를 이해하는것은 중요합니다.

만약 메모리 관리를 요하는 언어를 사용할 계획이라면(C++ 같이) 아래에 있는 그림이 도움이 될것입니다.

![](https://i.stack.imgur.com/mMHbW.png)

Windows 운영체제에 특화된 관점이며

이 다이어그램을 보는 방법은 위로 갈수록 고차원적인 구현이 이루어져있습니다.

아래 부분부터 살펴봅시다

# Kernel-Mode memory manager

커널 모드 메모리 매니져는 운영체제에서 메모리를 예약하고 할당하는 기능을 제공합니다.

잘 알려진 Memory Mapped file과 Shared memory, copy-on-write 기능들도 지원하며

직접적으로 User-Mode code에서 접근이 불가합니다. 그러므로 간단하게만 소개하고 넘어갑니다.


# VirtualAlloc

Windows API로 많은 옵션을 제공합니다. 

이 함수는 주로 특별한 상황에 사용합니다.

큰 메모리 청크를 할당할 때 사용할 수 있습니다.

가장 일반적인 상황은 메모리를 다른 프로세스와 공유해야 할 때 사용 할 수 있습니다.

Windows OS에서 가상 메모리(VVM) 시스템을 사용하기위해 고안된 할당 방법입니다.

User Mode에서 사용 가능한 가장 낮은 수준의 API이며 VirtualAlloc 함수는 기본적으로 ZwAllocation를 호출합니다.

이는 User Mode에거 나으한 메모리 블록흘 예약/할당하는 가장 빠른 방법입니다.

하지만 이함수를 사용하려면 크게 두가지 조건이 필요합니다.

 - 시스템 Granularity(세분성) 크기로  정렬된 메모리 블록으로만 할당합니다.
 - 시스템 Granularity(세분성) 크기의 배수로 정렬된 메모리 블록으로만 할당합니다.

 ```
 System Granularity 란?

 GetSystemInfo 함수를 통해 반환된 값중에서 dwAllocationGranularity 파라메터의 값입니다.

 이 값은 하드웨어나 구현에 따라 다르지만, 많은 64bit windows가 0x10000 byte나 64k로 설정됩니다.
 ```

위의 조건을 살펴보기 위해서 가정을 하나 해봅시다.

여러분이 8 byte memory block을 할당하고싶어서 아래와 같은 코드를 작성 합니다..

```cpp
 void* pAddress = VirtualAlloc(NULL, 8, MEM_COMMIT | MEM_RESERVE, PAGE_READWRITE);
 ```

함수 호출이 성공했다면, pAddress는 0x10000 바이트 경계에 정렬되어 할당됩니다.

그리고 8byte memory만을 할당 요청했지만 실제 메모리 블록은 page단위로 할당됩니다. (정확한 페이지 사이즈는 dwPageSize 파라메터를 동해서 얻을 수 있습니다.)

하지만, pAddress로부터 0x10000 바이트 (혹은 대부분 64k) 공간은 어떤 할당도 할 수 없습니다. 8 byte만을 할당하는것이 65536을 할당하는것과 같은 뜻이 되어버립니다.

이런 이류오 VirtualAlloc은 아주 특별한 경우에 사용합니다. VirtualAlloc을 잘못 사용한다면 메모리 파편화 현상을 야기할것입니다.

VirtualFree 함수를 사용해서 매모리를 해제합니다.

# HeapAlloc

 VirtualAlloc처럼 큰 사이즈가 아니어도 어떤 사이즈든 당신이 원하는 메모리를 할당합니다.

HeapAlloc 내부적으로 VirtuialAlloc을 자동으로 호출해야 할때를 알고있습니다. malloc 처럼요,

그러나 Windows-only 함수이며, 다양한 옵션을 제공합니다.

일반적인 메모리 청크를 할당하기에 적합합니다. 몇 Windows API는 이 함수를 사용해서 생성한 메모리를 넘겨주기를 바랄것입니다.

HeapFree 함수를 통해서 메모리를 반환합니다.

# malloc

C 언어에서 메모리를 할당하는 방법입니다. 

C++에서도 사용가능하지만, C 언어 스타일을 선호하신다면 사용가능합니다. 

유닉스 계열 Computer에서도 같은 코드로 동작하거나, 누군가 특별히 사용해야한다고 말할 경우 malloc을 사용하세요

초기화 되지 않은 메모리를 할당합니다. 

free를 통해서 메모리를 해제합니다.

# new

메모리를 C++ 스타일로 할당하고싶다면 new를 사용하세요.

할당된 메모리에 객체를 넣습니다.

Visual studio에서 new는 HeapAlloc을 호출하며, 당신이 어떻게 호출했느냐에 따라 객체를 초기화합니다. 

C++11이상의 표준에서는 스마트포인터를 이용해 자동으로 메모리를 해제할 수 있으며 수동으로 해제시

delete 혹은 delete[]를 통해서 메모리를 해제할 수 있습니다.

## Reference

[What's the differences between VirtualAlloc and HeapAlloc?](https://stackoverflow.com/questions/872072/whats-the-differences-between-virtualalloc-and-heapalloc)