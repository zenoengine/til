# Droup out

Droup out은 네트워크에서 무작위로 네트워크 유닛을 꺼버리는 일반화 기법으로, Over-fitting 현상에 도움이 됩니다.

# Invert Drop out

A에 Keep-Prob 의 역수를 곱해, 0가 된 부분의 값을

# Drop out은 왜 동작할까?

Input Feature가 임의로 비활성화 되므로 

1. 더 작은 네트워크에서 학습하는것과 같은 효과과 됩니다.
2. 특정 Feature에만 의존하지 않게 되고, Weight(가중치)가 분산됩니다. 이는 L2 일반화와 비슷한 효과를 가집니다.

# Drop out 사용 여부

Drop out이 대표적으로 사용되는 분야는 컴퓨터 비전 분야입니다.

이미지 데이터 확보가 어렵기에 많은 경우 Over-fitting 되어버리는데, Drop out을 사용해

Over-fitting 방지합니다.

이처럼 풀려는 문제가 Over-fitting이 있을 경우에만 사용하는 것을 추천합니다.

# 단점

단점으로는 무작위로 노드를 꺼버리기 때문에 Cost Function J가 명확하게 정의되지 않는다는 점입니다. 

이를 극복하기 위에서 Drop out Keep-Prob = 1.0으로 설정하여 모든 노드를 끄지 않고,
학습을 진행해 Cost Function J가 지속적으로 줄어드는지 확인하고, Drop out을 켜고 본 학습을 진행하는 방법이 있습니다.