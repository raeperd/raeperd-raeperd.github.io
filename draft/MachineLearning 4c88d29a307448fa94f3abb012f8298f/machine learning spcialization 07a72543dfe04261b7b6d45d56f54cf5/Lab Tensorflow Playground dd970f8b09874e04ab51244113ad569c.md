# Lab: Tensorflow Playground

[Tensorflow - Neural Network Playground](http://playground.tensorflow.org/#activation=linear&regularization=L2&batchSize=1&dataset=spiral&regDataset=reg-plane&learningRate=0.01&regularizationRate=0&noise=0&networkShape=&seed=0.84739&showTestData=false&discretize=false&percTrainData=50&x=true&y=true&xTimesY=true&xSquared=true&ySquared=true&cosX=false&sinX=true&cosY=false&sinY=true&collectStats=false&problem=classification&initZero=false&hideText=false&showTestData_hide=true&learningRate_hide=true&regularizationRate_hide=true&percTrainData_hide=true&numHiddenLayers_hide=true&discretize_hide=true&activation_hide=true&problem_hide=true&noise_hide=true&regularization_hide=true&dataset_hide=true&batchSize_hide=true&playButton_hide=false&stepButton_hide=true)

![Lab%20Tensorflow%20Playground%20dd970f8b09874e04ab51244113ad569c/Untitled.png](Lab%20Tensorflow%20Playground%20dd970f8b09874e04ab51244113ad569c/Untitled.png)

- 새로운 feature를 찾을 필요가 있다.
- 더 복잡한 모델을 사용하는 것이 대답일 때가 많다.

[Tensorflow - Neural Network Playground](https://goo.gl/VyoRWX)

- non-linear feature 를 추가하는 것이 아니라 batchsize와 layer의 depth 만을 조정하는 것으로 non-linear decision boundary 를 찾는것이 가능하다.
- 중간 레이어의 유닛을 4개 정도 하니까 좋은거 같음

![Lab%20Tensorflow%20Playground%20dd970f8b09874e04ab51244113ad569c/Untitled%201.png](Lab%20Tensorflow%20Playground%20dd970f8b09874e04ab51244113ad569c/Untitled%201.png)

- batch size가 커지면 loss 가 smooth 해진다.
- batch size가 커지면 데이터 하나하나의 노이즈가 상쇄되는 경향이 있음

![Lab%20Tensorflow%20Playground%20dd970f8b09874e04ab51244113ad569c/Untitled%202.png](Lab%20Tensorflow%20Playground%20dd970f8b09874e04ab51244113ad569c/Untitled%202.png)

- 머신러닝 이전의 데이터 사이언티스트는 피쳐 엔지니어링에 시간을 많이 쏟았는데 이제는 모델 자체가 일종의 feature engineering 을 해주는 것으로 이해할 수 있다.
- 일종의 오버피팅이 감지됨

# Loss Curve Trobleshooting

- cost function이 local minima 에 갖히는 것은 여러 복잡한 원인들이 있다. ⇒ 원인을 알기가 힘들다.
- performance matrix 를 통해 좀더 좋은 트러블 슈팅을 할 수 있다.