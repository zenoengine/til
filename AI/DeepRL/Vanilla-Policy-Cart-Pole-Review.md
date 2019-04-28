# Vanilla-Policy-Cart-Pole Review


플레이한 history 리스트를 가지고 역순으로 순회하면서 discount factor를 적용하는 부분이 재미있었습니다.


```py

def discount_rewards(r):

""" take 1D float array of rewards and compute discounted reward """

discounted_r = np.zeros_like(r)

running_add = 0

for t in reversed(xrange(0, r.size)):

running_add = running_add * gamma + r[t]

discounted_r[t] = running_add

return discounted_r

```


python에 익숙하지 않아, 이 부분도 갸우뚱 했는데요.


```py

s1,r,d,_ = env.step(a) #Get our reward for taking an action given a bandit.

ep_history.append([s,a,r,s1])


ep_history = np.array(ep_history)

ep_history[:,2] = discount_rewards(ep_history[:,2])

```


ep_history[:,2]이 코드는


모든 행에 대해서 2 열 (reward) 만 split해옵니다.

```py

[[s,a,r,s1],

[s,a,r,s1],

[s,a,r,s1]]


[r,r,r]

```


# argmax(a_dist == a)


agent의 행동 가능한 액션 출력값이 확률과 같기 때문에


np.random.choice에 p argument로 자신의 begin을 넣는점이 재미있었고,


argmax에서 비교연산자를 통해 [True, False] 형태로 만들어 선택된 값을 뽑아오는데, explict loop하게 가져오는 부분도 재미있었습니다.


```

a_dist = sess.run(myAgent.output,feed_dict={myAgent.state_in:[s]})

a = np.random.choice(a_dist[0],p=a_dist[0])

a = np.argmax(a_dist == a)

```


# self.indexes




```py

self.state_in = tf.placeholder(shape=[None, s_size], dtype=tf.float32)

hidden = slim.fully_connected(self.state_in, h_size, biases_initializer=None, activation_fn=tf.nn.relu)

self.output = slim.fully_connected(hidden, a_size, biases_initializer=None, activation_fn = tf.nn.softmax)

self.chosen_action = tf.argmax(self.output,1)


self.reward_holder = tf.placeholder(shape=[None], dtype=tf.float32)

self.action_holder = tf.placeholder(shape=[None], dtype=tf.int32)


# 이 아래 코드가 잘 이해가지 않았엇는데요,

self.indexes = tf.range(0, tf.shape(self.output)[0]) * tf.shape(self.output)[1] + self.action_holder

self.responsible_outputs = tf.gather(tf.reshape(self.output, [-1]), self.indexes)

```


feed 되는 state_in 의 shape이 [None, s_size] 인것을 잘 살펴봐야했습니다.



```py

feed_dict={myAgent.reward_holder:ep_history[:,2],

myAgent.action_holder:ep_history[:,1],myAgent.state_in:np.vstack(ep_history[:,0])}


grads = sess.run(myAgent.gradients, feed_dict=feed_dict)

```


간단하게 설명하면,


state 히스토리가 넘어오는데, 1차원 배열 형태로 넘어오게 됩니다.


각 state마다 할 수 있는 액션의 개수가 있죠?


2개의 state history가 feed되면 output으로 4개의 action policy 배열이 나오게됩니다.


Indexes는 출력된 4개의 output)에서 선택한 action의 index 값으로 세팅됩니다.


```py

output : [0.1, 0.2, 0.3, 0.25]

indexes = [1 3]


# output 에서 1, 3 weight만을 pick!

self.responsible_outputs = tf.gather(tf.reshape(self.output, [-1]), self.indexes)

```


마지막으로 gather 함수를 통해, 선택 된 output tensor들만을 선택하게 됩니다.

이해를 돕기 위해 [예제 코드 링크](https://github.com/zenoengine/openai-playground/blob/master/classic-control/Understanding-Vanilla-Policy.py)를 첨부합니다. :)

### Reference Code Repo : https://github.com/awjuliani/DeepRL-Agents