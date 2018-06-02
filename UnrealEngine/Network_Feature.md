# Unreal Engine 4 Network Features 

언리얼 엔진에서 네트워크 기능 예제를 제공하고 있습니다.

Contents Example 프로젝트를 받아서 Map 하위 폴더에 NetworkFeature를 열면 살펴 볼 수 있습니다.

오늘은 언리얼 엔진 네트워크 기능에 대해 간단하게 핵심만 정리하려합니다.



# 액터란?
우선, UE4에서 액터란 게임 씬에 배치할수 있는 모든 오브젝트를 말합니다.

(유니티를 아시는 분들이라면 게임오브젝트와 비슷한 개념으로 보시면 좋겠습니다.)

액터에 게임 매니져 컴포넌트를 붙여서 게임의 시작/종료 조건을 체크하는 매니저로 만들수도 있고,

액터에 이동/조작 관련 컴포넌트를 붙여서 플레이어로 만들수도 있습니다.

UE4에서는 액터를 상속받아서 특정한 속성을 가진 개념들을 구현하고 있는데요,

예를들면 StaticMesh(움직이지 않는 형태를 가진 오브젝트), Pawn(이동하는 캐릭터), Particle(이펙트) 등이 있습니다.


여기까지 액터에 대해서 이해를 하셨다면, 게임에 대한 규칙을 가지고 있는 액터(게임 매니져)를 만든 후에 

게임 매니져 "액터" 단위에서 **리플리케이션** 이루어지면 된다는 것을 알 수 있습니다.


# 리플리케이션이란?

리플리케이션이란 호스트 컴퓨터가 가진 데이터를 다른 컴퓨터로 복사하는 것으로, 흔히 이야기하는 캐릭터간의 이동 동기화, HP 동기화로 생각하면 될것입니다.

UE4에서 제공하는 리플레케이션 시스템중 액터에서 이루어지는 리플리케이션은 크게 2가지로 이루어지고 있습니다.

## 1. 변수 리플레케이션

### *Replicated*

변수 리플리케이션은 액터가 가지고있는 변수에게 Replication 항목의 옵션을 Replicated 로 켜주면 동작하게 됩니다.

![variable replication](http://api.unrealengine.com/images/Resources/ContentExamples/Networking/1_3/1_3_Rep_Variables.jpg)

이렇게 옵션이 켜진 변수(프로퍼티)가 서버에서 값이 변하면 통지가 되고, 클라이언트에서 동기화 처리합니다.

### *RepNotify*

그런데 어떤 변수가 변했을 때 특정한 일련의 행동을 하고 싶을때가 있겠죠?(HP가 깎이면서 피가 튄다! 라던지) 

이때 사용하는 옵션이 RepNotify 입니다.

![repnotify](http://api.unrealengine.com/images/Resources/ContentExamples/Networking/1_4/1_4_RepNotify.jpg)

이 옵션으로 세팅 해 줄 경우 해당 변수가 수정되었을때 호출되는 Call back 함수 하나가 추가되는데요, (예.  On HP Notify() )

이 함수를 통해 값이 바뀌었을 때 특정 동작을 구현 할 수 있습니다.

예제에서는 신호등의 색상이 (빨간색, 초록색, 노란색) 으로 바뀔 때 Mesh의 메테리얼 색상을 바꾸는 함수를 호출해서 시간에 따라 변하는 신호등을 구현해 두었습니다.

![street light color](http://api.unrealengine.com/images/Resources/ContentExamples/Networking/1_4/1_4.jpg)

## 2. RPC (Remote Proceuder Call)

변수 뿐만 아니라, 함수 호출 또한 리플리케이션이 가능합니다.

캐릭터가 스킬을 쓰거나, 아이템을 먹었을때 다른 유저에게도 같은 스킬 이펙트나 아이템 획득 이펙트를 터뜨리고 싶은 경우가 있을 텐데요. 

이 때, Remote(원격) Proceuder(함수) 호출을 통해서 특정 위치에 이펙트를 Spawn해달라고 호출 할 수 있는것입니다.

옵션은 2가지가 있는데 특징에 따라 호출 여부를 결정할 수 있으므로 최적화에 좋은 영향을 미치겠죠?

|option|특징|
|---|---|
|Reliable (신뢰할수 있는)| 반드시 호출이 보장된다.|
|Unreliable (신뢰 할 수 없는)| 네트워크 트래픽 상황에 따라 호출이 되지 않을 수 있다.|

블루프린트에서 New Custom Event를 생성하면 해당 Event 노드에서 리플리케이트 옵션을 지정 할 수 있는데 , 멀티 캐스트로 지정을 하면 서버에서 다른 클라이언트로 리플리케이션 하는 것으로 설정할수 있습니다.

언리얼 예제의 시나리오를 설명하자면 다음과 같습니다.

```
상황 : 바닥에 발판(버튼)이  있고 플레이어가 올라서있느냐 아니냐에 따라 버튼의 색상과 위치가 바뀐다.
```

여기서 버튼에서 멀어졌을 때의 기준만을 보고 만드는 과정은 다음과 같습니다.


1. 우클릭으로 New Custom Event로 Event(콜백)노드를 만들고, 리플리 케이트 옵션을 '멀티캐스트'로 지정한다

2. Event가 통지되었을 때 행동할 일련의 동작(시퀀스)들을 정의한다.

![custom event button released](http://api.unrealengine.com/images/Resources/ContentExamples/Networking/1_5/1_5_EventGraph3.png)

3. 이벤트를 호출하는 코드를 작성합니다. 이 경우는 트리거를 만들어서 구현되었습니다. 이제 Call Button Released Event를 호출하면, 2에서 작성한 서버에서도 이벤트 호출 이후의 시퀀스가 이루어지며, 클라이언트도 Event를 통지받아 같은 일을 할 것 입니다.

![](http://api.unrealengine.com/images/Resources/ContentExamples/Networking/1_5/1_5_EventGraph1.png)


여기까지 언리얼 엔진 네트워크 예제를 분석하고 핵심만 살펴 봤습니다.

중요도에 따라 먼캐릭터에게는 통지를 하지 않거나 서버와 클라이언트의 동작을 다르게 처리하는 등 

더 자세한 응용 예는 [언리얼 공식 문서](http://api.unrealengine.com/KOR/Resources/ContentExamples/Networking/index.html)에 정말 친절하고 자세하게 나와있으니 살펴보시길 바랍니다.



