# 10강 Advanced deep learning best practice

# Beyond Sequential model

![10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled.png](10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled.png)

![10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%201.png](10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%201.png)

![10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%202.png](10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%202.png)

![10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%203.png](10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%203.png)

![10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%204.png](10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%204.png)

# Functional API

```python
input_tensor = input(shape=(64,))
x = layers.Dense(32, activation = 'relu')(input_tensor)
x = layers.Dense(32, activation = 'relu')(x)
output_tensor = layers.Dense(10, activation = 'softmax')(x)
model = Model(input_tensor, output_tensor)

model.summary()
model.compile(optimizer = 'rmsprop', loss='cartegorical_crossentropy')
model.fit(x_train, y_train, epochs=10, batch_size = 128)

```

# Model Input

```python
from keras.models import Model
from keras import layers
from keras import Input

test_vocabulary_size = 10000
question_vocabulary_size = 10000
answer_vocabularty_size = 500

text_input = Input(shape = (None,), dtype = 'int32', name = 'text')
embedded_text = layers.Embedding(64, test_vocabulary_size)(text_input)
encoded_text = layers.LSTM(32)(embedded_text)

question_input = Input(shape =(None, ), dtype = 'int32', name = 'question')
embedded_question = layers.Embedding(32, question_vocabulary_size,)(question_input)
encoded_question = layers.LSTM(16)(embedded_question)

concatenated = layers.concatenate([encoded_text, encoded_question],axis=1)
answer = layers.Dense(answer_vocabularty_size, activation='softmax')(concatenated)
model = Model([text_input, question_input],answer)
model.summary()
```

![10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%205.png](10%E1%84%80%E1%85%A1%E1%86%BC%20Advanced%20deep%20learning%20best%20practice%20750963db9c1b458a9343e9ea08deca25/Untitled%205.png)

```
__________________________________________________________________________________________________
Layer (type)                    Output Shape         Param #     Connected to                     
==================================================================================================
text (InputLayer)               (None, None)         0                                            
__________________________________________________________________________________________________
question (InputLayer)           (None, None)         0                                            
__________________________________________________________________________________________________
embedding_1 (Embedding)         (None, None, 10000)  640000      text[0][0]                       
__________________________________________________________________________________________________
embedding_2 (Embedding)         (None, None, 10000)  320000      question[0][0]                   
__________________________________________________________________________________________________
lstm_1 (LSTM)                   (None, 32)           1284224     embedding_1[0][0]                
__________________________________________________________________________________________________
lstm_2 (LSTM)                   (None, 16)           641088      embedding_2[0][0]                
__________________________________________________________________________________________________
concatenate_1 (Concatenate)     (None, 48)           0           lstm_1[0][0]                     
                                                                 lstm_2[0][0]                     
__________________________________________________________________________________________________
dense_1 (Dense)                 (None, 500)          24500       concatenate_1[0][0]              
==================================================================================================
Total params: 2,909,812
Trainable params: 2,909,812
Non-trainable params: 0
__________________________________________________________________________________________________
```