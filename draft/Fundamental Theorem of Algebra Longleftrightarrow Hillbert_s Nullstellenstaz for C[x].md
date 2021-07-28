---
title: "Fundamental Theorem of Algebra"
date: 2018-03-16T19:58:54+09:00
tags: ["math"]
draft: true
---

Fundamental Theorem of Algebra $\Longleftrightarrow$ Hillbert's Nullstellenstaz for $\C[x] $

  

 대수학 과제로 $\C[x]$ 에서의 힐버르트 영점정리(Hillbert's Nullstellenstaz)와 대수학의 기본정리(Fundamental Theorem of Algebra)가 동치임을 보이는 문제가 나왔는데 나름 오래 고민해 봤으나 제 시간에 풀어내지는 못했습니다. 

  

 프렐라이 책의 27장 33번 문제인데 유명할 것 같은 문제임에도 불구하고 구글링으로 힌트만 좀 얻으려 했지만 잘 안나오더군요. 결국 제출 기간이 다되서 솔루션을 확인해 봤는데 생각보다 간단한 증명이었고 영점정리를 조금만 잘 이해하고 있었으면 풀 수 있었겠구나 싶은 증명이었습니다. 구글링해도 잘 안나오는 증명이고 다시 복습하는 겸 이를 정리해봅니다. 

  

 먼저 Nullstellenstaz for $\C[x] $ 가 어떤건지에서 부터 시작해보겠습니다. 

​    	

#### Hillbert's Nullstellenstaz for $\C[x] $

 

>  $f_{1}\left( x \right),f_{2}\left( x \right),...,f_{r}\left( x \right) \in \C[x]$ 에 대해 이 모든 r 개의 다항식의 해인 $\alpha$에 대해 $g(x)\in \C[x] $ 또한 이 $\alpha$를 해로 가진다고 가정하면, $g(x)$의 어떤 지수승은 $f_{1}\left( x \right),f_{2}\left( x \right),...,f_{r}\left( x \right)​$ 를 포함하는 가장 작은 ideal의 원소이다.  

​    	

 프렐라이 책에 있는 영어로 된 설명을 잘못 이해하면서 쓸대없이 긴 고민을 시작했는데 가정에서의 $\alpha$는  $\alpha_{1}, \alpha_{2} ...\alpha_{r} $ 과 같은 값이 아니라 하나의 $\alpha$ 가 저 모든 다항식의 해가 되는 것이 가정입니다. 저 $\alpha$ 는 있을 수도 없을 수도 있는 해이고, 여러개 있을 수도 있는 값입니다.

​    

 원래의 영점정리는 이보다 더 확장된 내용의 정리이고 문제의 영점정리는 원래의 정리에서 대수적으로 닫힌 field가 $\C$ 인 특수한 경우를 말합니다.  한글 설명이 참 와닿지가 않아서 영어 위키피디아를 참고해봤는데 기하학과 대수학을 잇는 아주 중요한 정리라고 합니다. polynomial ring의 ideal에 관한 정리인데 이게 어떻게 기하학과 연관이 있는지는 도무지 이해가 잘 안되네요. 이 부분에 대해서는 더 공부를 해보고 알게 된다면 정리해보겠습니다.  

​    

 자세한 설명은 이곳에 있습니다. 한글 위키에도 정리된게 있긴 하지만 저는 수업을 영어로 해서 그런지 영문 위키쪽이 읽기 좋았습니다. 

  

영문 위키: https://en.wikipedia.org/wiki/Hilbert%27s_Nullstellensatz

한글 위키: https://ko.wikipedia.org/wiki/%ED%9E%90%EB%B2%A0%EB%A5%B4%ED%8A%B8_%EC%98%81%EC%A0%90_%EC%A0%95%EB%A6%AC

​    

#### Fundamental Theorem of Algebra

>  상수 다항식을 제외한 모든 복소계수 다항식은 근을 가진다. 

​    

 3학년때 복소변수함수론에서 해석적인 복소변수 함수의 성질과 리우빌 정리를 이용해 증명할 수 있음을 봤는데 깔끔한 증명이지만 복소해석학을 알아야하고 증명이 대수적인 성질을 사용했다는 느낌은 없었습니다. 교수님께서   대수학2까지 끝내고 나면 이를 대수학으로만 증명해보는게 좋은 공부가 될 것이라는 말씀을 하셨는데  이 증명이 그것의 시발점이 아닐까 하는 생각이 듭니다. 

​    

 이제 증명을 적어보려 하는데 물론 프렐라이 솔루션을 참고 했고, 제 나름대로의 이해를 가미?해서 작성했습니다. 간략하게 정리해서 수학적으로 굉장히 엄밀한 증명은 아니지만 큰 논리적 오류는 없는 것 같습니다.  증명은 편의상 영어로 했습니다. 

​    

### Fundamental Theorem of Algebra $\Longleftrightarrow$ Hillbert's Nullstellenstaz for $\C[x] $

​    

proof)

($\Longrightarrow$)  

Let N be smallest ideal containing $f_{1}\left( x \right),f_{2}\left( x \right),...,f_{r}\left( x \right)$.

Since $\C[x]$ is P.I.D. , $\exist h\left(x \right) \in \C[x]$ s.t. $ N$ = $<h\left(x \right)>$.

Let $\alpha_{1}, \alpha_{2},… \alpha_{s}$ be all the zeros in $\C$ of $h\left(x \right) $.

By the Fundamental Theorem of Algebra,  ==$h\left(x\right)$ = $ c(x-\alpha_{1})^{m_{1}}(x-\alpha_{2})^{m_{2}}...(x-\alpha_{r})^{m_{r}}$ (where $m_{i}\in \N$  , $c \in \C$ )==

Now every $\alpha_{i}$ is zero of $f_{j}(x)$ , Since every $f_{j}(x)$ is multiple of $h(x)$, and has zeros. 

By assumption, g(x)  = $k(x)(x-\alpha_{1})(x-\alpha_{2})...(x-\alpha_{r})$ for some  $k(x) \in \C [x]$

Let K := $n = max[{m_{i}}]_{i=1}^{i=n}$ , Then $g(x)^K \in <h(x)> = N $

  

($\Longleftarrow$)

  Suppose $\exist p(x) \in \C[x]$ s.t. nonconstant polynomial which has no zero. 

By assumption, every element in $\C[x]$ satisfies condition of Nullstellensatz for p(x). 

Thus $\forall f(x) \in \C[x]$, $f(x)^n \in <p(x)>$ for some $ n \in \N$.

$\Longrightarrow$  p(x)|f(x)	

$\Longrightarrow$  For f(x) = 1, p(x)|1 

This is contradictory to the fact that p(x) is nonconstant polynomal. 

  

  

 증명 자체가 생각보다 어려운 증명은 아닌데, 문제 자체를 이해하는 것이 조금 어려웠습니다. 첫 증명에서 $\C[x]$ 의 모든 ideal 은 principal ideal 임을 사용하는 방법도 다른 문제에서 많이 사용되었던 증명 방식이라 완전히 생각하기 어려운 방법은 아니었는데 h(x)의 형태가 구체적으로 저런식의 모양을 가질 수 밖에 없다고 결론 내리는 아이디어가 가장 큰 핵심이었습니다. 



​	







