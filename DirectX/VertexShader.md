# Shader의 정의

화면에 출력되는 픽셀들의 위치와 색상을 결정하는 함수다.


# 간략한 3D Pipeline

```
정점 데이터
```
▼
```
// GPU
▼
Vertex Shader (정점 위치 결정 
▼
Rasterizer (픽셀 위치 결정)
▼
PixcelShader (색상결정)  

```
▼
```
화면 
```     

# Vertext shader 기본 코드


Vertex shader 의 가장 중요한 일은 정점들의 위치를 계산하는 것이다.

```c
struct VS_INPUT
{
   float4 mPosition : POSITION;
};

struct VS_OUTPUT
{
   float4 mPosition : POSITION;
};

float4x4 gWorldMatrix;
float4x4 gViewMatrix;
float4x4 gProjectionMatrix;

VS_OUTPUT vs_main(VS_INPUT Input)
{
 VS_OUTPUT Output;
 Output.mPosition = mul(Input.mPosition, gWorldMatrix);
 Output.mPosition  = mul(Output.mPosition, gViewMatrix);
 Output.mPosition  = mul(Output.mPosition, gProjectionMatrix);
 
 return Output;
}
```

# 자료형

float4 : 실수 4개(x,y,z,w)를 가진 vector

float4x4 : 실수 4x4개를 가진 matrix

# Sementic

시멘틱, Tag정도로 이해하면 좋다.

DirectX의 Vertex Buffer의 특별한 정보들을 가져오거나 설정할수 있게한다.

```
float4 mPosition : POSITION(이부분이 시멘틱에 해당한다. 위치 정보를 담고있다는 뜻)
```

# Direct X에서의 좌표계

x,y,z 세개의 축을 가지고 있고 왼손 좌표계이다.

```
y   z
|  /  
| / 
|ㅡㅡㅡ x
````

# 3D 공간 변환

방안에 사과가 있는 3D 씬을 가정할 때 일어나는 공간 변환에 대해서 설명  

## Local Space, Object Space

사과의 표면 Vertex는 사과의 중심(원점) 으로부터 각각의 Vertext Position을 가진다.

사과를 방안의 어디로 움직이던, 사과의 Vertex 들은 원점으로 부터 거리가 변하지 않는다.

이것이 바로 object space 혹은 local space라고 한다.

## World Space

방안에 책상, 의자 등 다른 물체들도 존재할 것이다.

여러 물체들은 각자의 local space에서 정의되어 있지만,

상대적인 좌표를 주기 위해선 새로운 좌표계로 가져와야한다.

이것이 world space다.

## View Space

카메라를 통해 바라본 공간은 한정적일것이다.

카메라가 사과를 바라보지 않고, 반대편을 본다면 사과는 뷰 공간에서 찾아 볼 수 없다.

그러나 world space에서는 좌표가 변하지 않는다. 이를 통해 View Space와 World Space는 다른 공간이라는 것을 알 수 있다.

뷰공간의 원점은 정 중앙으로 z가 앞으로 들어가는 방향이다.

## Projection Space

카메라로 사진을 찍기위해서는 두단계가 필요하다.

1. 월드 공간 물체들을 뷰공간으로 이동, 회전, 확대/축소 시킨다.

2. 뷰공간의 오브젝트들을  2D이미지 위에 투영된다. 이때 시야각을 어떻게 잡을 것인가, 원근/직각 투시에따라 투영된 이미지가 다르게 보여진다.

1 의 경우 뷰 공간, 2 단계를 투영 공간이라고 구분할 수 있다.
 
 
## 정리
---

정점의 위치벡터에 행렬을 곱해서 정점의 공간을 변환할수 있다. 

```
Local Space   ->    World Space   ->    View Space   ->   Projection Space
         X World Matrix       X View Matrix      X Projection Matrix 
```

# 전역 변수와 지역 변수

전역 변수 예 : 모든 정점이 공유하는 데이터로 정의

지역 변수 예 : 각 정점마다 가지고있는 데이터 (Vertex buffer로부터 입력을 받음) 

