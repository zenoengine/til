Unreal Engine 4의 collision presets 중에서

액터에 물리 프리셋을 'IgnoreOnlyPawn'과 'OverlapOnlyPawn' 설정하니 지형을 뚫고 떨어져버리는 동작을 하길래 이상해서 문의해보았습니다.

질문의 요지는 오직 pawn 만 무시(겹침)인데, 왜 떨어지죠 ㅠㅠ...?

```
Hi,
When i use collision preset 'IgnoreOnlyPawn'. but applied actor was fall in the terrain.
so, I readed comment and document.
My hypothese 'IgnoreOnlyPawn' preset is made for kismetic object. or preset wanted to make that
dynamic block all, but exclude pawn. (also 'OverlapOnlyPawn').
I think that second theory is correct. But I'm not sure. Am i wrong?

thank you :)

Here is referenced documents
https://docs.unrealengine.com/latest/INT/Engine/Physics/Collision/index.html
https://docs.unrealengine.com/latest/INT/Engine/Physics/Collision/Reference/index.html
```

그에 따른 답변

```
Hi zenoengine,

Thank you for your contribution to the Engine.
One of our engineers looked over your proposed change, and unfortunately we have decided not to bring it into the Engine.
We are very wary of making changes to default profiles, since it could easily break someone else's project. 
In general, those profiles are designed for things like triggers, not stuff that will simulate. 
You probably need a custom profile for that.

Tim
```

두가지 옵션은 Trigger를 고려해 설계된 옵션입니다. 물리 시뮬레이션 + pawn 무시 를 사용하고싶으시다면, custom으로 사용해주세요

였습니다.