# Direct3D 초기화

4X Multi Sampling을 지원하는 응용프로그램의 초기화 과정 예

```
1. D3D11CreateDevice 함수를 호출해서 Device와 DeviceContext를 생성
2. CheckMultiSamplingLevel을 호출해 지원하는 MSAA(MultiSampling Anti Aliasing)의 품질 수준 지원 정보 획득
3. SwapChain 관계를 정의하는 DXGI_SWAP_CHAIN_DESC 구조체를 채움
4. Device를 생성했던 IDXGIFactory Interface를 Query해서 SwapChain Instance를 생성
5. Back Buffer의 RenderTarget View를 생성
6. Depth,Stencil Buffer 생성
7. Render Target View와 Depth,Stencil View를 파이프라인에 바인딩
8. ViewPort 설정
```

## 1. Device 생성, Device Context 생성

```c
HRESULT D3D11CreateDevice(
    IDXGIAdapter * pAdapter, // 디스플레이 어댑터 번호
    D3D_DRIVER_TYPE DriverType, // 하드웨어 가속, 소프트웨어 구동기, reference 장치 생성
    HMODULE Softwrae, // 소프트웨어 구동기 핸들, DriverType 가 소프트웨어 구동기가 설정되어 있을 경우 사용, 하드웨어 사용시 NULL
    UINT Flags, // 추가적인 생성플래그 지정, DEBUG 계증사용이나 SINGLE THREAD 모드 
    CONST D3D_FEATURE_LEVEL *pFeatureLevels, //이전에 이야기한 하드웨어가 지원하는 체크하고싶은 DX 버전 배열
    UINT FeatureLevels, // pFeatureLevels 배열의 개수 
    UINT SdkVersion, // 사용하고있는 SDK VERSION
    ID3D11Device **ppDevice, //생성된 Device 반환 
    D3D_FEATURE_LEVEL pFeatureLevel, // FeatureLevels에서 처음으로 지원하는 Feature
    ID3D11DeviceContext **ppImmediateContext //생성된 Device Context 반환
);
```

## 2. 4XMSAA 품질 수준 지원 정보 획득

```c
UINT m4xMassQuality;
md3dDevice->CheckMultiSamplingQualityLevels) D3XGI_FORMAT_R8G8B8A8_UNORM, 4, &m4MassQuality));
assert(m4xMassQuality > 0);
```

## 3. SwapChain 정의 및 관계 생성

SwapChain의 특성을 정의하기 위한 구조체
```c
typedef struct DXGI_SWAP_CHAIN_DESC { 
    DXGI_MODE_DESC BufferDesc; // Back Buffer의 높이,너비, 텍셀 포맷 정의
    DXGI_SAMPLE_DESC SampleDesc; //Sampling 관련 정보 설종
    DXGI_USAGE BufferUsage; //Back Buffer의 사용법, Render_Target_Ouput, 
    UINT BufferCount; //Back buffer의 개수, 일반적으론 한개(더블 버퍼링) 
    HWND OuputWindow; //렌더링 결과를 출력할 Window Program의 Handle
    BOOL Windowed; //창모드 여부
    DXGI_SWAP_EFFECT SwapEffect; //일반적으론 DISCARD
    UINT Flags; //추가 플래그, DXGI_SWAP_CHAIN_FLAG_ALLOW_MODE_SWITCH
} DXGI_SWAP_CHAIN_DESC;
```

### 참고
일반적으로 후면 버퍼에 대해 DXGI_FORMAT_R8G8B8A8_unorm 형식을 사용하는 데, 일반적인 모니터 출력이 색상당 비트 수를 24bit이하이므로 정밀도를 그보다 높이 잡아봤자 낭비이기 때문이다. 여분의 8비트는 모니터가 출력하는 것이 아니다. 다만 이 여분의 비트를 가지고 몇 가지 특수효과에 사용 할 수 있다.

## 4. SwapChain 생성

SwapChain을 생성하기위해서는 IDXGIFactory에 있는 Interface를 통해서 생성해야하는데, Device를 생성한 Interface를 통해서 생성해야 한다.

