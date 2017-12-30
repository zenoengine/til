# 기본 지식

 ## Direct3D의 개요
 Direct3D는 응용 프로그램이 3차원 가속 기능을 이용하여 3차원 세계를 렌더링 할수 있게 하는 API 이다. 
 Direct3D는 그래픽 하드웨어를 제어 할 수 있기 때문에, 화면을 깨끗히 지우라는 명령을 하드웨어에게 ID3D11DeviceContect::ClearRenderTargetView 함수를 통해서 전달 할 수 있다.
 기존 널리 쓰이는 버전을 DirectX 9을 사용하였는데, 9의 경우 특화 기능을 사용하려면, 현재 하드웨어(그래픽 카드)가 해당 기능을 지원하는지 일일히 체크해야했다면, 11은 그럴 필요가 없다.
 그래픽 장치가 DX11을 지원한다고 얘기하려면 DX11 의 모든 기능을 지원해야 한다는 엄격한 조건이 붙기 때문이다.

## COM
Component Object Model은 Direct X의 하위 호환성과 언어 독립성을 가능하게 하는 기술이다.
쉽게 표현하자면, IUnkown 인터페이스를 상속 받게 하는 패턴이라고 이해하면 좋을 듯하다.
IUnkown Interface를 상속받은 객체들은 new로 할당하지 않으며, 메모리를 해제할 때도 delete를 사용하지 않고, Release라는 메소드를 호출해서 자원을 해제한다.
COMInterface 를 갖고 있는 객체들은 이름이 대문자 I로 시작한다.

## 텍스쳐 및 자료 자원 형식
2차원 텍스쳐는 사실상 2차원 배열(혹은 행렬)이다. 
2차원 텍스쳐의 한가지 용도는, 이미지 자료를 저장하는 것으로, 그런 경우 텍스쳐의 각 원소(element)는 각 픽셀의 색상을 담는다.
그러나 이미지 저장만이 텍스쳐의 유일한 용도는 아니다. 법선 매핑이라는 기법에서는 각 원소가 3차원 벡터를 의미한다.
따라서, 텍스쳐라 하면 흔히 이미지를 생각하기 쉽지만, 그보다는 훨씬 범용적인 곳에서 사용된다.
그리고 사실, 단순히 2차원 배열 뿐만은 아니다. 텍스쳐에는 밉맵 수준들이 존재 할 수 있으며, GPU필터링, 다중표본화 등 연산을 텍스쳐에 적용 할 수 있다.

텍스쳐에는 특정 포맷을 따르는 자료만 저장 할 수 있다는 점이 중요하다.
다음은 텍스쳐에 담을 수 있는 자료 형식의 예이다.
```c
DXGI_FORMAT_R32G32B32_FLOAT // RED, GREEN, BLUE 32bit float로 구성
DXGI_FORMAT_R16G16B16A16_UNORM // RED, GREEN, BLUE, ALPHA 16bit로 0~1사이 정규화(normalized) 된 값으로 구성
DXGI_FORMAT_R32G32_UINT // red, green 32bit 정수로 구성
DXGI_FORMAT_R8G8B8A8_UNORM // red,green,blue,alpha 8 bit로 0~1값을 가짐
DXGI_FORMAT_R8G8B8A8_SNORM // red,green,blue,alpha 8 bit로 -1~1 값을 가짐
DXGI_FORMAT_R8G8B8A8_SINT // red,green,blue,alpha 8 bit로 -128~127 값을 가짐
DXGI_FORMAT_R8G8B8A8_UINT // red,green,blue,alpha 8 bit로 0~255 값을 가짐
```

앞서 얘기한듯 해당 포맷에는 반드시 색상 정보만을 담을 필요는 없다.
r,g,b 값에 x,y,z 값을 저장해둘 수 도 있다는 뜻이다.

또한 자료 형식에는 TYPELESS도 있는데, 메모리만 확보해두고 해석 방식은 텍스쳐를 파이프라인에 묶을 때 정할 수 있다. (C++의 reinterpret_cast와 비슷하다.)

