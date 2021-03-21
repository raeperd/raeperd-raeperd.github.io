---
title: "Markov Decision Processes"
date: 2020-06-11T02:32:36+09:00
tags : ["ml", "rl"]
---



# INTRO



# CONETNS



##  Markov Process



###  Introduction to MDPs

- Markov decision processes formally describe an environment for reinforcement learning
- Where the environment is fully observable
- i.e. The current state completely characterises the process
- Almost all RL problems can be formalised as MDPs, e.g. 
  - Optimal control primarily deals with continuous MDPs 
  - Partially observable problems can be converted into MDPs 
  - Bandits are MDPs with one state



###  Markov Property

"미래는 현재를 고려할때, 과거와 완전히 무관하다."

![image-20200603115558095](https://drive.google.com/uc?export=view&id=12uC2Xpi6mb0PdD_i5oxAxOAIQ89MIo_D)

- State가 history 로 부터 필요한 모든 정보를 가지고 있다.
- State가 알려지면, history는 필요 없다.
- i.e. State는 미래를 알기 위한 충분한 특징이다. 



### State Transition Matrix

Marcov state s와 이후 state s'에 대해, State transition probabilty는 다음과 같이 정의 된다.

![image-20200603153953084](https://drive.google.com/uc?export=view&id=17VX4KenZCe1uKXN4_9lmX5qUoEvHYeiH)



State transition matrix P는 모든 가능한 State transition probabilty를 원소로 가지는 행렬이다.

![image-20200603154118645](https://drive.google.com/uc?export=view&id=1r4wU1_z46A52TYAyPsYn1uJes5Pt1yXx)



### Markov Process

![image-20200603154229389](https://drive.google.com/uc?export=view&id=1yrEIcXmG163NHp2XRbw-f51qZL2dusgh)



#### Example: Student Markov Chain

![image-20200603154516515](https://drive.google.com/uc?export=view&id=1Zgsg0jw5A-uUmnMVhqoxcl4pSdGN8u7W)

![image-20200603154547586](https://drive.google.com/uc?export=view&id=18yBqMnf7ybFDVh7aBcFgBE2EA0bcEk7T)



### Markov Reward Process

- Markov Reward Process = Markov chain + value

![image-20200603154651332](https://drive.google.com/uc?export=view&id=1Gm0UP9Cx5sauaTkb6MeAL_wayFq0gMEg)





#### Example: Student MRP

![image-20200603154728147](https://drive.google.com/uc?export=view&id=1H2do97ek8w8jTzL8iSuUV54omuQiU2sI)



### Return

![image-20200603154755560](https://drive.google.com/uc?export=view&id=16ydjBm646ugk8GqJSGDcAqWmAKuonZ7l)

- r 값은 미래의 reward에 대한 현재의 가치를 표현한다. (discount)
  - r 값이 작으면, 당장의 가치를 더 중요하게 생각한다.
  - r 값이 1이라면, 먼 미래까지 판단을 고려하게 된다.

- 확률적인 값이 아니다. 하나의 시나리오에 대한 상수 값을 얻을 수 있다. 





#### Why discount?

대부분의 Markov reward와 decision process들은 discount 를 사용한다.

- 수학적으로 편리하다.
- 무한히 순환되는 Markov process를 피할 수 있다.
- 미래에 대한 불확실성까지 완벽하게 반영할 수 없다.
- 인간/동물들의 행동은 당장의 reward에 더 큰 반응을 보인다.
- undiscount를 하는 것도 가능하다.



### Value Function 

valuefucntion v(s)는 state s의 장기적인 가치를 제공한다.

![image-20200603155248406](https://drive.google.com/uc?export=view&id=1sRzUoKjuHYaFkoWAN4zTLly0HcqhQTDv)





### Bellman Equation for MRPs

value function은 두가지 단위로 분해될 수 있다.

- 즉각적인 reward R(t+1)
- 이후 state에 대한 discounted value rv(S(t+1))



![image-20200603155541740](https://drive.google.com/uc?export=view&id=16nDj6uz1RRUtPW4JSS5NctX9RHUo2Ff1)





### Bellman Equation in Matrix Form

![image-20200603155718545](https://drive.google.com/uc?export=view&id=1H9739Nq5PbBOe2Cb4NLNYGClrgpykwq7)

![image-20200603155705112](https://drive.google.com/uc?export=view&id=13sXqDZsovUFUrS8gm8VrVPiutw-mQi87)



- 선형 방정식이기 떄문에 푸는 것이 가능하다.
- 시간복잡도가 O(n^3)로 실질적으로 매번 풀어내는 것은 힘들다.
- 대안으로 여러가지 방법이 있다.
  - Dynamic programming
  - Monte-Carlo evaluation
  - Temporal-Difference learning



## Markov Decision Process

  MDP는 Markov reward process와 decision(action)의 결합이다. 모든 환경이 Markov인 환경이다.



![image-20200603160020190](https://drive.google.com/uc?export=view&id=1_fNYwQjZk88Lm_uq9rMmIB0BE66ME4aG)

- MDP를 푸는 것은 이 상황에서 reward의 합을 다 합친 값을 최대화하는 policy를 찾는 것이다. 



### Policy

![image-20200603160200551](https://drive.google.com/uc?export=view&id=1sWjns3E0W-K9IsJ4_7jLqxfYHL6RBeew)

- 결정을 어떻게 수식으로 표현할 수 있을까? 에 대한 대답
  - 주어진 state에서 다음 action을 선택의 mapping
- MDP의 policy는 현재상태에 의존한다. (not the history)
- i.e. Policy는 시간에는 독립적인 값이다. 





### Value Function

![image-20200603160343831](https://drive.google.com/uc?export=view&id=1v_fY5KFOx9YSgNBFSuToWPTB1QBjFMj7)



![image-20200603160352884](https://drive.google.com/uc?export=view&id=1YNrNu3TiMU_7xxcQlS9VSogLVr-rJmyZ)



### Optimal Value Function

![image-20200603160916287](https://drive.google.com/uc?export=view&id=1olqEnUYzzWvxZmE2fP1fprzrCR9SmVhN)

- q*를 알게 된다면, 현재 상태에서 최선의 선택을 할 수 있다. 







### Optimal Policy

![image-20200603160945745](https://drive.google.com/uc?export=view&id=15D8q2jFggHx1lpwNShXZmh4ObRCdu98p)

- MDP 안에서 항상 deterministic 한 optimal policy 가 존재한다.

- Bellman Optimality Equation은 non-linear다.
- 보통 해가 존재하지 않는다.
- 방정식을 푸는 대신 대안을 사용한다.
  - Value iteration
  - Policy iteration
  - Q-learning
  - Sarsa



### Finding an Optimal Policy

![image-20200625185614659](C:\gdrive\raeperd.github.io\content\post\images\image-20200625185614659.png)

- optiaml value function을 따르는 것이 곧 optimal policy가 된다.
- 모든 MDP 문제는 optimal value function을 알 수있다면 끝나게 되며, 이를 알게되면 주어진 MDP 를 푸는 optimal "deterministic" policy를 얻을 수 있다. 





### Bellman Optimality Equation for v*

 

### Solving the Bellman Optimality Equation 

- Bellman Optimality Equation is not-linear but Bellman expectation equation
- No closed from solution (in general)
- Many iterative solution methods
  - value iteration
  - policy iteration
  - Q-learning 
  - Sarsa








# REFERENCE