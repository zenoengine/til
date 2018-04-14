# 쿠다 튜토리얼 Cuda C/C++ Basics

# Cuda란?
CUDA ("Compute Unified Device Architecture", 쿠다)는 그래픽 처리 장치(GPU)에서 수행하는 (병렬 처리) 알고리즘을 
C 프로그래밍 언어를 비롯한 산업 표준 언어를 사용하여 작성할 수 있도록 하는 GPGPU 기술입니다.
이번 튜토리얼에서 우리는 C/C++ 프로그래밍 언어를 가지고 쿠다를 배워 볼 것입니다.

# 사전 지식
C/C++에 대한 경험이 있으면 좋습니다.

다음 항목들은 반드시 필요하지는 않습니다. (GPU, 병렬 프로그래밍, graphics)

# Heterogeneous Computing

두개의 계산기를 사용합니다. 앞으로 쓰일 용어는 다음과 같습니다.

Host : CPU와 CPU 메모리를 의미합니다. (or Host Memoery)
Device : GPU와 GPU메모리를 의미합니다. (ort Device Memeory)

# 간소화된 처리 과정

1. CPU 메모리에 존재하는 input 데이터를 GPU 메모리에 복사합니다

2. GPU가 코드를 로드하고 실행합니다. 성능 향상을 위해 gpu 상에 데이터가 캐시됩니다.

3. 수행한 결과 값을 gpu 메모리에서 cpu로 복사합니다.

# Hello World

아래 코드는 아주 일반적인 C 코드로 Host에서 동작합니다.

```c
int main()
{
    printf("Hello World! \n");
    return 0;
}
```

 이 코드는 Device Code 가 없지만, NVIDIA Compiler(nvcc)로도 컴파일할 수 있습니다.

 # Hello World With Device Code

 이제 Device Code를 추가된 코드를 보죠, 두가지 문법 요소가 추가되어있습니다.

 ```c
 __gloval__ void mykernel(void) {}

 int main(void)
 {
     mykernel<<<1,1>>>();
     printf("Hello World! \n");
     return 0;
 }
 ```

 __global__ : CUDA C/C++ keyword로 이 키워드를 앞에 붙인 함수는 다음과 같은 의미를 가집니다.
  1. Device에서 실행됩니다.
  2. Host에서 호출합니다.

### nvcc는 코드를 host 부분과 device에 들어갈 부분을 분리합니다.
 - Device functions(예, mykernel())은 NVIDIA compiler에 의해 처리됩니다.
  - Host functions(예, main) 일반 compiler에 의해 처리됩니다.

```
mykernel<<<1,1,>>>();
```
꺾쇠 괄호(angle brackets) 3번은 host code가 device code를 호출한다고 마킹하는것입니다.
- kernel launch이라고도 부릅니다.
- 인자값으로 (1,1)이 넘어갑니다.

__global__ keyword와 <<<>>>, 이 2가지 가 GPU에서 함수를 실행하기 위한 모든 것입니다.

이제 다시 코드를 봅시다.

 ```c
 __gloval__ void mykernel(void) {}

 int main(void)
 {
     mykernel<<<1,1>>>();
     printf("Hello World! \n");
     return 0;
 }
 ```

mykernel()함수는 아무것도 하지않습니다.

조금 시시한가요?

다음 편에서는 조금 더 흥미로운 예제를 가지고 진행해보도록하겠습니다.