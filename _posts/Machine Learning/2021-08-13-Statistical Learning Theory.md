---
title: "[ML]2. Statistical Learning Theory"
categories:
  - MachineLearning
tags:
  - ML
  - python
  - Industrial Engineering
  - Blog
excerpt: Statistical Learning Theory and Empirical Risk Minimizer..
use_math: true
comments: true

---

## 0. Review
저번 포스트에서는 머신러닝으로 문제를 해결하는 과정과 문제를 설계 할때 사용되는 용어들을 정의했다. 오늘은 이 정의들을 이용해서 일반적인  __Statistical Learning Theory Framework__ 에대해서 알아보겠다.

## 1. Setting
- #### Risk
  데이터$(x,y)$를 독립적으로$(i.i.d)$ 랜덤하게 생성하는 분포 __$ P_{ X\times Y } $__ 가 있다고 하자. 우리는 데이터에대해서 일반적으로 loss function을 작게 해주는 $f$를 구하고 싶을 것이다. 이를 조금더 공식화해보자.
  $P_{ X\times Y }$에서 추출한 데이터 $(x,y)$ 에대하여 decision function $f$를 이용해 구한 loss function의 기대값을 $R$이라고 하면, 



  $$R(f) = E [l(f(x),y)]$$


  라고 쓸 수 있다. 하지만 우리는 $ P_{ X\times Y } $ 가 무엇인지 모르기 때문에 정확한 기대값을 계산할 수 없다. 하지만, 우리는 통계적인 방법을 이용해서 기댓값을 예측을 할수 있게 된다.

- #### Bayes Decison Function.
  __Bayes decision function:__  $$f^*:X \rightarrow A $$ is a function that achieves __minimal risk__ among all possible function.

  즉, Decision function 중에서 리스크를 <span style="color:red">최소화</span>하는 함수를 베이지안 결정함수라고 한다는 것이다.
  이를 수학적으로 나타내보면, Bayes decision function $$f^*$$는


  $$f^*=\underset{f}{\operatorname{argmin}} R(f)$$


  추가로, Decision function이 $f^*$(bayes decision function)일 때의 리스크 $R$을 __Bayes Risk__ 라고 한다.

## 2. Decision Theory
- #### Setting
  Bayes decision function을 결정하는 과정을 살펴보자.
  데이터 x가 주어졌을때, y를 예측하는 함수가 $f$ 가 있다고 하자. 우리는 일반적으로 작은 리스크를 선호할 것이며, 리스크를 최소화하는 함수를 Bayesian Decision Function이라고 정의했다. 이를 수학적으로 나타내보자.
  데이터쌍 $(X,Y)$가 분포 $P(X,Y)$로부터 주어졌다고 하자, 기댓값의 정의로부터

  $$ r(f):= \int\int L(f(X),Y)p(X,Y)dX\ dY $$
  
  즉 $f$ 에관한 함수 $f$ 를 최소화하는 문제이다. 이어서 optimal $f$를결정하는 과정을 알아보자.

- #### Optimal Decision Rule
  Optimal Decision Rule: For each $X$ choose the prediction $f(X)$ that minimizes the conditional expected risk
  즉, 특정 데이터 쌍 $(X,Y)$가 주어졌을때, 주어진 $X$ 에관한 conditional risk 를 최소화하는 $f$가 optimal 이라는 것이다.
  이를 수학적으로 표현해보면, 

  $$ f_{opt}= argmin_f \int L(f,Y)p(Y|X)dY $$

  여기서 적분이 한번만 되는 이유는 한 데이터 쌍에 대해서만 구하기 때문이고, Conditional Risk 를 최소화해야하기 때문에, Conditional Probability 가 주어졌다.
  이런식으로 모든 데이터쌍에 대해, 각각의 $f_{opt}$ 를 구해주면, 우리가 원하는 Expected Risk가 최소화된다는것은 간단하게 증명 할 수 있다.
  통계시간에 배우는 Bayesian 정리 로부터 $P(X,Y)=P(Y|X)P(X)$로 바꿔서 쓸 수 있고, 어떤 함수 $f$에 대해서, 

  $$ r(f):= \int(\int L(f(X),Y)p(Y|X)dY)P(X)dX $$

  로 쓸 수 있게 된다. 아까 정한 함수 $f_{opt}$ 에대하여, 가운데 괄호친 부분은 최소화 되고
  모든 함수 $f$에 대하여, 

  $$ r(f) \geq \int(\int L(f_{opt}(X),Y)p(Y|X)dY)P(X)dX $$

  따라서 특정 데이터 $X$ 가주어졌을때 그 conditional risk 를 최소화하는 함수를 Optimal Decision Function혹은 Bayesian Decision function이라고 할 수 있다.






## 3. Examples
- #### 0-1 Loss
  분포 $P(X,Y)$ 가 주어지고 Y는 k개의  Discreteg한 category 로 분류된다고 하고($Y=y_{1},...,y_{k}$), Loss Function $l$은 
  
  $$l(a,Y)= 1(a\neq Y)$$
  으로 정의된다고 하자. (0 if a=Y else 1)
  이 경우에 Optimal Decision Rule 을 결정해보자.  데이터 $x$ 가 주어진다면, 그의 결과 $y$는 분포 $y|x$ 로부터 주어지고, 0-1 loss 의 Conditional Risk를 수식으로 표현해보면,

  $$
  \begin{aligned}
  r(f|x)&= \sum_{i=1}^{k} 1(f(x)\neq y_i)P(y_i|x) \\
  &=  1-\sum_{i=1}^{k} 1(f(x)=y_i)P(y_i|x)\\
  &= 1- P(\hat{y}|x)
  \end{aligned}
  $$
  
  이다. ( $\hat{y}$ 는 $f$ 가 예측한 $y$ ) 결국 
  주어진 $x$ 를 통해서 $y$ 를 구할 때 가장 가능성이 높은 $y$ 를 골라야 리스크가 최소화 된다는 얘기다.
  
  즉, decision rule 은 , 주어진 $x$ 에 대해서 가능한 $y$ 값들($y_1\ to \ y_k$ ) 중 가장 확률이 높은걸 고르는 것이 될 것이다. 
  이를 수식으로 표현하면,

  $$ f^*=argmax_{y}p(Y=y|X=x)  $$




