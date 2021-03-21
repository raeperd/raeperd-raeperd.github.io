---
title: "NLP Basic"
date: 2020-03-20T22:21:46+09:00
tags : ["ai", "ml", "nlp"]
---


# INTRO 

 머신러닝을 계속 해보면서 느끼는 거지만 어떤 모델을 써서 어떻게 결과가 좋았다는 건 아주 대부분 경험적인 것에 의존할 수 밖에 없다. 초기에 어떤 모델로 학습을 할 것인가를 제외하면 Hyper parameter 에 따라 변화하는 정확도는 대부분 개발자의 손을 떠나게 된다. (정말 깊이있는 머신러닝 전문가가 한다면 다를 수 있는지는 잘 모르겠다. 그런 사람을 본적이 없어서..)



 그래서 전처리를 어떻게 하느냐가 더더욱 중요하게 느껴진다. 학습과정에는 사람이 개입할 수 없지만 전처리 과정에서는 사람이 온전히 관여할 수 있다. 데이터를 어떻게 처리하느냐에 따라 학습에 결과는 눈에 띄게 달라지는 경우가 많다. 머신러닝이 성공적이었던 분야는 크게 보면 이미지와 텍스트 두가지로 분류할 수 있다. 이 둘 중에 또 특히 텍스트의 경우 전처리 과정이 중요하다. 



 개인적으로 회사에서 함수, 그리고 매크로 문자열에 대해 머신러닝을 도입해 볼 수 있는 기회가 있었는데 두 분야 모두 꽤나 괜찮은 효과를 얻을 수 있었다. 추후에 또 텍스트로 된 데이터에 대해 머신러닝을 도입해 볼 수 있겠다는 생각이 들어 자연어 처리를 공부하면서 알게된 여러가지 내용을 용어를 중심으로 정리해보고자 한다. 용어만 정리 해두면 나중에 필요할 때 언제든지 가져다 쓸 수 있을 것 같다. 



# CONTENTS


![text-mining-intro](/images/text-mining-intro.jpeg)

## Text Mining

- 비정형 데이터에서의 텍스트 데이터의 패턴과 관계를 추출하여 의미있는 정보를 추출하는 마이닝 기법
- 비정형 자료의 수치형 자료로 변환하는 기법이 필요



### Text mining 기술

1. 정보추출
2. 정보검색
3. 분류
4. 클러스터링
5. 요약

- 분류와 클러스터링의 차이는 지도 학습이냐  비지도 학습이냐의 차이다. 
- 클러스터링은 중간에 데이터를 가공할때 사용할 수 있다.



### Text mining 응용분야

1. Risk management, Customer case service
2. Fraud detection
3. Business intelligence
4. Social media analysis
5. 기타 등등 ..   
  
  
### Text mining 용어 이해 - 기본구성

![text-data-hierarchy](/images/text-data-hierarchy.png)
  
- 말뭉치(corpus) > 문서(document) > 단락 > 문장 > 단어(words) > 형태소(morpheme)
  - 말뭉치: 분석을 목적으로 언어의 표본을 추출한 집합(문서의 집합).
  - 형태소(morpheme): 의미를 가지는 최소의 단위.
- words → (feature 추정) → document → (통계 분석) → corpus

1. `Tokens` : 의미를 나타내는 가장 작은 단위 (보통 단어 혹은 단어의 집합)
2. `Terms` : 단일 단어 또는 복수단어 단위로 표현됨
3. `Document` : 문장으로 구성됨
4. `Corpus` : 분석을 목적으로 언어의 표본을 추출한 집합(문서의 집합).
5. `Stopword`(불용어) 
   - 분석동안 제외하려는 단어
   - 예) a, an, the, to, of 등
   - 이걸 처리하는 것도 중요한 노하우임
   - 가지고 있으면 다른데 또 쓸 수 있음


### Text mining 용어 이해 - 전처리

1. `Sparse term `
   - 매우 적은 수의 문서에서만 나타나는 용어
   - `removeSparseTerms(dtm, 0.70)`
2. `Stemming `(어간 추출) 
   - 단어의 의미를 담고 단어의 핵심부분을 추출하는 것
   - example, examples -> exampl(어간추출결과)
3. `Tokenization` 
   - 단어, 문구, 키워드 등과 같은 구조적 데이터 토큰으로 분할하는 과정
4. `Polarity` 
   - 긍정/부정/중립의 여부를 식별
   - 감정 분석에 사용되는 용어
5. Part of Speech Tagging 
   - 문서의 모든 단어에 태그를 지정한 형태
   - 명사 동사 형용사 대명사 등



## 문서의 수치화와 BoW

