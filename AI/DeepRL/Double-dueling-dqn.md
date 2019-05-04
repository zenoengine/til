# Double-Dueling-dqn 분석

핵심 3가지

1. Conv Network
 - 사람이 화면을 보고 플레이 하는것처럼, 합성곱 신경망을 통해 학습
2. Experience Replay
 - 과거 경험했던 히스토리를 기반으로, 일정 주기마다 학습한다. Correlation 현상 완화 
3. Sperate network
 - Q Target을 따로 두어, 일정 주기마다 Main-Q Network를 업데이트한다.

Atari이후 DQN 을 개선하기 위한여러가지 방법들이 나왔으며, 그중 하나는 Dueling이다.

# Dueling

Q-Table의 Q(가치)값을 아래처럼 2개로 나눌 수 있다.

1. 어떤 행동을 하지 않아도, 그 상태에 자체의 가치

2. 해당 State에서 행동을 함으로 서 얻는 가치

```py
self.streamAC,self.streamVC = tf.split(self.conv4,2,3)
        self.streamA = slim.flatten(self.streamAC)
        self.streamV = slim.flatten(self.streamVC)
        xavier_init = tf.contrib.layers.xavier_initializer()
        self.AW = tf.Variable(xavier_init([h_size//2,env.actions]))
        self.VW = tf.Variable(xavier_init([h_size//2,1]))
        self.Advantage = tf.matmul(self.streamA,self.AW)
        self.Value = tf.matmul(self.streamV,self.VW)
        
``` 

그리고 1,2를 합산하여 다시 Q Value를 산출한다.

```py
self.Qout = self.Value + tf.subtract(self.Advantage,tf.reduce_mean(self.Advantage,axis=1,keep_dims=True))
self.predict = tf.argmax(self.Qout,1)
```

# Reshape

Reshape에서 -1값을 넣으면 tensor의 값에 따라 맞는 형태로 reshape한다.

```py
# tensor 't' is [[[1, 1, 1],
#                 [2, 2, 2]],
#                [[3, 3, 3],
#                 [4, 4, 4]],
#                [[5, 5, 5],
#                 [6, 6, 6]]]
# tensor 't' has shape [3, 2, 3]
# pass '[-1]' to flatten 't'
reshape(t, [-1]) ==> [1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4, 5, 5, 5, 6, 6, 6]

# -1 can also be used to infer the shape

# -1 is inferred to be 9:
reshape(t, [2, -1]) ==> [[1, 1, 1, 2, 2, 2, 3, 3, 3],
                         [4, 4, 4, 5, 5, 5, 6, 6, 6]]
# -1 is inferred to be 2:
reshape(t, [-1, 9]) ==> [[1, 1, 1, 2, 2, 2, 3, 3, 3],
                         [4, 4, 4, 5, 5, 5, 6, 6, 6]]
# -1 is inferred to be 3:
reshape(t, [ 2, -1, 3]) ==> [[[1, 1, 1],
                              [2, 2, 2],
                              [3, 3, 3]],
                             [[4, 4, 4],
                              [5, 5, 5],
                              [6, 6, 6]]]
```

# // operator

Floor Division(//) operator 

C++ int value 나눗셈과 같은 동작을 합니다.

나눈 후 몫을 소수를 제외한 정수 값만 표시

```py
>>> 3/4 # output : 1
>>> 10//3 # 3
```