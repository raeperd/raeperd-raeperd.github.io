---
title: "Decision tree"
date: 2019-10-25
tags: ["machine-learning", "algorithm"]
---

## INTRO

 데이터를 다른 그룹으로 분류하는 문제를 풀 때, 가장 기본적으로 생각할 수 있는 방법은 여러개의 if~ else~ 문을 반복하는 것입니다. 날씨가 좋고, 평일이 아니면 놀기 좋은날이다! 와 같이 생각할 수 있습니다. 하지만 충분히 많은 데이터를 올바르게 분류하는 if~ else~ 코드는 직접 작성하기에 어려운 부분이 많이 있습니다. 



 Decision tree는 이런 if~ else~ 문을 자동으로 만들어 내는 알고리즘이라고 할 수 있습니다. 예를 들어 아래와 같은 데이터가 있을때 어떤 조건이 심장병을 성공적으로 분류 할 수 있는지 알아보겠습니다. 



| Chest Pain | Good Blood Circulation | Blocked Arteries | Heart Disease |
| ---------- | ---------------------- | ---------------- | ------------- |
| NO         | NO                     | NO               | NO            |
| YES        | YES                    | YES              | YES           |
| YES        | YES                    | NO               | NO            |
| YES        | NO                     | NO               | YES           |
| etc..      | etc..                  | etc..            | etc..         |



최종적으로 아래와 같은 Tree를 만드는 것이 목표입니다. Tree 의 leaf 들을 YES / NO 의 숫자를 대변합니다. 

![1569549456005](https://i.ibb.co/zHC60Zy/1569549456005.png)

문제는 이런 트리를 만드는 과정이 생각보다 어렵다는 것입니다. 왜 Wind 나 Humidity 를 기준으로 먼저 분류를 하는 것이 아니라 Weather를 먼저 사용했는지 등의 문제가 있습니다. 이에 대한 자세한 설명은 [영상](https://www.youtube.com/user/joshstarmer/search?query=Decision+Tree)을 통해 확인할 수 있습니다.  



## Algorithm

알고리즘을 간단히 요약하면 아래와 같습니다.

1. 변수(Column) 하나를 가지고 각각 이진 트리를 만든다. 
2. Gini index, entropy 와 같은 지표를 활용해 어떤 변수가 가장 데이터를 잘 분류했는지 평가한다. (에제는 Gini index를 사용)
3. 트리의 leaf 에 대해 반복하다가 지표가 최적이 되는 시점에 종료한다. 



### 1. 변수(Column) 하나를 가지고 각각 이진 트리를 만든다.

![1569487506732](https://i.ibb.co/gV6PcYL/1569487506732.png)

- 우선 하나의 변수만을 가지고 이진 분류를 해본다.
- 각각의 변수에 대해 모두 이진 분류를 한 트리를 만든다.



![1569487637982](https://i.ibb.co/LRjvgfv/1569487637982.png)

- 결측값(missing value)이 있을 수 있기 때문에 각각의 합계는 다를 수 있다. 

- 어떠한 노드도 100% 의 결과를 가지는 leaf 가 없기 때문에 각각의 leaf 는 impure 하다. 

- 어떤 분류가 가장 좋은지 판단하기 위해 impurity 를 측정할 만한 척도가 필요하다. 

  -> **Gini index** ! 

  

​	

### 2. Gini index, entropy 와 같은 지표를 활용해 어떤 변수가 가장 데이터를 잘 분류했는지 평가한다.



#### Gini index

- 낮을 수록 좋다. 
- 계산하기 쉽다. 



먼저 Chest Pain에 대한 Gini index 를 계산해보면 아래와 같다. 

![1569485863162](https://i.ibb.co/yq13FPc/1569485863162.png)


$$
\begin{align}
	\text{For left leaf, the Gini impurity} 
	&= 1 - (\text{the probability of "yes"})^2 - (\text{the probability of "no"})^2\\
	&= 1 - (\frac{105}{105+39})^2 - (\frac{105}{105+39})^2						   \\
	&= 0.395
\end{align}
$$

$$
\begin{align} 
	\text{For right leaf, the Gini impurity}
	&= 1 - (\text{the probability of "yes"})^2 - (\text{the probability of "no"})^2 \\
	&= 1 - (\frac{34}{34+125})^2 - (\frac{125}{34+125})^2						    \\
	&= 0.336
\end{align}
$$

 	

​	

### 3. 트리의 leaf 에 대해 반복하다가 지표가 최적이 되는 시점에 종료한다.

다음으로 트리 전체의 Gini index 를 계산한다. 


- 각 leaf 의 크기가 다르기 때문에 전체의 gini index 는 gini index의 weighted average가 되어야 한다. 


$$
\begin{align} 
	\text{Gini impurity for Chest Pain} &= \text{weighted average of Gini impurities for the leaf node}\\
	&= (\frac{144}{144+159}) \times 0.395 + (\frac{159}{144+159}) \times 0.336 \\
	&= 0.364
\end{align}
$$


- 각각의 leaf 에 대해 동일한 과정을 반복한다. 

- 분리한 뒤의 gini index 와 분리하기 전의 gini index 를 비교해 알고리즘을 끝낼 것인지 판단한다. 





### 응용

#### numeric data

1. Sort by numeric data 

2. 사이사이에서 평균을 구한다. (아래의 그림 참고)

3. 각각의 평균에서의 impurity value 를 계산한다. 

4. 최선의 impurity value 에서 분기를 친다. 

    

![1569488629706](https://i.ibb.co/2nptTwT/1569488629706.png)





#### ranked data 

- value 를 less equal 기준으로 분류한다. 



#### multi class 

- choice A , choice B, choice C, choice A or B, choice B or C, choice C or A 를 기준으로 분류한다. 





## 프로젝트에서 사용할 수 있는 방안

### 장점

- 비교적 직관적이고 단순한 알고리즘이다.
- 다른 모델과 달리 샘플의 분류 기준을 명확하게 알 수 있다.

### 단점

- 실제로 Decision Tree 단독으로는 그렇게 좋은 성능을 내주지 못한다.
- 데이터 셋 자체를 분류하는 것에는 훌륭하지만 새로운 샘플을 잘 분류해주지는 못한다.
  - 이를 **Random Forest** 를 이용해 해결할 수 있고, 실제 사례도 있다.



자세한 내용은 아래 참고 사이트를 보시면 추가 설명을 확인할 수 있습니다. 특히 전체적인 이해는 [영상](https://www.youtube.com/user/joshstarmer/search?query=Decision+Tree)을 통해 확인해보시는 것을 추천드립니다. 

# 참고

[Using Gini-index for Feature Weighting in Text Categorization](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.353.3216&rep=rep1&type=pdf)

