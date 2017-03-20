# What is agent?
```
| Robot  |<- (sensor) -- | wrold |
| (agent)|-- actions --> |       |
```

![img](http://cs-alb-pc3.massey.ac.nz/notes/59302/fig02.01.gif)

# Reflex-based agent (=Rule-based)

```c
if(condition1)
{
    action1();
}
else if(condition2)
{
    action2();
}
```

![reflex agent](http://cs-alb-pc3.massey.ac.nz/notes/59302/fig02.07.gif)

# State-based agent (model-based)

- 현재 정보
- 과거 정보
- my action

![state based](http://cs-alb-pc3.massey.ac.nz/notes/59302/fig02.09.gif)

# Goal-based agent


- 현재 정보
- 과거 정보
- my action
- Goal


if my goal = being happy

|state| value|
|---|---|
|married| (T) or F|
|having(child)| (T) or F|
|having(enaughMoney)| (T) or F|
|healthy| (T) or F|
|alive| (T) or F|

![goalbased](http://cs-alb-pc3.massey.ac.nz/notes/59302/fig02.11.gif)

# Utility-based agent

수치화, 정량화

Goal-based와 유사하나, goal based에서 수치화,정량화 가능한것을 변경함 eg) money

# ETC

### Social agent
### Learning Agent