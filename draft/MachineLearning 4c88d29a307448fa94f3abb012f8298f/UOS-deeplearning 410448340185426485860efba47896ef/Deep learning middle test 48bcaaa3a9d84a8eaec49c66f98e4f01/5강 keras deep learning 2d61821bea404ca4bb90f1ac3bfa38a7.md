# 5강 keras deep learning

[5강. Keras Deep Learning-1부.pdf](5%E1%84%80%E1%85%A1%E1%86%BC%20keras%20deep%20learning%202d61821bea404ca4bb90f1ac3bfa38a7/5._Keras_Deep_Learning-1.pdf)

### Sentiment Analysis

```python
# from keras.datasets import imdb
from keras.datasets import imdb

(train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words=10000)
print(train_data.shape)
print(test_data.shape)
```

- 빈도수가 높은 것 1만개를 선택

```python
word_index = imdb.get_word_index()

reverse_word_index = dict( [ (value, key) for (key, value) in word_index.items()])
    
#for i in range(1000, 1200):
#    print (i, reverse_word_index.get(i))

decoded_review = ' '.join([reverse_word_index.get(i - 3, '?') for i in train_data[0]])
```

- -3 을 하는 이유는 숫자의 0,1,2 가 각각 "padding" "start of sequence" "unknown" 으로 예약되어 있기 때문
- 모르는 단어는 ?로 표시

### one-hot encoding

```python
import numpy as np
def vectorize_sequences(sequences, dimension=10000):
    results = np.zeros((len(sequences), dimension)) # shape가 (25000, 10000)인 results 객체 생성, 초기값은 모두 0
    for i, sequence in enumerate(sequences):
        results[i, sequence] = 1.      # i번째 데이터에 대하여, 해당 단어에 대한 index가 1, 14, 22, ... 이면 =>  (i, 1), (i, 14), (i, 22) 셀의 값이 1로 셋팅됨 
    return results

x_train = vectorize_sequences(train_data)
x_test = vectorize_sequences(test_data)
```

- 입력과 출력은 다시 one-hot 데이터로 사용해야 한다.

### set model

```python
from keras import models
from keras import layers
model = models.Sequential()
model.add(layers.Dense(16, activation='relu', input_shape=(10000,)))
model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))

model.summary()
```

> _________________________________________________________________
Layer (type) Output Shape Param #
=================================================================
dense_35 (Dense) (None, 16) 160016
_________________________________________________________________
dense_36 (Dense) (None, 16) 272
_________________________________________________________________
dense_37 (Dense) (None, 1) 17
=================================================================
Total params: 160,305
Trainable params: 160,305
Non-trainable params: 0
_________________________________________________________________

- output 이 확률이기를 원하기 때문에 activation 은 sigmoid로 설정된다.
- input shape = (10000, ) : input의 양은 모름, 입력 벡터의 차원은 one-hot이기 때문에 고정
- # of parameters (as scalars) = 10000 * 16(units) + 16 (bias)

### training

```python
model.compile(optimizer='rmsprop',
	            loss='binary_crossentropy',
              metrics=['acc'])
clf = model.fit(partial_x_train, partial_y_train, epochs=10, batch_size=512,  validation_data=(x_val, y_val))
```

### evaluate

```python
results = model.evaluate(x_test, y_test)
print("loss:", results[0], "- accuracy:", results[1])
```

## overfitting 현상 포착

![5%E1%84%80%E1%85%A1%E1%86%BC%20keras%20deep%20learning%202d61821bea404ca4bb90f1ac3bfa38a7/Untitled.png](5%E1%84%80%E1%85%A1%E1%86%BC%20keras%20deep%20learning%202d61821bea404ca4bb90f1ac3bfa38a7/Untitled.png)

- underfitting and overfitting
- 충분한 epoch 이후에서야 알 수 있다.

```python
x_val = x_train[:10000]                  # validation 데이터는 1만개
y_val = y_train[:10000] 
partial_x_train = x_train[10000:]        # train 데이터는 나머지 1만 5천개
partial_y_train = y_train[10000:]

model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=['acc'])

history = model.fit(partial_x_train, 
                    partial_y_train, 
                    epochs=10, 
                    batch_size=512,  
                    validation_data=(x_val, y_val))
```

### 학습 진행과정의 시각화

```python
import matplotlib.pyplot as plt
histroy_dict = history.history

loss_values = history_dict['loss']
val_loss_values = history_dict['val_loss']
epochs = range(1, len(history_dict['loss']) + 1)
plt.plot(epochs, loss_values, 'bo', label='Training loss')             # training loss는 '원'으로 표시
plt.plot(epochs, val_loss_values, 'b', label='Validation loss')    # validation loss는 '선'으로 표시
plt.title('Training and validation loss')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.legend()

plt.show()
```