- #### Least Square Regression.
  Loss function $l(a-y)=(a-y)^2$ 으로 정의된다고 하자. 
  이 경우에 Optimal Decision Rule을 결정해보자.
  데이터 $x$ 가 주어진다면, 그의 결과 $y$는 분포 $y|x$ 로부터 주어지고, Squared loss 의 Conditional Risk를 수식으로 표현해보면,  

  $$
  \begin{aligned}
  E[(f(x)-y)^2|x]&=E[(f(x)-E[y|x]+E[y|x]-y)^2|x] \\
  &=E[(f(x)-E[y|x])^2|x]+E[(E[y|x]-y)^2|x]+2E[(f(x)-E[y|x])(E[y|x]-y)|x] \\
  \end{aligned}
  $$



  증명을 위해서 햇갈릴 만한 개념 세가지만 짚고 넘어가자  
  $$
  \begin{aligned}
  1.&E[y|x]는 ,\ x에관한 \ 함수이다  \\
  2.&E[E[y|x]]=E[y]  이다. (Law \ of \ Iterated \ Expectaion.)\\
  3.&E[g(x)Y|X=x]=g(x) \times E[Y|X=x](g(x) 를 \ 상수취급 \ 가능 )
  \end{aligned}
  $$


  잘 와닿지 않는다면, [여기](https://www.youtube.com/watch?v=yDkm9AYaczk)를 참고해보자.
  

  $g(x)=E[y|x]$   
  라고 하자. 문제를 다시써보면

  $$
  \begin{aligned}
  E[(f(x)-y)^2|x]&=E[(f(x)-E[y|x]+E[y|x]-y)^2|x] \\
  &=E[(f(x)-g(x))^2|x]+\textcolor{blue}{E[(g(x)-y)^2|x]}+\textcolor{red}{2E[(f(x)-g(x))(g(x)-y)|x]} \\
  \end{aligned}
  $$

  여기서 빨갛게 칠한 부분을 보자. $(f(x)-g(x))$ 는 위에서 말했듯이 상수취급해줄 수 있고,
  그렇게 된다면 남는건, $E[(g(x)-y)|x]$인데, 이를 정리하면,
  
  $$
  \begin{aligned}
  E[(g(x)-y)|x]&=E[g(x)|x]-E[y|x]\\
  &=g(x)-E[y|x] \\
  &= E[y|x]-E[y|x](앞서서, g(x)=E[y|x]로 정의했다.) \\
  &=0
  \end{aligned}
  $$

  따라서, 빨갛게 칠한 부분은 0이되고, 
  $$E[(f(x)-y)^2|x]=E[(f(x)-g(x))^2|x]+ \textcolor {blue}{E[(g(x)-y)^2|x]}$$ 
  이렇게 쓸 수 있다.  

  다시 위에수식으로 돌아가 이번엔 파란글씨부분을 살펴보자, 
  우리가 최소화하고자하는 함수 $E[(f(x)-y)^2|x]$는 $f$에 관한 함수이며,  $E[(g(x)-y)^2|x]$ 는 $f$에관한 항이 하나도 없으므로, 상수로 생각할 수 있다.  결국 $E[(f(x)-y)^2|x]$ 를 최소화하기 위해서는, $E[(f(x)-g(x))^2|x]$ 을 최소화 하는것이랑 같고,이를 최소화하기 위해서 $ f=g(x) $, 이어야하고, $f ^*=E[y|x ]$ 이된다.

  여기까지, 우리는 주어진 데이터 $ x $ 에 대하여  conditional risk를 최소화해주는 $ f^* $ 을 구했다. 
  모든 데이터 x에 대해서, 각각의 Conditional Risk 를 최소화시켜주는 $f^*$가 존재할 것이며,   

  $$ E[(f^*(x)-y)^2|x] <=E[(f(x)-y)^2|x] $$

  가 성립하고, 

  $$E[(f^* (x)-y)^2]=E[E[(f^* (x)-y)^2|x]]$$  
  
  이므로(Law of Iterated Expectation),


  $$ E[(f(x)-y)^2]=\int\limits E[(f(x)-y)^2|x] P(X=x)dx\geq\int\limits E[(f^* (x)-y)^2|x] P(X=x)dx$$


  따라서,주어진 데이터 $x$에 대하여 Optimal Decision Rule은 
  $f^*=E[y|x]$ 이다.
  사실, Expected Risk를 적분형태로 표현해서, 미분을통해 구하는 방법도 있지만, 강의에서는 이 방법을 사용했다.


## 4. 참고 문서
Introduction to Statistical Learning theory Lecture Slide(https://davidrosenberg.github.io/mlcourse/Archive/2017Fall/Lectures)

Bayes Decision Rule for prediction problems. (https://stephens999.github.io/fiveMinuteStats/decision_theory_bayes_rule.html)










