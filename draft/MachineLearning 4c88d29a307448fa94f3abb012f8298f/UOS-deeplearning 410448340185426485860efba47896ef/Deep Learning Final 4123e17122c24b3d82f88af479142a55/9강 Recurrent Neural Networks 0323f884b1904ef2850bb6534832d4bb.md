# 9강 Recurrent Neural Networks

# Intro

- 우리는 하나의 단어만을  이해하지 않는다.
- 이전의 단어들을 기반으로 다음 단어를 이해한다.
- DNN / CNN 으로는 이러한 일을 할수 없다.

# RNN Applications

- **Document classification and timeseries classification:**
    - such as identifying the topic of an article or the author of a book
- **Timeseries comparisons:**
    - such as estimating how closely related two documents or two stock tickers are
- **Sequence-to-sequence learning:**
    - such as decoding an English sentence into French
- **Sentiment analysis:**
    - such as classifying the sentiment of tweets or movie reviews as positive or negative
- **Timeseries forecasting:**
    - such as predicting the future weather at a certain location, given recent weather data

# Recurrent Neural Network

![9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled.png](9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled.png)

- the same function and the same set of parameters are used at every time step. (모든 상태에서 W값은 동일하다)

![9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%201.png](9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%201.png)

- 3종류의 weight가 정의된다. Whh Wxh Why
- 각 단계별 weight는 동일하다.

## Character-level language model example

![9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%202.png](9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%202.png)

![9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%203.png](9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%203.png)

## RNN의 다양한 구조

![9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%204.png](9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%204.png)

### multi-layer RNN

![9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%205.png](9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%205.png)

# Recurrent Layers in Keras

## SimpleRNN

```python
from keras.models import Sequential
from keras.layers import Embedding.SimpleRNN

model = Sequential()
model.add(Embedding(10000, 32))
model.add(SimpleRNN(32))
model.summary()
```

- `Embedding(input_dim, output_dim)`
- `SimpleRNN(num_of_hiddenlayer_feature)`
    - or num_of_hidden_layer_node

```
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
embedding_2 (Embedding)      (None, None, 32)          320000    
_________________________________________________________________
simple_rnn_2 (SimpleRNN)     (None, 32)                2080      
=================================================================
Total params: 322,080
Trainable params: 322,080
Non-trainable params: 0
_________________________________________________________________
```

- 2080 = Wxh + Whh + b = 32*32 + 32*32 + 32 = 2080

### return_sequence = True or False

```python
model = Sequential()
model.add(Embedding(10000, 32))
model.add(SimpleRNN(32, return_sequence = True))
model.summary() 
```

- return_sequence가 True 이면 중간 출력값을 모두 사용한다.
- 따라서, RNN을 stacking 할 때, 중간의 return_sequence 는 모두 True 여야 할 것 !

![9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%206.png](9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%206.png)

```python
model = Sequential()
model.add(Embedding(10000, 32))
model.add(SimpleRNN(32, return_sequence = True)
model.add(SimpleRNN(32, return_sequence = True)
model.add(SimpleRNN(32, return_sequence = True)
model.add(SimpleRNN(32, return_sequence = True)
model.add(SimpleRNN(32))
model.summary()
```

## Training RNNs is challenging

- 기본 RNN 은 복잡해지면 학습에 어려움
- 대부분 다음과 같은 advanced model 들을 사용함
    - Long Short Term Memory (LSTM)
    - GRU

# LSTM

![9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%207.png](9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%207.png)

- 무엇을 망각하고 무엇을 기억할지에 대한 weight의 값을 추가로 학습하는 모델
- Cell state 덕분에 State가 꽤 오래 경과하더라도 gradient가 잘 전파됨

![9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%208.png](9%E1%84%80%E1%85%A1%E1%86%BC%20Recurrent%20Neural%20Networks%200323f884b1904ef2850bb6534832d4bb/Untitled%208.png)

- forget gate ft: 과거 정보를 잊기 위한 게이트
- Input gate it: 현재 정보를 기억하기 위한 게이트

## Simple RNN with LSTM

```python
model = Sequential()
model.add(LSTM((1), batch_input_shape(None, 5, 1), return_sequence = True ))
model.add(LSTM(1), return_sequence = False)
model.summary()
```

- batch_input_shape(num_data, len_seq, num_feature)

# Forecastring Temperature