- `bo` blue with o shape

![5%E1%84%80%E1%85%A1%E1%86%BC%20keras%20deep%20learning%202d61821bea404ca4bb90f1ac3bfa38a7/Untitled%201.png](5%E1%84%80%E1%85%A1%E1%86%BC%20keras%20deep%20learning%202d61821bea404ca4bb90f1ac3bfa38a7/Untitled%201.png)

- epoch 값을 4로 해서 다시 training 하는 것이 좋겠군

```python
model = models.Sequential()
model.add(layers.Dense(16, activation='relu', input_shape=(10000,)))
model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))
model.compile(optimizer='rmsprop',
loss='binary_crossentropy',
metrics=['accuracy'])
model.fit(x_train, y_train, epochs=4, batch_size=512)
results = model.evaluate(x_test, y_test)
```

## Multi-class Classification for Newswires

```python
from keras.datasets import reuters
(train_data, train_labels), (test_data, test_labels) = reuters.load_data(num_words=10000)
```

- 바이너리 클래스를 구분하는 코드를 멀티 클래스 구분 문제로 바꿀려면 코드의 어떤 부분이 변화되어야 하는가?

    = output layer? 

### 데이터 가공: 출력데이터

```python
def to_one_hot(labels, dimension=46):
    results = np.zeros((len(labels), dimension))
    for i, label in enumerate(labels):
        results[i, label] = 1.
    return results

one_hot_train_labels = to_one_hot(train_labels)
one_hot_test_labels = to_one_hot(test_labels)
```

```python
# 이미 Keras에서는  one-hot encoding 함수를 지원하고 있음
from keras.utils.np_utils import to_categorical
one_hot_train_labels = to_categorical(train_labels)
one_hot_test_labels = to_categorical(test_labels)
```

- same with keras function

```python
from keras import models
from keras import layers
model = models.Sequential()
model.add(layers.Dense(64, activation='relu', input_shape=(10000,))) # Input layer의 노드 개수는 입력 벡터의 성분 개수와 일치해야 함
model.add(layers.Dense(64, activation='relu'))
model.add(layers.Dense(46, activation='softmax'))   # Output layer의 노드 개수는 46개: class 개수와 일치해야 함
# output[i] is the probability that the sample belongs to class i.

model.compile(optimizer='rmsprop',
                loss='categorical_crossentropy', # measures the distance between two probability distributions
                metrics=['accuracy'])

x_val = x_train[:1000]
partial_x_train = x_train[1000:]
y_val = one_hot_train_labels[:1000]
partial_y_train = one_hot_train_labels[1000:]
```

### training

```python
history = model.fit(partial_x_train, 
										partial_y_train,
									  epochs=20,
									  batch_size=512,
										validation_data=(x_val, y_val))
```

overfitting catch

```python
import matplotlib.pyplot as plt
loss = history.history['loss']
val_loss = history.history['val_loss']
epochs = range(1, len(loss) + 1)
plt.plot(epochs, loss, 'bo', label='Training loss')
plt.plot(epochs, val_loss, 'b', label='Validation loss')
plt.title('Training and validation loss')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.legend()
plt.show()
```

![5%E1%84%80%E1%85%A1%E1%86%BC%20keras%20deep%20learning%202d61821bea404ca4bb90f1ac3bfa38a7/Untitled%202.png](5%E1%84%80%E1%85%A1%E1%86%BC%20keras%20deep%20learning%202d61821bea404ca4bb90f1ac3bfa38a7/Untitled%202.png)

```python
plt.clf()
acc = history.history['acc']
val_acc = history.history['val_acc']
plt.plot(epochs, acc, 'bo', label='Training acc')
plt.plot(epochs, val_acc, 'b', label='Validation acc')
plt.title('Training and validation accuracy')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.legend()
plt.show()
```

![5%E1%84%80%E1%85%A1%E1%86%BC%20keras%20deep%20learning%202d61821bea404ca4bb90f1ac3bfa38a7/Untitled%203.png](5%E1%84%80%E1%85%A1%E1%86%BC%20keras%20deep%20learning%202d61821bea404ca4bb90f1ac3bfa38a7/Untitled%203.png)

- train data로 training, validation data 로 검증, test data로 최종 확인
- 검증과 테스트라는 이름에는 왔다갔다 하는 경향이 있음
- 테스트 데이터가 사실 미래의 데이터를 대표하는 데이터임