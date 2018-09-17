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
  
S는 현재 게임의 state space를 의미하며, 모든 agent들이 공유합니다.  
Initial state s(1) 은 p1(s)를 따릅니다.  
controll 하는 agent의 action space를 A(i)로써 정의합니다.  






### [Local, Individual Rewards]



### [Communication w/ Bidirectional Backpropagation]



### [Experiments]



