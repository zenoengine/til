GDC 2019 에서 Intel에서 발표한 병렬 프로그래밍 주제를 듣고 정리 해두려합니다. :D

# 모던 하드웨어의 고성능을 위한 병렬화의 종류 

최신 하드웨어에서 고성능을 위한 병렬화의 종류에는 두가지가 있습니다.

1. Multi Threading, Multi-Core Programming등 Task

2. **Single Instruction Multi Data (SIMD)**

여기서는 두번째 SIMD와 관련된 기술인 ISPC를 소개하려합니다.

# ISPC

ISPC는 Intel [SPMD](https://en.wikipedia.org/wiki/SPMD) Program Compiler의 줄임말입니다.

이 컴파일러를 사용하면 닌자프로그래머 없이 컴퓨터의 성능을 FLOPS 올릴 수 있으며, 다양한 ISA에 맞는 고성능의 벡터 코드를 만들 수 있습니다.

ISPC는 자동 벡터라이징을 해주는 컴파일러가 아닙니다. 따라서 스칼라 코드를 보고 자동으로 분석하거나 변환하여 벡터 코드를 생성하지 않습니다. 프로그래머가 컴파일러에게 뭐가 벡터코드고 뭐가 스칼라 코드인지 명시적으로 지정해주어야합니다.

# 언어의 형태

```c
export void simple(uniform float vin[], uniform float vout[], uniform int count) {
    foreach (index = 0 ... count) {
        float v = vin[index];
        if (v < 3.)
            v = v * v;
        else
            v = sqrt(v);
        vout[index] = v;
    }
}
```

C 코드와 많이 비슷해 보입니다. 따라서 읽기도 이해하기도 쉽습니다.
순차적으로 코드로 보이지만, 병렬적으로 실행됩니다. 쉽게 스칼라 코드와 벡터 연산 코드를 같이 작성할수있습니다.

# Uniform vs Varing

처음 ISPC를 입문할때 두가지의 새로운 키워드는 Uniform과 Varing입니다.

## Uniform
- 스칼라 데이터

## Varing
- 벡터 데이터이며, 결과 값이 SIMD vector register(XMM,YMM,ZMM,etc)에 있습니다.
- Varying 이 기본 타입입니다.
- 각 SIMD 연산은 고유의 값을 얻습니다.


## ISPC 왜 좋을까요?
프로그래머가 좋은 벡터 코드를 작성하기위해 명령 셋(ISA)에 대해 깊게 알지 않아도 되며, SIMD Intrinsics 친숙하지 않은 프로그래머도 다룰 수 있습니다.

더 많은 프로그래머들이 다양한 최적화를 위해 시도해볼수 있다는 뜻이기도 합니다.

또한, 읽기에도 쉬워 유지 보수에도 좋으며, *새로운 ISA*가 나와도 컴파일러가 지원을 해준다면, 커맨드 라인을 통해 재컴파일해 최신 ISA셋을 지원할수 있다는 점입니다. 

Intrinsics를 작성하지 않아도, 4-wide SSE를 사용하면 3배, 8-wide AVX2를 사용하면 5-6배의 성능 향상이 되는것이 일반적입니다.  

# 어디에 써야 할까요?
- Physcis (eg. [UE4 Chaos Demo](https://www.youtube.com/watch?v=u3ktiewcLpo))
- Particle System
- Skning
- Asset pipeline(huge benefit)
- compression/decompression
- raycasting
- intersection testing
- image convolution
- post processing
- software culling

# Reference 
https://software.intel.com/en-us/videos/simple-single-instruction-multiple-data-simd

https://github.com/ispc/ispc

In file included from /usr/include/c++/7/cassert:44:0,
                 from /home/zeno/project/ispc/llvm_build/llvm-8.0/llvm/include/llvm/ADT/StringSet.h:20,
                 from /home/zeno/project/ispc/llvm_build/llvm-8.0/clang-tools-extra/clangd/index/Merge.cpp:14:
/home/zeno/project/ispc/llvm_build/llvm-8.0/clang-tools-extra/clangd/index/Merge.cpp: In member function ‘virtual void clang::clangd::MergedIndex::refs(const clang::clangd::RefsRequest&, llvm::function_ref<void(const clang::clangd::Ref&)>) const’:
/home/zeno/project/ispc/llvm_build/llvm-8.0/clang-tools-extra/clangd/index/Merge.cpp:106:20: warning: comparison of unsigned expression >= 0 is always true [-Wtype-limits]
   assert(Remaining >= 0);
          ~~~~~~~~~~^~~~
[ 86%] Building CXX object tools/clang/tools/extra/clangd/CMakeFiles/clangDaemon.dir/index/SymbolID.cpp