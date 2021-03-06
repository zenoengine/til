# Weighted Mean


Wdigted Mean이란 각 요소의 값과  대응되는 가중치를 곱해서 모두 더한 값을, 가중치의 합으로 나누는 것이다.

수식으로 표현하면 다음과 같다.

$$ \frac{\sum{^n_{i=1} ({x_i} * {w_i}}) } {\sum^n_{i=1}w_i}  $$

A라는 학생의 한 과목 점수 계산을 예시로 들어 이해해보자.

A 학생의 한과목 점수의 가중치가 다음과 같이 정해져있고

|항목|가중치|
|---|---|
|중간고사|40|
|퀴즈|10|
|출석|10|
|기말고사|40|

획득한 점수가 다음과 같을 때

|항목|점수|
|---|---|
|중간고사|70|
|퀴즈|90|
|출석|100|
|기말고사|75|

각 항목당 점수와 가중치를 곱해서 모두 더한뒤 가중치의 합으로 나눈다.

$$ \frac{(40*70) + (90*10) + (100*10) + (75*40)}{100} = \frac{2800 + 900 + 1000 + 3000}{100} = \frac{7700}{100} = 77 $$

출석과 퀴즈를 높게 맞았지만 가중치가 높지 않아

평균이 많이 오르지 않은 안타까운 모습을 살펴 볼 수 있다.

이것이 WeightedMean이다.