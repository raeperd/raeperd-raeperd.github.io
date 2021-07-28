# 6강 Fundamental of machine Learning

---

# Four Branches of Machine Learning

### Supervised Learning

- learning to map input data to known targets (also called annotations), given a set of examples (often annotatted by humans)
    - Seauence genteration : Given a picture, predict a caption describing it.
    - Syntax tree prediction
    - Object detection
    - Image segmentation

### Self-supervised Learning

- Supervised learning without human-annotated labels (Supervised Learning의 특수한 형태)
    - To predict the next frame in a video, given past frames
    - To predict the word in a text, given previous words
    - Autoencoder

### Unsupervised Learning

- Finding interesting transformations of the input data without the help of any targets, for ther purposes of data visualization, data compression, or data denoising, or to better understand ther correlations present in the data at hand.
    - Dimensionality reduction
    - Clustering

### Reincforcement Learning

- An agent receives information about its environment and learns to choose actions that will maximize some reward.
    - A neural network that “looks” at a videogame screen and outputs game actions in order to maximize its score can be trained via reinforcement learning.

![6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled.png](6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled.png)

---

# Evalutating Machine Learning Models

### 2 types of errors

- MSE (Mean Squared Error) for Regression
- Classification Error for Classification

## Hold-out validation

- 데이터 집합을 서로 겹치지 않는 training set과 validation set 으로 무작위 구분을 한 후, training set을 이용하여 분석 모형을 구축하고 validation set을 이용해 분석 모형의 성능을 평가하는 기법

![6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%201.png](6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%201.png)

```python
num_validation = 10000 
np.random.shuffle(data)

valdation_data = data[:num_validation]
training_data  = data[num_validataion:]

model = get_model()
model.train(training_data)
validation_score = model.evaluate(validation_data)

# 
# engineering here 
#

model = get_model()
model.train(np.concatenate([trainig_data,
														validataion_data]))
test_score = model.evaluate(test_data)

```

- 데이터를 분리하기 전 셔플링을 하는 것이 흔함
- 트레이닝을 어느정도 완료하면 test-data 가 아닌 모든 사용 가능한 데이터에 대해 학습을 하는 것이 보통이다.

## k-fold (cross) validation

1. Divide the sample data into k parts.
2. Use k-1 of the parts for training, and 1 for validation.
3. Repeat the procedure k times, rotating the validation set. 
4. Determin an expected performance metric (mean square error, misclassification error rate, confidence interval, or other appropriate metric) based on the result across the iterations

![6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%202.png](6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%202.png)

```python
import numpy as np

data = [] 
k = 4 
num_validation_samples = len(data) // k 

np.random.shuffle(data)

validation_scores = []
for fold in range(k):
    validation_data = data[num_validation_samples * fold:
        num_validation_samples * (fold+1)]
    training_data = data[:num_validation_samples * fold] + # list concat 
        data[num_validation_samples * (fold + 1):]         # except for validataion_data, all other data is training data
    
    model = get_model()
    model.train(training_data)
    validation_score  = model.evaluate(validation_data)
    validation_scores.append(validation_score) 

validation_score = np.average(validation_scores)

model = get_model()
model.train(data)
test_score = model.evaluate(test_data)
```

### Purpose of k-fold cross validation

- The purpose of cross-validation is model checking, not model building. [[ref1](https://stats.stackexchange.com/questions/52274/how-to-choose-a-predictive-model-after-k-fold-cross-validation)]
- To build model, we would need to train on the entire dataset. [[ref2](https://stats.stackexchange.com/questions/52274/how-to-choose-a-predictive-model-after-k-fold-cross-validation)]

## Iterated k-fold validation with shuffling

- 데이터가 매우 적은 경우에 활용
- K-fold를 랜덤하게 여러번 생성함으로써 validation을 반복함

## 모델 평가시 주의할 점

- Training / Validation / Test set 구분 이전에 Random shuffling 수행은 필수다.
    - 초기 주어진 데이터가 class 별로 배치되는 경우가 많음
- 시계열 데이터의 경우 당연히 셔플 하면 안된다.
- Training / Validation / Test set 영역의 데이터 중복 검사

# Data Preprocessing

## Vectorization

- All input and targets in a neural network must be tensors of floating point data (or intergers)
- 어떤 데이터를 처리하던지 벡터로 먼저 변환할 필요가 있다.

## Value normalization

- 일반적으로 neural network에 비교적 큰 값이나 이기종의 값을 대입하는 것은 좋지 못하다.
- an trigger large gradient updates that will prevent the network from converging

### Minmax normalization

![6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%203.png](6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%203.png)

### Z-score normalization

![6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%204.png](6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%204.png)

## Handling missig values

- 삭제
- 대체
    - 평균값 사용
    - 카테고리별 평균값 사용

# Feature Engineering

- 데이터에 대한 자신의 지식을 사용하는 과정
- 기계 학습 알고리즘을 사용하여 데이터가 모델에 들어가기 전에 하드 코딩 된 변환을 데이터에 적용하여 알고리즘 작동을 향상시키는 프로세스
- 특징을 보다 추상화 또는 의미화된 방식으로 표현하여 머신러닝 모델을 효과적으로 구축하는 과정

# Overfitting을 억제하는 방법

- Get more training data.
- Reduce the capacity of the network.
- Add weight regularization.
- Add dropout.

## Reduce the capacity of the network

![6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%205.png](6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%205.png)

- smaller network이 overfitting 늦게 시작하고 Performance가 보다 천천히 나빠지고 있음

## Add weight regularization

- Learning 과정에서 큰 가중치에 대해서 그에 상응하는 penalty를 부과하여 overfitting 억제
    - 많은 경우 overfitting은 가중치 W의 값이 커서 발생한다.

### L1 regularization

![6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%206.png](6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%206.png)

### L2 regularization

![6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%207.png](6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%207.png)

- weght 의 값의 합을 비용함수에 포함하여 weight 값을 규제Adding L2 weight regularization to the model

```python
from keras import regularizers

model = model.Sequential()
model.add(layers.Dense(16, kernel_regularizer = regularizers.l2(0.001),
                    activation = 'relu', input_shape = (10000,)))
model.add(layers.Dense(1, activation = 'sigmoid'))
```

## Add dropout

- Learning 단계: 데이터를 입력할 때마다 dropout 될 뉴런을 random 하게 선택
- Testing 단계: 모든 뉴련을 사용하여 출력값을 생성 (단, 뉴런의 출력값에 dropout 비율의 역수를 곱함)

```python
model = model.Sequential()
model.add(layers.Dense(16, activation = 'relu', input_shape = (10000,)))
model.add(layers.Dropout(0.5))
model.add(layers.Dense(1, activation = 'sigmoid'))

```

# Workflow of Machine Learning

1. Defining the problem and assembling a dataset
    - Classification or Regression
2. Choosing a measure of success 
    - Accuracym precision, recall
3. Deciding on an evaluation protocol
4. Preparing your data
5. Developing a model that does better than a baseline 
6. Scaling up: developing a model that overfits
    - Remember that the universal tension in machine learning is between optimization and
    generalization
    - The ideal model is one that stands right at the border between underfitting and overfitting;
    between undercapacity and overcapacity.
    - To figure out where this border lies, first you must cross it.
        1. Add layers 
        2. Make the layers bigger
        3. Train for more epochs
7. Regularizing your model and tuning your hyperparameters

![6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%208.png](6%E1%84%80%E1%85%A1%E1%86%BC%20Fundamental%20of%20machine%20Learning%20e3549dc3955e4c2089bb79348361d4a9/Untitled%208.png)