```c
IDXGIDevice* dxgiDevice = 0;
m_pd3dDevice->QueryInterface(__uuidof(IDXGIDevice), (void**)&dxgiDevice);

IDXGIAdapter* dxgiAdapter = 0;
dxgiDevice->GetParent(__uuidof(IDXGIADapter), (void**)&dxgiFactory));

//드디어 Device를 생성한 IDXGIFactory 획득
IDXGIFactory dxgiFactory = 0;
dxgiAdapter->GetParent(__uuidof(IDXGIFactory), (void**)&dxgiFactory );

//드디어 SwapChain 생성
IDXGISwapChain* mSwapChain = 0;
dxgiFactory->CreateSwapChain(md3dDevice, &sd, &,mSwapChain);

// 획득한 COM Interface를 이제 다 사용했으므로 해제
ReleaseCOM(dxgiDevice);
ReleaseCOM(dxgiAdapter);
ReleaseCOM(dxgiFactory);
```

## 5. RednerTarget View 생성

```c
ID3D11RenderTargetView* mRederTargetView;
ID3D11Texture2D* backBuffer;
mSwapChain->GetBuffer(0, __uuidof(ID3D11Texture2D), reinterpret_cast<void**>(backbuffer)); //0번 back Buffer를 가져온다.
md3dDevice->CreateRenderTargetView(backBuffer, 0, &mRenderTargetView); //backbuffer Resource의 RenderTargetView 생성 
ReleaseCOM(backbuffer); //후면버퍼를 다 사용했기 때문에 Release 호출
```

## 6. Depth, Stencil View 생성
Depth Buffer도 2차원 Texture이다.
2차원 Texture를 생성하기 위해 Description 구조체는 다음과 같다.
```c
typedef struct D3D11_TEXTURE2D_DESC{
    UINT Width; // 텍스쳐의 너비(텍셀)
    UINT Height; // 텍스쳐의 높이(텍셀)
    UINT MipLevels; // 뎁스 버퍼는 밉맵수준이 하나만 있으면 된다.
    UINT ArrahySize; // 텍스쳐 배열의 개수 뎁스 버퍼는  1개면 된다.
    DXGI_FORMAT Format; // 텍셀의 포맷, DXGI_FORMAT_D24_S8 과 같은 형식
    DXGI_SAMPLE_DESC SampleDesc; // Multi Sampling 정보와 같아야 함.
    D3D11_USAGE Usage; // 텍스쳐의 용도에 따른 필드, CPU(Dynamic, Staging), GPU(DEFAULT, IMMUTABLE)
    UINT BindFlags; // 자원이 파이프라인에 어떤식으로 묶일지 정의 Shader_Resource, Redner_Target
    UINT CPUAccessFlags; // Usage가 DYANMIC or STAGING
    UINT MiscFlags; // 기타 플래그
} D3D11_TEXTURE2D_DESC;
```

Usage에서 DYNAMIC이나 STAGING의 경우 CPU <-> GPU 메모리간의 전송이 있어야하므로 성능상 저하가 심하므로 꼭 필요한 경우에만 사용한다.

```c
ID3D11Texture2D mDepthStencilBuffer;
ID3D11DepthStencilView mDepthStencilView;
md3dDevice->CreateTexture2D(
    &depthStencilDesc, // 생성할 텍스쳐를 서술하는 구조체
    0,
    &mDepthStencilBuffer // 깊이, 스텐실 버퍼를 가리키는 포인터를 돌려준다.
    );
mpd3dDevice->CreateDepthStencilView(
    mDepthStencilBuffer, //생성한 텍스쳐 포인터
    0, // DepthStenCilView Descrption 포인터, 뎁스 스텐실 버퍼를 생성할 때 형식을 지정했으면 NULL값으로 지정해 줘도 된다. 
    &mDepthStencilView // 생성한 DepthStencilView 반환
    );
```

## 7. ViewPort 설정

툴에서 사용할 때 처럼 Window 내에 일부 영역에만 렌더링을 출력하거나,
화면 분할 2인용 플레이 기능을 구현할 때 사용 가능하다.

```c
typedef struct D3D11_VIEW_PORT {
    FLOAT TopLeftX; // 위치 X값 
    FLOAT TopLeftY; // 위치 Y값
    FLOAT Width; // 뷰포트의 너비
    FLOAT Height; // 뷰포트의 높이
    FLOAT MinDepth; // 특수 효과를 원하지 않으면 0 
    FLOAT MaxDepth; // 특수 효과를 원하지 않으면 1
} D3D11_VIEWPORT;

md3dImmediateContext->RSSetViewport(1, &vp);
```