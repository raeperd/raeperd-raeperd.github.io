---
title: "Probability"
date: 2018-03-16T19:58:54+09:00
tags: ["math"]
draft: true
---

# 확률 (Probability)

------

  ![](C:\Users\qkrfo\Documents\Blog Posting\localimage\dice.jpeg)

   

 수학 전공을 4년째 하고 있음에도 불구하고 확률이 뭐냐? 확률변수가 뭐냐는 질문에 명확하게 대답하지 못했던 제 자신이 한심해서 쓰게된 글입니다. 이 글을 보고 있는 여러분이 한심하다는 소리가 아닙니다. 하지만 제가 이 글을 다시 보고 있다면 그건 한심한 겁니다 .. 

​    

**표본공간(Sample Space) :** 어떤 시행에서 일어날 수 있는 모든 결과들의 모임

**사건공간(Event Space) :** 표본공간의 부분집합으로 실험으로 일어날 수 있는 결과의 집합

  

- 대부분의 상황에서 event space를 sample space의 부분집합으로 정의하는 것이 잘 작동하지만, sample space의 크기가 무한한 경우 실험을 통해 얻은 결과가 예측과 벗어나거나 (badly-behaved), 의미있는 수치를 부여할 수 없는 사건(non-measurable set)에 대해 정의할 때 문제가 생길 수 있음.

  

  

## 확률

  

### axiom1.

> event space의 원소 E에 대해, $0 \le P[E]\le 1$

  

### axiom2.

> sample space S에 대해, $P[S] = 1$

  

### **axiom3.**

> 배반 사건 ${E}_{i}$ $({i}=1,2, …)$ 에 대해,    $P[\bigcup _{ i=1 }^{ \infty  }{ { E }_{ i } } ]= \sum _{ i=1 }^{ \infty  } P[{ E }_{ i }]$



   

 1,2의 경우 당연히 공리가 되어야 한다고 생각했지만 3번 공리의 경우 조금 의외였다. 정리로 알고 있었는데 이게 공리였다니.. 이 세가지 공리를 이용해 한가지 중요한 정리를 증명할 수 있는데, 이 정도는 알야둬야지 싶다. 

​    

​    

### Addition law of property

​    

$P[A\bigcup B] =  P[A] + P[B] - P[A\cap B]$

​    

​    

proof) 

​    

By axiom3, 

$P[A\bigcup B] = P[A] + P[B-A] \\ \quad \quad \quad \quad = P[A] + P[B-A\cap B] $

  

Same way, for P[B]

​     $P[B] = P[B-A\cap B] + P[A\cap B] \\ \Rightarrow P[B-A\cap B] = P[B] - P[A\cap B]$

​    

Applying this to *

$P[A\bigcup B] = P[A] + P[B-A\cap B] \\ \quad \quad \quad \quad =P[A] + (P[B] - P[A\cap B]) \\ \quad \quad \quad \quad = P[A] + P[B] - P[A\cap B]$

​    

  

 사실 이 Addition law가 공리여도 큰 문제가 없긴 하다. 반대로 Addtion law가 성립하면 axiom3. 이 성립하는 것은 쉽게 확인할 수 있다. 

​    

​    

​    

### 확률변수

​    

확률을 다루다보면 항상 나오는 또하나의 단어가 확률변수인데 확률변수는 이런 친구다.

  

**확률변수(Random Variable) : ** 표본공간을 실수로 공간으로 대응시키는 함수

  

 확률 이라는 함수가 표본공간에서 실수로 대응되는 함수이기 때문에 표본공간과 확률 공리만 정의한 것으로 충분해 보이지만 실은 그렇지 않다. 많은 경우에 우리는 확률의 정의역을 실수 처럼 쓰고 싶다. 이를테면 많이 사용하는 정규분포의 정의역은 보통 실수이지 사건공간이 아니다. 곧 우리가 자주 쓰는 아래와 같은 notation은 사실 합성함수의 또다른 표현이였던 거임!

######     

$P[X=1] = \frac {1}{2} \Longleftrightarrow  P \circ X^{-1} = \frac {1}{2}$

​    

 그래서, 조금 더 엄밀하게는 ==확률의 분포가 정규분포를 따른다는 표현보다는 확률변수의 분포가 정규분포를 따른다는 표현이 더 옳바른 표현이다.==

​    

  

  

