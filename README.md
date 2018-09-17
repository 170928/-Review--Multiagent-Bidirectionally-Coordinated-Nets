# -Review- Multiagent-Bidirectionally-Coordinated-Nets

### [Abstract]
많은 AI application들은 협력 (collaborative) 작업을 위해서 다중 agent들이 필요합니다.  
agent간의 의사소통과 협력을 위해서 효과적인 학습 방법은 general AI를 위해서 필수적인 단계입니다.  
이 논문에서는 StarCraft 를 case study로 사용합니다.  
확장 가능성이 있으며, 효과적인 communication protocol을 유지하기 위해서, 이 논문은 Multiagent Bidirectionally Coordinated Network (BiCNet) 을 소개합니다.  
이 구조는 actor-critic formulation의 벡터화된 확장 식을 사용합니다.  
그 결과, 제안하는 모델은 임의의 AI agnet들과의 협력적인 플레이를 이끌어 낼 수 있다는 것을 보여주었습니다.  
뿐만아니라, BiCNet은 경험있는 사람의 행동처럼 진보된 수준의 협력적인 플레이를 배울 수 있습니다.  

### [Introduction]
이 논문에서는, multiagent learning 을 "zero-sum Stochastic Game"으로써 모델링 합니다.  
Agent들은 BiCNet에 의해서 서로 communication 합니다.  
반면에 learning 은 multiagnet actor-critic framework를 사용하면서 이루어집니다.  
뿐만아니라, "parameter sharing"기법을 사용해서 확장성 문제를 다룹니다.  
그 결과, BiCNet은 자동적으로 다양한 agent들 간에 협력을 위해서 최적의 전략을 학습하는 것을 보여줍니다.  

### [Multiagent Bidirectionally Coordinated Nets]
Starcraft combat 환경은 한 측(편)의 다중 agent들은 완전히 협력적 (fully cooperative) 한 모습을 보여주며 상대방과 싸웁니다.  
그러므로, 각각의 전투는 N agent들과 M enemie들 간의 "zero sum competitive game"으로써 해석될 수 있습니다.  
이 논문에서는 "zero-sum Stochastic Game  (SG)"로 구성합니다.  