## 교환 사슬과 페이지 전환
화면에 그려질 때 깜빡임(flicking)이나 찢어짐(tearing) 현상을 막기 위한 트릭으로 Texture 두장을 사용한다.
이때 두개의 텍스쳐는 Swap chain(교환 사슬)을 형성한다.
두개의 텍스쳐(버퍼)를 번갈아 가면서 보여 주는 것을 더블 버퍼링이라고 한다.

## 깊이 버퍼
깊이 버퍼(Depth Buffer)는 이미지 자료를 담지 않는 텍스쳐의 한 가지 예로 들 수 있다.
깊이 버퍼의 한 요소는 픽셀의 깊이 정보를 담는다.
여러가지 물체가 있을 경우, 해당 물체의 깊이 정보를 살핀 후, 현재 있는 깊이 버퍼 정보와 비교해 더 작은 값이면 갱신한다.

깊이 버퍼도 하나의 텍스쳐 이므로 특정한 자료 형식을 지정해서 생성해야한다. 깊이 버퍼링을 위한 텍스쳐 형식으로는 다음과 같은 것들이 있다.

```c
DXGI_FORMAT_D32_FLOAT_S8X24_UINT //Depth 정보 32bit float, 스탠실 값을 위한 8bit unisgined 정수 그리고 다른 용도 없이 채우기로만 쓰이는 pading 24bit로 구성
DXGI_FORMAT_D32_FLOAT //Depth 정보 32bit float
DXGI_FORMAT_D24_UNORM_S8_UINT //Depth 정보 24bit 정규화된 값 0~1, 스탠실 값을 위한 8bit unisgined 정수
DXGI_FORMAT_D16_UNORM //Depth 정보 16bit 정규화된 값 0~1
```

###참고
응용프로그램이 스텐실 버퍼를 반드시 사용해야 하는 것은 아니나, 만일 사용한다면 **반드시 스텐실 버퍼를 깊이버퍼에 부착해야한다.**
DXGI_FORMAT_D24_UNORM_S8_UINT 포맷의 경우 각 요소당 깊이 정보를 24bit 만큼 담고, 스텐실 정보를 8bit만큼 담는다. 이 때문에 깊이*스텐실 버퍼라는 용어가 더 정확하다.

## 텍스쳐 자원 뷰
렌더링 파이프라인에는 텍스쳐를 몪을(Bind)수 있는 단계들이 여럿 있다. 흔한 용도는 텍스쳐를 렌더대상을 묶는 것과 셰이더 자원으로 묶는 경우이다.
이 두가지 용도로 사용할 텍스쳐를 생성 할 때에는 다음과 같이 텍스쳐를 묶을 두 파이프 라인 단계를 지정한 결속 플래그(Bind flag)를 사용한다.
 
 ```
 D3D11_BIND_RENDER_TARGET | D3D11_BIND_SHADER_RESOURCE
 ```

사실 텍스쳐를 직접 바인딩 하는 것은 아니다.

어떤 용도로든 텍스쳐 자원을 사용하려면 자원 뷰(Resource View)라는 것을 생성해야 하고, 이 자원 뷰가 바인딩 된다.
자원 뷰를 생성하는 시점에 텍스쳐 생성 포맷 플래그를 검사하고, 결속 시에 형식 검사가 최소화 된다.
따라서 렌더 대상과 셰이더 자원으로 사용하는 경우 RenderTargetView를 생성하고 ShaderResourceView를 생성해야한다.

자원 뷰들은 본질적으로 두가지 일을 한다.
- Direct3D에게 자우너의 사용 방식을 알려주는 것
- 생성 시점에서 무형식을 지정한 자원 형식의 구체적인 형식을 결정하는 것 (TYPELESS 자원을 어떨때는 float로, 혹은 int format으로 사용 할 수 있음)

자원 뷰를 생성 하려면 Flag 값을 지정해야 하고, 이 Flag 값에 따라 특정 자원 뷰를 생성 가능/불가능 해진다.

예를 들어
D3D11_BIND_DEPTH_STENCIL 플래그를 지정하지 않은 자원이라면, DepthStencilView를 생성하지 못한다.

