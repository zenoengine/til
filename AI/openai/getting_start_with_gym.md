# Open AI Gym 시작하기

Gym은 강화학습 알고리즘을 개발, 비교하기 위한 개발 도구이며, Tensorflow나 Theano 같은 수치 계산 라이브러리와도 호환됩니다.

# 설치

pip 명령어를 통해 gym을 설치합니다.(python 3.5+)

```
pip install gym
```

# 환경들

최소한에 시작할 수 있는 예제 코드입니다.

이 코드는 CarPole-v0 환경을 1000번의 스텝을 반복하고, 각 스텝마다 렌더링합니다. 

아래 코드를 실행하게 되면 고전적인 cart-pole 문제가 렌더링되는 윈도우 창을 볼 수 있습니다. 

```py
import gym
env = gym.make('CartPole-v0')
env.reset()
for _ in range(1000):
    env.render()
    env.step(env.action_space.sample()) # take a random action
env.close()
```

일반적으로, 카트가 화면 밖으로 벗어나기도전에 시뮬레이션이 끝날 것 입니다.

만약, 다른 환경에서 액션을 테스트 해보고싶다면 CartPole-v0 대신에 MountainCar-v0, MsPacman-v0([Atari](https://github.com/openai/gym#atari)종속성이 필요), Hopper-v1([Mujoco](https://github.com/openai/gym#mujoco)종속성이 필요)로 교체할 수 있습니다.
모든 환경들은 Env 라는 Base Class에서 파생 되었습니다.

만약 종속성 에러가 출력된다면, 메세지가 도움이 될것입니다. 설치 이슈에 관한 자세한 [문서](https://github.com/openai/gym#environment-specific-installation)

# 관찰

스텝마다 랜덤한 액션을 하는 대신에 더 나아가고 싶으실 것입니다. 그러기 위해서는, 우리의 액션이 환경에 어떤 영향을 끼치는지 아는것이 좋을것입니다.

환경에서 step 함수는 4개의 값을 리턴합니다. 
4개의 값은 아래와 값습니다.

|이름|설명|
|---|---|
|obeservation(object)|환경 정보를 표현하는 특별한 오브젝트입니다. 예를 들어 카메라가 담는 픽셀 정보, 로봇 관절의 각도, 속도, 보드 게임에서의 보드 상태 등 이있습니다.|
|reward(float) |이전 액션으로 성취한 리워드(보상) 값입니다. 환경마다 보상값의 크기가 다르지만, 모든 문제에서 리워드를 많이 얻는것이 목표가 됩니다.|
|done(boolean)| done값이 True라면, 에피소드가 종료 되었음을 의미합니다. 에피소드를 reset 합니다.|
|info(dict)| 디버깅에 유용한 진단 정보입니다. 가끔은 학습에도 유용하게도 사용될 수 있습니다. 하지만, 공식적으로는 이 정보를 Agent가 학습하는데 사용하는 것은 허락하지 않습니다. |

아래 그림은 "agenet-enviornemt loop"입니다. 1회 마다 Agenet는 행위(액션)을 선택하고,
Enviornment(환경은) observation과 reward를 리턴합니다.

![agent-enviornment loop](https://gym.openai.com/assets/docs/aeloop-138c89d44114492fd02822303e6b4b07213010bb14ca5856d2d49d6b62d88e53.svg)

```py
import gym
env = gym.make('CartPole-v0')
for i_episode in range(20):
    observation = env.reset()
    for t in range(100):
        env.render()
        print(observation)
        action = env.action_space.sample()
        observation, reward, done, info = env.step(action)
        if done:
            print("Episode finished after {} timesteps".format(t+1))
            break
env.close()
```