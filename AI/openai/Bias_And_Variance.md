# Bias and Vairance

고양이와 개사진을 보여주면서 분류하는 문제라면, 사람이 풀경우 거의 틀리지 않을것입니다.

이런 문제를 AI가 풀었다고 가정할 때, 각 케이스별로, 현재 AI가 갖고있는 편향과 편차에 대한 문제를 진단할 수 있습니다.

| - |훈련 데이터 | 개발용 데이터 | 결과 |
|---|---|---|---|---|
|Case 1(Error Percentage)|1%|11%|High Variance|
|Case 2(Error Percentage)|15%|16%|High Bias|
|Case 3(Error Percentage)|15%|30%|High Bias & High Variance|
|Case 4(Error Percentage)|0.5%|1%|Low Bias & Low Variance|

만약, 문제가 복잡해지고, 맞추기 어려워 질 경우에는 오차률에 대한 보정을 생각해야합니다.

아주 흐릿한 이미지를 맞추기 문제에서 사람도 15% 정도 틀린다면, Case 3번에서의 오차률이라면, 괜찮은 정도일수도 있으니까요.

# Bias 가 큰경우

- 네트워크의 규모를 키운다.
- 학습 시간을 더 투자한다.
- (NN 아키텍쳐를 수정한다)

# Vairance가 큰경우
 - 데이터를 더 수집한다.
 - 일반화한다.
 - (NN 아키텍쳐를 수정한다)

 # Bias Virance Trade off
 딥러닝 세대에서는 Trade off에 대한 이슈가 잘 대두되지 않습니다.

 
 더 큰 네트워크를 사용하면, Bias를 줄이고, Viarance에 영향없이 줄일 수있고

 더 데이터를 수집하면, Viarnace를 줄이면서 Bais에도 영향이 없습니다.

 이 때문에, 딥러닝이 최근 지도 학습분야에서 부각되고 있습니다.