### 참고
형식이 완전한 지정된 자원은 생성시 지정된 형식으로만 사용 할 수 있다. 그러면 런타임시 자원 접근을 최적화 할 수 있다. 즉, 무형식 자원은 꼭 필요한 경우에만 사용하자.

## 다중 표본화 이론
대각선을 표현하는 픽셀들이 계단 현상을 보이는 앨리어싱(aliasing)을 막아주는 기법들이 존재한다.

### 초과 표본화(Super Sampling)
하나는 초과 표본화로 그려야하는 화면보다 4배(가로,세로 두배) 크게 잡고 렌더링 한후 원래 화면 크기 로 환원한다(resolving). 
이때 resolving하는 과정은 4픽셀 블록의 평균 색상을 해당 픽셀의 최종 색상으로 사용한다.
하양 표준화(downsampling)이라고도 불린다.
픽셀 처리량과 메모리 소비량이 4배라서 비용이 크다. 따라서 Direct3D는 다음에 나올 다중 표본화 라는 절충적인 앨리어싱 제거 기법을 지원한다.

### 다중 표본화(Multi Sampling)
다중 표본화는 일부 계산 결과를 픽셀들 사이에서 공유하기 때문에 초과 표본화보다 비용이 적다.
Vertex Triangle이 화면에 그려지고, 가상의 그리드가 있다고 할 때, 그리드의 Center가 삼각형의 안쪽에 있는 부분 픽셀들만 계산을 하여 Super Sampling의 과도한 연산을 줄일 수 있다.

### 참고
Super Sampling 에서는 색상이 부분 픽셀별로 계산되므로, 한 픽셀의 부분 픽셀들의 색이 각자 다를 수 있다.
반면 다중 표본화는 다각형이 덮은 부분 픽셀에 모두 같은 색상이 적용된다. 이미지 색상 계산은 그래픽 파이프라인에서 가장 비싼 단계 중 하나 이기 때문에 비용 Multi sampling의 비용 절감 효과는 상당하다.
그러나 Super Sampling은 기술적으로 더 정확하고 텍스쳐와 셰이더 앨리어싱을 처리하는 반면, 다중 표본화는 할 수 없다.

## Direct3D의 MultiSampling
멀티 샘플링을 사용하기 위해서는 다음 구조체를 사용해야한다.
```c
typedef struct DXGI_SAMPLE_DESC {
    UINT count; //픽셀당 추출할 표본의 갯수
    UINT Qaulity; // 품질 수준, 높을 수록 렌더링 비용 증가.
} DXGI_SAMPLE_DESC, *LPDXGI_SAMPLE_DESC;
```

표본 개수와 품질 수준의 개수를 알고 싶으면 다음 메서드를 사용한다.
```c
HRESULT CheckMultisampleQuailityLevels(DXGI_FORMAT Format, UINT SampleCount, UINT *pNumQualityLevels);
```

하나의 텍스쳐 형식,표본 개수 조합에 대한 유요한 품질 수준의 범위 : 0~pNumQualityLevels - 1

한 픽셀당 추출할 수 있는 최대 표본의 개수 Define은 다음과 같다.
```c
#define D3D11_MAX_MULTISAMPLE_SAMPLE_COUNT (32)
```

대부분은 합리적인 수준을 위해 4개나 8개만 표본을 추출하는 경우가 많고, Direct3D 11 대응 장치는 모두 4X 다중표본화를 지원한다.
사용하고 싶지않다면 표본 개수를 1로 품질을 0으로 설정하면 된다.

### 참고
DXGI_SAMPLE_DESC 구조체는 후면버퍼, 깊이버퍼에 모두 채워야하며 설정은 동일해야한다.

## 기능 수준
Direct3D 11은 Feature Level이라는 개념을 도입했다.
응용 프로그램이 Direct3D Version을 어느 수준까지 지원하는지 파악하고 순서대로 점검할 수 있다.
관련 enum은 D3D_FEATURE_LEVEL을 찾아보면 된다.

Direct3D 초기화 함수에서 D3D_FEATURE_LEVEL 배열을 넘겨주면 Direct3D는 해당 응용프로그램이 기능을 지원하는지 순차적으로 점검하고 가장 높은 버전의 기능을 사용하면 좋을 것이다.

