# DirectX11의 Math Library

DirectX 9, 10의 경우 d3dx 라이브러리를 링크해서 D3DXVECTOR, D3DXMATRIX와 같이 D3DX 객체를 편리하게 사용할 수 있었다.

DirectX11 부터는 DirectXMath(XNAMath) 라이브러리를 사용해서 프로그래밍을 할 수 있다.

이를 사용하는 방법은 간단하다. 그냥 DirectXMath.h 헤더를 include 하면 끝이다.


# XNA Math Library의 특징

XNA Math 라이브러리는 Xbox와 Windows에서 사용할 수 있는 특별한 하드웨어 Register를 사용한다.

SIMD(Single Instruction Multiple Data)를 활용해서 여러개의 데이터를 하나의 명령어로 처리 할 수 있다.

다음과 같이 벡터 U와 V를 더하는 예를 살펴보자.

```
 u + v = (Ux + Vx, Uy + Vy, Uz + Vz);
```

SIMD를 이용하면 각각 x, y, z의 성분을 더하는 4개의 연산이 아닌,  하나의 SIMD 명령어로 처리가 가능하다.

## Intrinsics

컴파일러에서 SIMD 명령어를 위해 내장 함수들을 구현해두었다. 이를 Intrinsics 함수라고 부르며

전처리기를 통해서 현재 하드웨어에서 컴파일이 가능한 내장 함수 코드를 선택하고 컴파일한다.

DirectX Math library는 intrinics의 효과를 살리기위해 몇가지 특징들을 설명해준다.

### 1. Return Type을 Value이다.

컴파일러가 최적화를하기 쉽게 해준다. return by reference라면 불필요한 레지스터와 연산을 더 만들게 된다.

### 2. data 선언은 purely 하다.

Data Type들은 연속된 메모리에 불과하다.
아래의 예제를 보면 XMVector도 실제로는 128바이트 메모리에 불과하다.

### 3. inline으로 함수로 선언한다.

inline으로 선언하며 함수 call 비용을 아낀다.


## 실제 구현 코드

다음은 실제 XMMath 구현 소스는 중 일부를 발췌해 온 것으로 위에서 설명한 것들이 잘표현되어있다.

```c
#if defined(_XM_SSE_INTRINSICS_) && !defined(_XM_NO_INTRINSICS_)
typedef __m128 XMVECTOR; // 특징 1. Data 는 단순한 128 byte memory이다.

// 2. inline으로 함수로 구현되어 있다.
// 3. return type은 value이다.
inline XMVECTOR XM_CALLCONV XMVectorAdd
(
    FXMVECTOR V1, 
    FXMVECTOR V2
)
{
#if defined(_XM_NO_INTRINSICS_)
// CPU 내장함수를 사용하지않으면 4번의 add operation이 이루어진다.
    XMVECTORF32 Result = { { {
            V1.vector4_f32[0] + V2.vector4_f32[0],
            V1.vector4_f32[1] + V2.vector4_f32[1],
            V1.vector4_f32[2] + V2.vector4_f32[2],
            V1.vector4_f32[3] + V2.vector4_f32[3]
        } } };
    return Result.v;

// 사용 가능 intrinsics 선택
#elif defined(_XM_ARM_NEON_INTRINSICS_)
    return vaddq_f32( V1, V2 );
#elif defined(_XM_SSE_INTRINSICS_)
    return _mm_add_ps( V1, V2 );
#endif
}
```

# 기타 참고 링크

[intel Intrinsics Guide](https://software.intel.com/sites/landingpage/IntrinsicsGuide/#text=_mm_add_ps)

[SIMD Wiki](https://en.wikipedia.org/wiki/SIMD)

[Designing Fast Cross-Platform SIMD Vector Libraries](https://www.gamasutra.com/view/feature/4248/designing_fast_crossplatform_simd_.php)