SG는 tuple 로써 표현될 수 있습니다. ![image](https://user-images.githubusercontent.com/40893452/45604640-4ebed480-ba71-11e8-82ff-58b2ac3f1bcb.png)  
  
1. S는 현재 게임의 state space를 의미하며, 모든 agent들이 공유합니다.  
2. Initial state s(1) 은 p1(s)를 따릅니다.  
3. controll 하는 agent의 action space를 A(i)로써 정의합니다.  
4. enemy의 action space를 B(j)로써 표현합니다.  
5. T는 deterministic transition function을 의미합니다. ![image](https://user-images.githubusercontent.com/40893452/45604696-e7555480-ba71-11e8-90cd-5a7efeca58b6.png)  
6. R(i)는 각 agent i 의 reward function을 의미합니다.  

임의의 숫자의 agent들에게 유연한 framework를 만들기 위해서, 이 논문에서는 다음과 같은 사항들을 고려합니다.  

1. multiple agents  
2. controlled agent  
3. enemy agents  
4. 현재 게임의 state S 를 모든 agent들이 공유  
5. multiple agent들은 샅은 action space A와 B를 갖습니다.  
(* continuous control output을 고려하며, attack angle과 distance 를 output으로 정합니다 *)  

Reward function으로는 다음과 같이 정의합니다.  
"연속된 2 time step사이에서의 health level의 차이에 기반하는 time variant global reward"를 reward function으로써 정의합니다.  
![image](https://user-images.githubusercontent.com/40893452/45604843-51222e00-ba73-11e8-9771-c3b06fb63d95.png)  


이때, reward function에서의 reduced health level 수식은 다음과 같은 형태입니다.  
![image](https://user-images.githubusercontent.com/40893452/45604843-51222e00-ba73-11e8-9771-c3b06fb63d95.png)  


위의 reward function은 controll 하는 agent의 측면에서의 수식으로, enemy agent입장에서는 반대가 됩니다.  
그러므로, controll agent의 reward와 enemy agent의 reward의 합은 0 이되는 zero-sum game이 됩니다.  

agent들의 목적은 discounted rewards 의 expected sum ![image](https://user-images.githubusercontent.com/40893452/45604898-e7eeea80-ba73-11e8-82fb-e842a6d20089.png)을 최대화 시키는 정책을 학습하는 것입니다.  
반면, enemy들의 "joint policy" 는 controlled agent들의 "joint policy"의 expected sum을 최소화 시키는 것이 됩니다.  
> joint policy란 multi agent들의 협력과정을 위한 policy를 의미합니다.  

그러므로, Minimax game으로 SG game 의 action-value function을 다음과 같이 표현할 수 있습니다.  
![image](https://user-images.githubusercontent.com/40893452/45604916-110f7b00-ba74-11e8-8c75-82327332f1b4.png)  
action-value function은 "Bellman Optimal Equation"을 따릅니다.  

이 논문에서는, deterministic policy를 cotrolled agent들과 enemy agents들에게 적용시키며,  
enemy의 policy는 고정되어 있다고 가정합니다.  
그 결과 위의 (2) 수식은 다음과 같이 변경됩니다.  
![image](https://user-images.githubusercontent.com/40893452/45604991-8b3fff80-ba74-11e8-9ba5-488bcedf9fdc.png)  
(* (2)의 수식에서 enemy policy가 고정되는 것으로 인해 notation이 빠지게 됩니다 *)  


### [Local, Individual Rewards]
그러나, (1) 식의 reward function을 사용하는 것은 team collaboration 이 local collaboration이 모여서 이루어진 것이라는 점을 배제 시킵니다.  
그러므로, agent들이 team collaboration을 이끌어 낼 수 있는 개개의 objective function을 갖게 하는 것이 필요합니다.  
이를 위해서, (1) 식을 agent들의 local reward 식으로 변경합니다.  
이 식은 agent의 다른 agent들과의 협업에 대한 공언도를 측정하여 반영 합니다.  
![image](https://user-images.githubusercontent.com/40893452/45605118-7617a080-ba75-11e8-966f-aa7f91a22cd0.png)  
각각의 agent i 는 top-K-u(i)와 top-K-e(i)를 유지합니다.  
top-K 는 현재 상호작용하는 동료 agent들과 적 agent들을 의미합니다.  
그러므로, 같은 parameter 로써 표현되는 policy function 을 위한 Bellman equation 이 총 N개 생겨납니다.  

![image](https://user-images.githubusercontent.com/40893452/45605118-7617a080-ba75-11e8-966f-aa7f91a22cd0.png)  

(* 식 (3)에서는 agent i 별로 나뉘지 않았으나,식 (5)에서는 Q_i (s, a) 가 됩니다. *)  

### [Communication w/ Bidirectional Backpropagation]
비록 (5) 번 식이 signle-agnet 들을 위해서 사용할 수 있지만, team-level collaboration을 위해서는 다소 부족한 메커니즘입니다.  
그러므로, 이 논문에서는 agent들 사이에 communication을 BiCNet을 통해서 수행합니다.  
BiCNet은 multiagent actor network와 multiagnet critic network들로 구성되어 있습니다.  
기본적으로 actor-critic 구조를 따르므로 actor / critic network 가 존재합니다.  
actor 와 critic network는 모두 bi-directional RNN 구조를 사용합니다.  
actor networks 는 shared obseravtion 정보 (모든 agent들이 같은 state 를 공유하므로 shared observation 이라 표현) 를 기반으로 각각의 agent들을 위한 action을 반환합니다.  

bi-directional recurrent structure 는 (1) communicatio channel 뿐 아니라 (2) local memory saver 로써도 역할을 수행합니다.  
각각의 individual agent는 자신의 "internal state"를 recurrent structure내에 유지합니다.  
뿐만 아니라 협력을 위해서 다른 agent들에게 공유합니다.  

![image](https://user-images.githubusercontent.com/40893452/45605458-0e168980-ba78-11e8-9b3e-a35a3ea41168.png)

이를 위해서, N 개의 unfold RNN structure를 사용하여 모델이 구현됩니다.  
학습 과정에서의 gradient는 개개의 Q (action-value function) 과 policy function으로 들어갑니다.  

이러한 과정을 수학적으로 다음과 같이 정리될 수 있습니다.  
1. 



### [Experiments]