- 비정형 데이터의 처리를 위해서는 수치형 자료 변환이 필요하다. 
- `Bag of words ` 가설 
  - 문서에서 특정 단어의 출현 빈도를 통해 질의와 문서의 관계를 파악할 수 있다.
  - Turney and Pantel, 2010
- `Bag` :  원소의 중복을 허용하는 중복집합
- `BoW` : 는 문서, 문장 표현 방식(DTM,TCM)



### 단일 표현과 복수 표현

- 단일 표현: `one-hot encoding`
- 복수 표현 : `word embedding`
  - 복수 뉴런의 작동으로 1개의 개념을 나타냄
  - 분포가설 
    - "비슷한 문맥을 가진 단어는 비슷한 의미를 가진다."
- `onehot encoding` 은 관계를 확인할 수는 없다. 관계를 파악하려면 `word embedding` 을 사용해야한다. 



### 분포가설 

- 비슷한 문맥을 가진 단어는 비슷한 의미를 가진다.
- Count-based methods 
  - SVD, HAL 등
  - 단어, 문맥 출현 횟수를 세는 방법
- Predictive methods 
  - NLPM, word2vec text2vec fasttext BERT
  - 단어에서 문맥 또는 문맥에서 단어를 예측하는 방법



### word2vec

- CBOW(연속 Bow 모델)
  - 문맥으로부터 단어를 예측
  - 소규모 데이터 셋에 대하여 성능이 좋음

- skip-gram 모델
  - 단어로부터 문맥을 예측
  - 대규모 데이터 셋에 사용됨
  - 성능이 우수, 속도도 빠름

  



## NLP(Natural Language Processing

- 컴퓨터가 사람의 말이나 텍스트를 이해하는 능력
- NLP 라이브러리 : NLTK, Stanford NLP, MALLET, Apache OpenNLP
- NLP 활용:  자동텍스트 요약, 품사태그 추가, 주제 추출, 감정 분석, 관계추출, 형태소 분석

  

### NLP 어휘분석

- part of speech
- named entitiy recognition
- co-reference
- basic dependenciies





### NLP vs Text Mining

Text mining 의 경우 보통 text의 구조 자체를 고려하지는 않으며, 비교적 간단한 분석에 사용한다.
반면 NLP의 경우 text의 구조도 고려하며 단어 단위가 아니라 문장 단위까지 text의 의미를 파악하려고 시도한다. 
NLP가 좀 더 고차원적인? 접근 방법이라고 이해하면 될 것 같다. [이 곳](https://discuss.analyticsvidhya.com/t/difference-between-nlp-and-text-mining/2977/2) 을 참조함.



# Tips

- 1만개 이내에서는 딥러닝 보다는 이전의 알고리즘(통계적 접근)이 좋다.
- 클러스터링은 중간에 데이터를 가공할때 사용할 수 있다.
- 텍스트 마이닝은 주로 지도학습을 사용한다.
- 자연어 처리의 시작은 태깅이다.
  - 자르고 분류한다.
  - 한글의 경우 spacy, konlpy, komoran, hannanum을 써볼만 함
- 유사 질문을 찾아서 바꾼다.
- 머신러닝과 통계 분석을 함께 이용할 수도 있다.
- python 버전 문제가 많다. 가상 환경을 만들어 두는 것이 좋다. 
- tf-idf 는 여전히 많이 쓰이고 활용이 많이 된다.
- 유사도는 어떤 걸 쓰든 거의 비슷해서 cosine 유사도면 충분했다.
- 파라미터는 얼추 알고있다가 차이가 큰 걸 발견하면 또 공부해야하지 어떻게해
- 1차적인 분석은 로지스틱으로 해도 충분하니, nlp를 먼저 쓰지 않는다. (2진분류 특히)
- 이전의 연구 결과를 참고 해야 한다.

  - 나랑 비슷한 모델을 참고한다.
  - 그런 모델이 없다면 아주 큰 확률로 내가 잘못된 기획을 한거다.



### 실습해볼만한 것들

- 로지스틱 회귀
- 워드투벡터
- bert



# REFERENCE

[딥러닝을 이용한 자연어 처리입문](https://wikidocs.net/book/2155)

[Text2Vec](http://text2vec.org/index.html)

[Bag Of Words 실습 (R)](https://statkclee.github.io/text/nlp-bag-of-words.html)

[Keras 창시자에게 배우는 딥러닝 소스](https://github.com/rickiepark/deep-learning-with-python-notebooks)

[Text Mining with R](https://www.tidytextmining.com/index.html)

[Google Bert](https://github.com/google-research/bert)

[딥러닝 기반 자연어처리 기법의 최근 연구 동향]([https://ratsgo.github.io/natural%20language%20processing/2017/08/16/deepNLP/])