# 8강 Deep Learning for Text and Sequence

# One-hot encoding of words and sequences

## Word-level one-hot encoding(toy example)

### Build an index of all tokens in the data

```python
import numpy as np

# This is our initial data; one entry per "sample"
# (in this toy example, a "sample" is just a sentence, but
# it could be an entire document).
samples = ['The cat sat on the mat.', 'The dog ate my homework.']

# First, build an index of all tokens in the data.
token_index = {}
for sample in samples:
	# We simply tokenize the samples via the split method.
	# In real life, we would also strip punctuation and special characters from the samples.
	sample = "".join([c for c in sample if c not in ('!', '?', '.', ',')])
	for word in sample.split():
		if word not in token_index:
		# Assign a unique index to each unique word
		token_index[word] = len(token_index) + 1
		# Note that we don't attribute index 0 to anything.

print(type(token_index))
print(token_index)
```

- index 0 은 비운다.

### Vectorize sequence (or whole doc)

```python
# Next, we vectorize our samples.
# We will only consider the first `max_length` words in each sample.
max_length = 10

# This is where we store our results:
results = np.zeros((len(samples), max_length, max(token_index.values()) + 1))
for i, sample in enumerate(samples):
	sample = "".join([c for c in sample if c not in ('!', '?', '.', ',')])
	for j, word in list(enumerate(sample.split()))[:max_length]:
		index = token_index.get(word)
		print(i, j, word, index)
		results[i, j, index] = 1.

print(results)
```

## Character-level one-hot encoding

```python
import string
samples = ['The cat sat on the mat.', 'The dog ate my homework.']
characters = string.printable
print(characters)
print(len(characters))
```

```python
token_index = dict(zip(characters, range(1, len(characters) + 1)))
print(token_index)
max_length = 50
results = np.zeros((len(samples), max_length, max(token_index.values()) + 1))
for i, sample in enumerate(samples):
	for j, character in enumerate(sample):
		index = token_index.get(character)
		#print(i, j, character, index)
		results[i, j, index] = 1.

print(results)
print(results[0][0])
```

## Using Keras for word-level one-hot encoding

```python
from keras.preprocessing.text import Tokenizer

samples = ['The cat sat on the mat.', 'The dog ate my homework.']
# We create a tokenizer, configured to only take into account the top-1000 most common words
tokenizer = Tokenizer(num_words=100)
# This builds the word index
tokenizer.fit_on_texts(samples)

# This turns strings into lists of integer indices.
sequences = tokenizer.texts_to_sequences(samples)
print(sequences)
```

```python
# You could also directly get the one-hot binary representations.
# Note that other vectorization modes than one-hot encoding are supported

one_hot_results = tokenizer.text_to_matrix(samples, mode='count')
print(one_hot_results)

# This is how you can recover the word index that was computed
word_index = tonkenizer.word_index
print("Found $s unique tokens." %len(word_index))
print(word_index
```

```
[[0. 2. 1. 1. 1. 1. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.
  0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.
  0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.
  0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.
  0. 0. 0. 0.]
 [0. 1. 0. 0. 0. 0. 1. 1. 1. 1. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.
  0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.
  0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.
  0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.
  0. 0. 0. 0.]]
Found 9 unique tokens.
{'ate': 7, 'homework': 9, 'sat': 3, 'the': 1, 'my': 8, 'cat': 2, 'dog': 6, 'mat': 5, 'on': 4}
```

# Using word embeddings

## 비교

- One-hot encoding: binary, sparse, very high dimensional
- Word embedding  : not binary, dense, low-dimensional

## Word embeddings

- Learn word embeddings jointly with the main task you care about.
- Start with random word vectors
- 단어 간 의미 관계를 반영한 기하학적 공간으로 단어를 매핑
- 동의어는 비슷한 단어 벡터로 임베딩
- 두 단어의 벡터간 거리 = 단어 간 의미적 거리

## Word2Vec 생성

### Window Size = 1

- "king brave man"
- "Queen beautiful woman"

![8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled.png](8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled.png)

### Window Size = 2

![8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%201.png](8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%201.png)

![8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%202.png](8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%202.png)

### Word2Vec 을  생성하는 Deep Learning 구조

![8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%203.png](8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%203.png)

![8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%204.png](8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%204.png)

![8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%205.png](8%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Text%20and%20Sequence%20b5e5f1361f3a4b77ae3ba44a0c0a5510/Untitled%205.png)

### Keras code using word embeddings

```python
import keras
from keras.layers import Embedding

# The Embedding layer takes at least two arguments:
# the number of possible tokens, here 1000 (1 + maximum word index)
# and the dimensionality of the embeddings, here 64

embedding_layer = Embedding(1000,64)
```

```python
from keras.datasets import imdb 
from keras import preprocessing

max_features = 10000 
maxlen = 20 

(x_train, y_train), (x_test, y_test) = imdb.load_data(num_words=max_features) 

x_train = preprocessing.sequence.pad_sequences(x_train, maxlen = maxlen)
x_test  = preprocessing.sequence.pad_sequences(x_test, maxlen = maxlen)
```

- `max_feature` : 상위 10000 개 단어를 feature로 선정
- `max_len` : 각 문서의 앞 20개 단어만을 조회

```python
from keras.models import Sequential 
from keras.layers import Flatten, Dense 

model = Sequential()
model.add(Embeddig(10000, 8, input_length = maxlen))

model.add(Flatten())

model.add(Dense(1, activation='sigmoid'))
model.compile(optimizer='rmsprop', loss='binary_crossentropy', metics=['acc'])
model.summary() 

history = model.fit(x_train, y_train, 
										epochs = 10,
										batch_size = 32,
										validation_split = 0.2)

```

```
Layer (type)                 Output Shape              Param #   
=================================================================
embedding_2 (Embedding)      (None, 20, 8)             80000     
_________________________________________________________________
flatten_1 (Flatten)          (None, 160)               0         
_________________________________________________________________
dense_1 (Dense)              (None, 1)                 161       
=================================================================
Total params: 80,161
Trainable params: 80,161
Non-trainable params: 0
```

- We will restrict the movie reviews to the **top 10,000 most common words**, and cut the reviews after **only 20 words**.
- Our network will simply learn **8-dimensional embeddings for each of the 10,000 words**,
turn the input integer sequences (2D integer tensor) into embedded sequences (3D float tensor),
- flatten the tensor to 2D, and train a single Dense layer on top for classification.
- `Embedding( num_of_features, dim_of_embeddings, num_of_input_per_doc)`
    - dim_of_embeddings * num_of_input_per_doc 만큼의 flatten 노드 생성