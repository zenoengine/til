# Q-Network 분석

Loss를 targetQ와 PredictQ의 차이값으로 설정하여

Weight 값들이 TargetQ에 형태로 접근하는 것이 인상적이었습니다.

```py
# loss = (targetQ - PredictQ)^2
loss = tf.reduce_sum(tf.square(nextQ - Qout))
```

# np.identity(16)[s:s+1]

아래 코드에서 feed_dict에 input state를 넣어주는 코드가 어떤 의미인지, 왜 이렇게 작성했는지 이해하지 못했었습니다.

```py
# Feed Forward
inputs1 = tf.placeholder(shape=[1, 16], dtype=tf.float32)
W = tf.Variable(tf.random_uniform([16,4], 0, 0.01))
Qout = tf.matmul(inputs1, W)
predict = tf.argmax(Qout, 1)

a, allQ = sess.run([predict, Qout], feed_dict={inputs1:np.identity(16)[s:s+1]})
```

Python 에 익숙하지 않아 문법도 처음 보는  작성되어있는 코드였는데요.

16*16 매트릭스에서 특정 행을 split 해서 1 차원 배열로 반환합니다.

이해를 돕기위해 아래 코드를 첨부합니다.

추가적으로, numpy.split에 대해서 찾아보면 좋을것같습니다.

```py
mat = np.identity(4)
mat[0][1] = 2

print(mat)
print(mat[0:1])
```
```py
[[1. 2. 0. 0.]
 [0. 1. 0. 0.]
 [0. 0. 1. 0.]
 [0. 0. 0. 1.]]

 [[1. 2. 0. 0.]]
 ```

결론적으로, 현재 위치(s) 인덱스를 가지고, Agent의 위치를 표현하는 1-D State Array를 만드는 코드였습니다.

# epsilon 

epsilon을 제어하는 방법도 재미있었습니다. episode를 진행하면서 epsilon을 감소시키고 exploration(탐험, 랜덤 액션)을 줄이는 코드입니다.

learning_rate 관련해서는 episdoe가 진행되었을때 변동을 주면 학습에 도움이 되겠다 라는 생각을 했었는데, epsilon 관련해서도 적용할수 있다는 시각을 준 코드였습니다.

```py
e = 1./((i/50) + 10)
```

### Reference Code Repo : https://github.com/awjuliani/DeepRL-Agents