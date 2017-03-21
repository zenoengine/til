# Actor

Actor, 액터란 레벨에 배치할 수 있는 오브젝트를 말합니다. 액터는 이동, 회전, 스케일과 같은 3D 트랜스폼을 지원하는 범용 클래스입니다. 

참고로 액터는 트랜스폼 (위치, 회전, 스케일) 데이터를 직접 저장하지 않으며, 액터의 루트 컴포넌트에 트랜스폼 데이터가 존재하는 경우, 그 데이터를 사용합니다.

개인적으로는 유니티가 Gameobject에 무조건 Transform을 가지고 있는 것과 비교하면 메모리 절약적라고 생각합니다.

Folder 같은 개념으로 사용하는 Empty GameObject에 Transform이 붙어있을 필요는 없기 때문입니다.

# Actor Spawn

Actor를 생성하는 방법은 전역 클래스인 UWorld의 SpawnActor를 통해 할 수 있습니다.
보다 자세한 내용은 [액터 스폰하기](https://docs.unrealengine.com/latest/KOR/Programming/UnrealArchitecture/Actors/Spawning/index.html)

```c
UWorld::SpawnActor() 
```

# 컴포넌트

액터 는 한 편으로 보면, Component (컴포넌트) 라 불리는 특수 유형 Object (오브젝트)를 담는 그릇으로 생각해 볼 수 있습니다. 여러가지 유형의 컴포넌트를 사용하여 액터 의 이동 및 렌더링 방식 등을 제어할 수 있습니다

컴포넌트는 생성시 자신을 포함하고 있는 액터에 할당됩니다.

컴포넌트 핵심 유형 몇 가지입니다:

### UActorComponent 액터 컴포넌트
베이스 컴포넌트입니다. 액터의 일부로 포함 가능합니다. 원한다면 Tick 시킬 수 있습니다. 액터 컴포넌트는 특정 액터에 연관지어지나, 월드의 특정 지점에 존재하지는 않습니다. 일반적으로 개념적 기능, 이를테면 AI 나 플레이어 입력 해석과 같은 것에 사용됩니다.

### USceneComponent 씬 컴포넌트
씬 컴포넌트는 트랜스폼이 있는 액터 컴포넌트입니다. 트랜스폼은 위치, 회전, 스케일로 정의되는 월드상의 포지션을 나타냅니다. 씬 컴포넌트는 계층구조 형태로 서로에게 붙일 수 있습니다. 액터의 위치, 회전, 스케일은 계층구조의 루트에 위치한 씬 컴포넌트에서 
취할 수 있습니다.

### UPrimitiveComponent 프리미티브 컴포넌트
일정한 형태의 (메시 또는 파티클 시스템과 같은) 그래픽적 표현이 있는 씬 컴포넌트를 말합니다. 여기에는 재미난 피직스 및 콜리전 세팅이 들어있습니다.

# 생명 주기

[생명 주기 문서](https://docs.unrealengine.com/latest/KOR/Programming/UnrealArchitecture/Actors/ActorLifecycle/index.html)

# 액터 소멸

액터는 일반적으로 가비지 콜렉팅되지 않는데, 월드 오브젝트가 액터 레퍼런스 목록을 저장하기 때문입니다. 

액터는 Destroy() 를 호출하여 명시적으로 소멸시킬 수 있습니다. 그러면 레벨에서 제거되어 pending kill (킬 대기) 상태로 마킹, 잠시 후 다음 가비지 콜렉션 때 지워진다는 뜻입니다.