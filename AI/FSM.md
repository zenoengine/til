# What is a FSM?

Only one state at a time;
by a triggering event or condition, it  change its state from one to another; this is called a transition

# Key Elements of a FSM

- A List of the machine's state

- Behaviors and/or actions in each state

- Transition

# What is a state?

Typicall,y a state = a set of conditions

Hard to define a state with  finite condtions in real life

# How to represent a FSM?

The best way to visualize a fsm is to think of it as a directed **graph** of the states.

![light switch](https://camo.githubusercontent.com/3373266dc8a8fc6c8f702d20350bd771bb78d214/687474703a2f2f7777772e61692d6a756e6b69652e636f6d2f6172636869746563747572652f73746174655f64726976656e2f7475745f7374617465315f66696c65732f696d6167653030322e6a7067)


# Turnstile

```

Ticket : Lokced - >  UnLocked
One-person-pass/lock : Lokced < -  UnLocked

```
ecceotuib
```c
if locked
    try pass
    alert

if unlocked
    ticket
    "thank you!"
```

```
Ticket : Lokced - >  UnLocked
One-person-pass/lock : Lokced < -  UnLocked

pass/alram : Locked -> Locked
Ticket/say "Thank U" : Unlokced -> Unlocked
```


Complained

```c
if Locked
    try pass
    alarm and ChangeState(EMERGENCY)

if Emergencystate
    confirm and reset
    Chagestate(LOCKED)
```

```
Ticket : Lokced - >  UnLocked
One-person-pass/lock : Lokced < -  UnLocked

confirm/insert : Locked -> Emergency
pass/alram : Emergency ->  Locked 
Ticket/say "Thank U" : Unlokced -> Unlocked

```

# PACKMAN Ghost

Wander, Chase, Attack, Flee