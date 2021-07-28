---
title: "NLP Basic"
date: 2019-08-25
tags: ["machine-learning"]
cover:
   image: https://i.ibb.co/PtDJKFb/nlp.jpg
---

# Contents

## NLP vs Text Mining

![](https://i.imgur.com/tTURpgC.png)

## Text mining
- 비정형 데이터에서의 텍스트 데이터의 패턴과 관계를 추출하여 의미있는 정보를 추출하는 마이닝 기법
- 비정형 자료의 수치형 자료로 변환하는 기법이 필요  
 
### Text mining 기술
1. 정보추출
2. 정보검색
3. 분류
4. 클러스터링
5. 요약
- 분류와 클러스터링의 차이는 지도와 비지도 학습이다.
- 클러스터링은 중간에 데이터를 가공할때 사용할 수 있다.  
 
### Text mining 응용분야
1. Risk Management, Customer case service
2. Fraud Detection
3. 비즈니스 인텔리전스
4. 소셜미디어 분석  
 
### 텍스트 데이터의 구조
- 말뭉치(corpus) > 문서(document) > 단락 > 문장 > 단어(words) > 형태소 
  - 말뭉치: 분석을 목적으로 언어의 표본을 추출한 집합(문서의 집합).
  - 형태소(morpheme): 의미를 가지는 최소의 단위.
- words → (feature 추정) → document → (통계 분석) → corpus  
  
### Text mining 용어 이해 - 기본구성
- Tokens 
   - 의미를 나타내는 가장 작은 단위
- Terms 
   - 단일 단어 또는 복수단어 단위로 표현됨
- Document 
   - 문장으로 구성됨
- Corpus 
   - 문장들의 집합
- Stopword(불용어) 
   - 분석동안 제외하려는 단어
   - 예) a, an, the, to, of 등
   - 이걸 처리하는 것도 노하우임
   - 가지고 있으면 다른데 또 쓸 수 있음  
  
### Text mining 용어 이해 - 전처리
1. Sparse term 
   - 매우 적은 수의 문서에서만 나타나는 용어
   - `removeSparseTerms(dtm, 0.70)`
2. Stemming (어간 추출) 
   - 단어의 의미를 담고 단어의 핵심부분을 추출하는 것
   - example, examples -> exampl(어간추출결과)
3. Tokenization 
   - 단어, 문구, 키워드 등과 같은 구조적 데이터 토큰으로 분할하는 과정
4. Polarity 
   - 긍정/부정/중립의 여부를 식별
   - 감정 분석에 사용되는 용어
5. Part of Speech Tagging 
   - 문서의 모든 단어에 태그를 지정한 형태
   - 명사 동사 형용사 대명사 등  
 
## 문서의 수치화와 BoW
- 비정형 데이터의 처리를 위해서는 수치형 자료 변환이 필요
- Bag of words 가설 
  - 문서에서 특정 단어의 출현 빈도를 통해 질의와 문서의 관계를 파악할 수 있다.
  - Turney and Pantel, 2010
- Bag은 원소의 중복을 허용하는 중복집합, BoW는 문서, 문장 표현 방식(DTM,TCM)  
 
### 단일 표현과 복수 표현
- 단일 표현: one-hot encoding
- 복수 표현 (워드임베딩) 
  - 복수 뉴런의 작동으로 1개의 개념을 나타냄
  - 분포가설 
    - "비슷한 문맥을 가진 단어는 비슷한 의미를 가진다."
- onehot encoding 은 관계를 확인할 수는 없음 → word embedding  
 
## 분포가설
- 비슷한 문맥을 가진 단어는 비슷한 의미를 가진다.
- Count-based methods 
  - SVD, HAL 등
  - 단어, 문맥 출현 횟수를 세는 방법
- Predictive methods 
  - NLPM, word2vec text2vec fasttext BERT
  - 단어에서 문맥 또는 문맥에서 단어를 예측하는 방법  
 
## word2vec
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
 
## Tips
- 만개 이내에서는 딥러닝 보다는 이전의 알고리즘이 좋다.
- 클러스터링은 중간에 데이터를 가공할때 사용할 수 있다.
- 텍스트 마이닝은 주로 지도학습을 사용한다.
- 자연어 처리의 시작은 태깅이다.
  - 자르고 분류한다.
  - 한글의 경우 spacy konlpy komoran hannanum 을 써볼만 함
- 유사 질문을 찾아서 바꾼다.
- 머신러닝과 통계 분석을 함께 이용할 수도 있다.
- python 버전 문제가 많음
- tf-idf 는 여전히 많이 쓰이고 활용이 많이 된다.
- 유사도는 거의 비슷해서 cosine 유사도면 충분했다.
- 파라미터는 얼추 알고있다가 차이가 큰 걸 발견하면 또 공부해야하지 어떻게해
- 1차적인 분석은 로지스틱으로 해도 충분하니, nlp를 먼저 쓰지 않는다. (2진분류 특히)  
 
## case study 
- 나랑 비슷한 모델을 참고한다.
- 모델이 없다면 내가 잘못된 기획을 한거다!  
 
### 실습해볼만한 것들
- 로지스틱 회귀
- 워드투벡터
- bert  
 
# Reference
- [딥러닝을 이용한 자연어 처리입문](https://wikidocs.net/book/2155)
  - 온라인 ebook 내용이 NLTK 활용, 내용 Good!
- [Text2vec](http://text2vec.org/index.html) 
  - 개념 설명과 Sample 소스(R) 제공
  - 샘플 잘되있음
- [BoW 실습 (R)](https://statkclee.github.io/text/nlp-bag-of-words.html)
- [Keras 창시자에게 배우는 딥러닝 소스](https://github.com/rickiepark/deep-learning-with-python-notebooks)
  - R and python
- [Text Mining with (R)](https://www.tidytextmining.com/index.html)
- [구글 BERT](https://github.com/google-research/bert)
- 차원 축소

