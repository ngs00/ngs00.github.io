---
type: post
title: Maximum Likelihood Estimation
category: Pattern Classification
---

앞의 글에서는 prior probability $$P(w_i)$$와 class-conditional probability $$p(\boldsymbol{x}|w_i)$$ (모델의 parameter)를 이용하여 최적의 classifier를 설계하는 방법에 대해 설명하였다. 그러나 실제 문제에서는 대부분 이러한 정보들이 주어지지 않기 때문에 우리는 다른 정보들로부터 prior probability와 class-conditional probability를 추정해야 한다.
<br />
알려지지 않은 확률이나 확률 모델의 parameter를 추론하기 위한 한 가지 방법으로는 주어진 sample로부터 확률이나 parameter를 추론하는 접근 방법이 있다. 주어진 sample로부터 알려지지 않은 확률이나 parameter를 추론하기 위한 일반적이면서도 합리적인 방법으로는 *maximum likelihood estimation (MLE)*과 *Bayesian estimation*이 있다. 아마도 머신러닝을 공부하다보면 maximum likelihood estimation이라는 단어를 매우 자주 접하게 될 것이다.
<br />
많은 경우에 MLE와 Bayesian estimation을 통해 얻어진 결과는 동일하다. 그러나 두 방법은 같은 estimation problem에 대해 다음과 같은 매우 다른 관점에서 문제를 해석한다.
\begin{itemize}
  \item \textbf{Maximum Likelihood Estimation}: MLE는 알려지지 않은 parameter를 고정된 값으로 생각한다. 모델의 parameter를 고정된 값으로 생각하면서 주어진 sample이 실제로 관측될 확률을 최대화 하는 것이 MLE의 estimation 전략이다.
  
  \item \textbf{Bayesian Estimation}: 반면에 Bayesian estimation은 parameter를 binomial distribution, normal distribution 등과 같이 알려진 확률 분포를 따르는 random variable로 생각한다. 
\end{itemize}
