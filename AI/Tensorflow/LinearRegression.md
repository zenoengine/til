# tf.train 

tf.train 는 트레이닝 모델을 돕는 클래스들와 함수들을 제공합니다.

# Optimiazers

Optimizer 기반 클래스들은 기울기를 계산해서 변수에 적용할 방법들을 제공합니다.
전통적인 최적화 알고리즘을 구현한 클래스들도 있습니다. GradientDescent나 Adagrad 처럼요.

Optimizer class 자체를 인스턴스화 하는 대신 sub class들을 인스턴스화 해서 사용합니다.


# Gradient descent algorithm

경사 하강법.