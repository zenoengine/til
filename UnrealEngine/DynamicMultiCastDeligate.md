# Unreal engine 4 에서 Dynamic Multicast Deligate 사용하기

UE4 에서 Dynamic Multicast deligate를 사용하면 Observer pattern 같은 것을 쉽게 구현할 수 있도록 도와줍니다.


## 1. 매크로를 통해 타입을 정의한다.

```c
DECLARE_DYNAMIC_MULTICAST_DELEGATE(FSubjectName);
```

## 2. 만든 타입을 선언한다.

```
FSubjectName OnSomethingHappend;
```

## 3. BroadCast한다.

```
OnSomethingHappend.BroadCast();
```

---

## 4. Listner에서 Delegate 선언

```
UFUNCTION() // 호출하려면 반드시 UFUNCTION 매크로를 위에 적어줘야합니다. 
void DelegateMethod(); // 예) void OnMonsterDeath();
```

## 5. 등록하기
```
BroadcastingInstance->OnSomethingHappend.AddUniqueDynamic(this, &Listner::DelegateMethod);
```