---
type: post
title: Maximum Likelihood Estimation and Bayesian Estimation
category: Pattern Classification
---

<a href="https://ngs00.github.io/Bayesian-Decision-Theory/">앞의 글</a>에서는 *prior probability* $$P(w_i)$$와 *class-conditional probability* $$p(\boldsymbol{x}|w_i)$$ (모델의 parameter)를 이용하여 최적의 classifier를 설계하는 방법에 대해 설명하였다. 그러나 실제 문제에서는 대부분 이러한 정보들이 주어지지 않기 때문에 우리는 다른 정보들로부터 prior probability와 class-conditional probability를 추정해야 한다.
<br />
알려지지 않은 확률이나 확률 모델의 parameter를 추론하기 위한 한 가지 방법으로는 주어진 sample로부터 parameter를 추론하는 것이 있다. 주어진 sample로부터 알려지지 않은 확률이나 parameter를 추론하기 위해 많이 사용되고 있는 방법으로는 *maximum likelihood estimation (MLE)*과 *Bayesian estimation*이 있다. 아마도 머신러닝을 공부하다보면 maximum likelihood estimation이라는 단어를 매우 자주 접하게 될 것이다.
<br />
많은 경우에 MLE와 Bayesian estimation을 통해 얻어진 결과는 동일하다. 그러나 두 방법은 같은 estimation problem에 대해 다음과 같은 매우 다른 관점에서 문제를 해석한다.
*   **Maximum Likelihood Estimation**: MLE는 알려지지 않은 parameter를 고정된 값으로 생각한다. 모델의 parameter를 고정된 값으로 생각하면서 주어진 sample이 실제로 관측될 확률을 최대화 하는 것이 MLE의 estimation 전략이다.
*   **Bayesian Estimation**: 반면에 Bayesian estimation은 parameter를 binomial distribution, normal distribution 등과 같이 알려진 확률 분포를 따르는 random variable로 생각한다.
<br />

### Maximum Likelihood Estimation
MLE는 sample의 수가 증가하여도 비교적 잘 수렴하는 특징과 다른 estimiation 방법들에 비해 단순하기 때문에 패턴 인식, 머신 러닝 등의 분야에서 많이 이용되고 있다. 머신러닝 분야에서 잘 알려진 Hidden Markove Model, Restricted Boltzmann Machine 등의 모델들은 모두 MLE를 통해 학습을 진행할 수 있다.
<br />
그러나 MLE 또한 완벽한 것은 아니다. *likelihood*에 해당하는 $$p(\boldsymbol{x}|\omega_j)$$를 풀어서 쓰면, $$p(x_1, x_2, ..., x_M|\omega_j)$$가 되기 때문에 입력에 존재하는 요소들의 dependency를 모두 고려하여 추정을 해야 한다. 이러한 문제는 대부분 해결하기가 매우 어렵거나, 불가능한 경우도 있다. MLE에서는 아래와 같은 몇 가지 가정을 추가함으로써 likelihood를 계산 가능한 형태로 변형한다.
<ul>
    <li>우리는 $$p(\boldsymbol{x}|\omega_j)$$를 잘 알려진 형태의 probability distribution이라고 가정한다. 따라서, model parameter $$\omega_j$$를 구하면 likelihood를 정확하게 계산할 수 있다.</li>
    <li>Sample에 존재하는 각 instance들은 독립적으로 생성된다.</li>
    <li>하나의 instance의 각 element들은 독립적으로 생성된다.</li>
    <li>각 class의 model parameter $$\omega_j$$는 다른 model parameter들과 독립적이다.</li>
</ul>

우리는 <a href="https://ngs00.github.io/Bayesian-Decision-Theory/">앞의 글</a>에서 *posterior probability*를 이용하여 결정을 내리는 것이 *probability error* 또는 *risk*를 최소화한다는 것을 보았다. 
