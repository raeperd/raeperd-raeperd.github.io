---
title: "Introduction to Reinforcement Learning"
date: 2020-06-11T02:32:21+09:00
tags : ["ml", "rl"]
---

# INTRO

# CONETNS

https://www.davidsilver.uk/teaching/



## About RL

![image-20200603142638131](https://drive.google.com/uc?export=view&id=1RBwx7xwJ_WTX3lLFXKKrC93z_9se_cUA)





강화학습이 다른 머신러닝 패러다임과 다른 것

- supervisor가 없고, reward signal 만 존재한다.
- Feedback is delayed, not instantaneous
- 시간이 중요하다. 
- 행동이 다음에 agent가 받을 시계열 정보에 영향을 미친다.



강화학습의 다른 예제

- 게임

- 헬리콥터의 자율 주행 
- 휴머노이드 로봇이 걸을 수 있게 하기 



## Reward

- A reward $R_t$ scalar feedback signal

- 시간 t에서 얼마나 agent가 잘 하고 있는가를 나타내는 지표
- Agent의 목표는 축적된 reward의 값을 최대화 하는 것이다. 



강화학습은 reward hypothesis를 기반으로 한다.



### Reward Hypothesis

- 모든 목표는 축적된 reward의 합의 최대화로 표현될 수 있다. 



### Examples of Rewards

- 게임
  - `+` / `-` reward for increasing/decreasing score
- 헬리콥터의 자율 주행 
  - `+` 의도한 경로대로의 주행
  - `-` 충돌 
- 휴머노이드 로봇이 걸을 수 있게 하기
  - `+` 앞으로 전진하는 행동
  - `-` 넘어짐 혹은 충돌



### Sequential Decision Making 

- Goal: 미래의 reward의 총합을 늘리기 위해 action을 하는 것
- Action은 장기적인 결과를 가질 수 있다.
- Reward는 늦어질 수 있다. 
- Reward의 총합을 늘리기 위해서는 당장의 reward를 포기하는 것이 더 좋을 수도 있다. 



## Environment

![image-20200603144216535](https://drive.google.com/uc?export=view&id=1aKhRC1n6USRh4DHjwvQkfhz58Gkam0YU)



![image-20200603144526623](https://drive.google.com/uc?export=view&id=1FMb-AbhIKBDrnN7jHwl7qp6C0OSZEQNm)

- t에서의 agent:
  - Action $A_t$ 를 수행한다.
  - 관측 $O_t$ 를 받는다.
  - Reward $R_t$ 를 전달 받는다. 

- t에서의 Environment:
  - Actction $A_t$ 를 받는다.
  - 관측 Ot+1 반환한다.
  - Reward Rt+1을 반환한다.



## State

### History and State

- History는 observation, action, reward의 sequence다.
  - H~t~ = O~1~, R~1~, A~1~, ... , A~t-1~, O~t~ , R~t~
- i.e. t시점까지 관측 가능한 모든 변수
- 다음 시점에 일어는 일은 그때까지의 History에 영향을 받는다.
  - Agent는 다음 action을 선택한다.
  - Environment는 observation/reward 를 선택한다. 
- State는다음에 어떤 일이 일어날지 결정하는데 사용된다.
- State는 History의 함수이다.



### Environment State

![image-20200603144526623](https://drive.google.com/uc?export=view&id=1FMb-AbhIKBDrnN7jHwl7qp6C0OSZEQNm)

- Environment의 State는 알 수 없다.
- i.e. Environment가 어떤 기준으로 다음 observation과 reward는 주는 지
- Environment의 State를 알 수 있다고 해도, 무관한 정보가 많이 포함되어 있을 것이다.



### Agent State

- Agent state는 agent의 내부 표현이다.
- i.e. Agent가 다음 action을 하는데 필요한 정보
- i.e. 강화학습 알고리즘에 사용되는 정보
- History의 어떤 함수라도 될 수 있다.



### Markov state

- Markov state는 history로부터 모든 유용한 정보를 가지고 있다. 



![image-20200603145731806](https://drive.google.com/uc?export=view&id=1Q7RjRLDMTHCfl4T9SXNL0NAZzpSWbzbR)

- "미래는 현재의 정보를 안다면, 과거와 무관하다"
- State가 알려진다면, History는 필요없다.
- i.e. State는 미래를 알기 위한 충분한 지표다.
- Environment state는 Markov state다.
- History는 Markov state다.



### Full Observable Environment

Full observability

- Agent는 직접 Environment state를 알 수 있다.

- Agent state = Environment state = Markov state 



### Partially Observable Environments 

Partial observability

- Agent는 간접적으로 Environment state를 알 수 있다.
  - Robot은 카메라로 자신의 절대위치를 알 수 없다.
  - 포커를 하는 agent는 알려진 카드들만 관측 할 수 있다.
- Agent state != Environment state
- 이를 보통 Partially observable Markov decision process 라 한다. 
- Agent는 state를 스스로 만들어야한다. (추측해야한다) 



## Inside An RL Agent 

- 강화학습 agent는 아래의 components를 하나 이상 가질 수 있다.
  - Policy: Agent의 행동 함수
  - Value function: 현재의 state 혹은 action이 얼마나 좋은지 판단하는 함수
  - Model: Agent의 Environments representation



### Policy

- Policy는 Agent의 행동이다.
- State를 Action으로 매핑하는 맵이다.
- Deterministic Policy
- Stochastic Policy



### Value function

- Value fucntion은 미래의 reward에 대한 예측이다.
- State의 가치를 판단하기 위해 사용된다.
- 따라서 Action을 선택하기 위해서도 사용된다.



### Model

- Model은 Environment가 다음에 무엇을 할지 예측한다.
- P는 다음 state를 예측한다.
- R은 다음 reward를 예측한다.



### Categorizing RL agents

- Value Based
  - No Policy
  - Value function
- Policy Based
  - Policy 
  - No Value function
- Actor Critic
  - Policy
  - Value Function



- Model Free
  - Policy and/or Value Function
  - No Model
- Model Based
  - Policy and/or Value Function
  - Model



![image-20200603151612294](https://drive.google.com/uc?export=view&id=1Kti2yhM5qc43Agarp-g7l3fqZDyn74Uw)





## Problems within RL



### Learning and Planning 

연속되는 선택을 하기 위해서는 두가지 근본적인 문제가 있다.



- 강화학습:
  - Environment는 초기에 알려지지 않는다.
  - Agent는 Environment와 상호작용을 한다.
  - Agent는 Policy를 향상시킨다.
- Planning:
  - Environment의 Model은 알려지지 않는다.
  - Agent는 model 내부에서만 계산을 한다.
  - Agent는 Policy를 향상시킨다.



### Exploration and Exploitation 

- 강화학습은 trial-and-error 학습법과 같다
- Agent는 좋은 policy를 찾아야 한다.
- Environment에 대한 경험으로 부터 
- 많은 Reward를 손해보지 않으면서



- Exploration은 Environment에 대한 더 많은 정보를 찾는다.
- Exploitation은 알려진 정보로 부터 더 많은 reward를 얻는다.
- 보통 Exploit 뿐만 아니라 Explore 또한 중요하다.



게임을 예로 들면, 

- Exploration: 실험적인 움직임을 하는 것

- Exploitation: 최선의 움직임을 하는 것



### Prediction and Control

- Prediction: 미래를 추정한다.
  - 주어진 policy를 이용해
- Control: 미래를 최적화 한다.
  - 최선의 policy를 찾는다.





# REFERENCE