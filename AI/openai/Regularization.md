# Variance - Regularization

Variance를 줄이기 위해서는 더 많은 데이터를 수집하면 좋으나, 많은 데이터를 충분히 수집하지 못할경우, 일반화가 도움이 됩니다.

알반화를 어떻게 구현하는지 살펴봅니다.

# Logistic regression

로지스틱 회귀에서 Cost function 에 일정한 값을 더해주는데

널리 알려진 방법으로는 L2 Regularization 이 있습니다.

# L2 vs L1 일반화

L2 일반화는 w 파라메터들의 제곱의 합을, L1 일반화는 w 파라메터들의 합을 구해 Cost Function에 더해줍니다.

w 파라메터들 중에서는 많은 수가 0이기 때문에 (sparse) L2 일반화보다는 덜 된다는 차이가 있습니다.

#### 왜 b는 더하지 않나요?

w는 벡터 파라메터인 반면 b는 1개의 파라메터이기 입니다.

그러므로 Variance에 영향을 주지 않기에, 잘 하지 않습니다.

# Weight decay

Cost function(J)에 w 제곱의 합을 더했으므로, learning 과정에서 영향을 미치게 됩니다.