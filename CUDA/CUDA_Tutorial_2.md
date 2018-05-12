# CUDA Tutorial 2

지난 Tutorial 1은 간단한 예제였습니다만, 이번 튜토리얼에서는 더 재미있는 것을 만들어봅시다.

오늘 구현할 것은 병렬적 덧셈입니다.

# Device Code

가장 간단한 덧셈 함수 부터 시작해봅시다.

```c
__global__ void add(int *a, int *b, int *c) 
{

}

```

지난 시간에 이야기한 __ global __ keyword가 보이시나요?

__ global __ keyword를 사용하면 add() 라는 함수는 다음과 같은 의미를 지닌다고 했습니다.

```
add()는 Host가 호출하며, GPU에서 실행되는 코드이다
```

이제 실제 내용을 구현해 보죠.

```c
__global__ void add(int *a, int *b, int *c) 
{
    *c = *a + *b;
}
```

문제가 없어보이죠? 그런데 여기에는 함정이 있습니다.

add() code는 CPU Memory가 아닌, Device Memory에서 실행된다는 점입니다.

아시다시피 CPU Memory와 GPU Memory는 별도의 공간입니다. (RAM, Graphic 카드)

그러므로 우리가 add함수를 사용하려면 

GPU Memory에 변수들을 할당하고 파라메터로 넘겨줘야 한다는 뜻입니다.

## Memory Management CUDA 

Device Memory를 제어하기 위한 CUDA API 함수들은 다음과 같은 함수가 있습니다.

cudaMalloc(), cudaFree(), cudaMemcpy()

c언어에서 malloc(), free(), memcpy()와 비슷하죠?

이제 main 함수에서 구현된 예를 보도록 합시다.

```c
int main(void) 
{
    int a, b, c; // host 에서 사용하는 변수들입니다.
    int *d_a, *d_b, *d_c; // device에서 사용할 변수들입니다
    int size = sizeof(int);
 
    // device에 메모리를 할당합니다.
    cudaMalloc((void **)&d_a, size);
    cudaMalloc((void **)&d_b, size);
    cudaMalloc((void **)&d_c, size);

    // 입력 값을 설정합니다.
    a = 2;
    b = 7;

    // 입력값을 device memory 영역에 복사합니다.
    cudaMemcpy(d_a, &a, size, cudaMemcpyHostToDevice);
    cudaMemcpy(d_b, &b, size, cudaMemcpyHostToDevice);

    // add 함수를 gpu에서 실행합니다.
    add<<<1,1>>>(d_a, d_b, d_c);

    // 실행 결과를 cpu memory에 복사합니다.
    cudaMemcpy(&c, d_c, size, cudaMemcpyDeviceToHost);

    // gpu memory를 해제합니다.
    cudaFree(d_a); cudaFree(d_b); cudaFree(d_c);
    return 0;
}
```

# CUDA C/C++에서의 병렬 프로그래밍

GPU에서는 엄청난 병렬적 계산들이 이루어집니다.

우리가 만든 함수를 병렬로 실행하려면 어떻게해야할까요?

이제 <<<>>> 꺽쇠의 의미에 대해서 알아볼 때입니다.

```c
add<<<1,1>>>(); // 꺽쇠의 인자를
add<<<N,1>>>(); // N으로 바꾸었습니다.
```

꺽쇠의 첫번 째 인자로 N을 넘겨주었는데, 이 의미는

```
add()를 한 번 실행하는게 아닌, N 번 병렬 실행해달라는 뜻입니다.
```

꺽쇠 괄호 안의 인자를 변경해서 N번 병렬 실행하면, 

더 많은 수를 병렬적으로 더할 수 있습니다.

아래 코드를 보면서 이해를 해봅시다.

```c
__global__ void add(int *a, int *b, int *c) 
{
    c[blockIdx.x] = a[blockIdx.x] + b[blockIdx.x];
}
```
add<<<4,1>>>()로 함수를 호출하면

동시에 4번의 add함수가 병렬적으로 호출되는데, 각 함수 호출마다 block을 가지게 됩니다.

그리고 각 함수 내에서는 blockIdx.x를 사용하여 변수를 참조로 접근이 가능합니다.

Device에서는 병렬적으로 각 Block 로직을 수행합니다. 
```c

0번 Block - add()
{
     c[0] = a[0] + b[0];
}

1번 Block - add()
{
     c[1] = a[1] + b[1];
}

2번 Block - add()
{
     c[2] = a[2] + b[2];
}

3번 Block - add()
{
     c[3] = a[3] + b[3];
}
```

이제 main 코드를 조금 수정합시다.

```c
#define N 512
int main(void) 
{
    
    int *a, *b, *c; // Host에서 사용할 연속된 메모리 블록을 가르키는 주소입니다. 
    int *d_a, *d_b, *d_c; // GPU에서 사용할 연속된 메모리 블록을 가르키는 주소입니다.
    int size = N * sizeof(int); // 이제 sizeof(int) * N이 되었습니다!

    // device에 메모리를 할당합니다.
    cudaMalloc((void **)&d_a, size);
    cudaMalloc((void **)&d_b, size);
    cudaMalloc((void **)&d_c, size);

    // a,b,c에 메모리를 할당하고 입력값을 설정합니다.
    a = (int *)malloc(size);
    random_ints(a, N);
    b = (int *)malloc(size); 
    random_ints(b, N);
    c = (int *)malloc(size);
    
    // 입력 값을 gpu memory 변수에에 복사합니다.
    cudaMemcpy(d_a, a, size, cudaMemcpyHostToDevice);
    cudaMemcpy(d_b, b, size, cudaMemcpyHostToDevice);
    
    // add() 함수를 gpu에서 N개의 병렬 블록으로 실행합니다.
    add<<<N,1>>>(d_a, d_b, d_c);
    
    // 결과들을 CPU Memory에 복사해옵니다.
    cudaMemcpy(c, d_c, size, cudaMemcpyDeviceToHost);
    
    // 할당된 자원들을 해제합니다.
    free(a); free(b); free(c);
    cudaFree(d_a); cudaFree(d_b); cudaFree(d_c);
    return 0;
}
```

# 복습

지금까지 튜토리얼 1, 2에서 살펴본 내용을 정리하자면 다음과 같습니다.

- Device와 Host의 차이
    - Host CPU
    - Device GPU

- __global__ keyword를 함수 앞에 사용하면
    - Device에서 실행되는 코드이다.
    - Host에 의해서 호출되는 함수이다.

- Device Memory 관리
    - cudaMalloc()
    - cudaMemcpy()
    - cudaFree()

- 코드(커널) 병렬 실행
    - add()함수 N개를 병렬적으로 실행 방법 add<<<N,1>>>()
    - blockIdx.x를 사용하면 각 Block의 index를 알 수 있다.