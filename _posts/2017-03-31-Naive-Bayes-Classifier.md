---
type: post
title: Naive Bayes Classifier (NBC)
category: Pattern Classification
---

이 글에서는 가장 단순한 형태의 Naive Bayes Classifier (NBC)에 대해 설명한다. NBC는 <a href="https://ngs00.github.io/Bayesian-Decision-Theory/">앞의 글</a>에서 설명한 Bayes decision theory에 어떠한 가정을 추가하여 만들어진 모델이다. NBC는 확률 기반 모델의 classifier를 만들 수 있는 간단하면서도 대중적인 방법이다. 그러나 간단한 원리와 구조로 설계되었음에도 NBC는 여러 복잡한 문제에서도 잘 작동한다.
<br />
NBC는 1960 년대 초에 text mining 분야에 소개된 이후로 스팸 분류, 주제별 문서 분류 등에 광범위하게 이용되고 있다. 이 글에서는 NBC에 적용된 Bayes decision theory와 NBC의 설계 원리에 대해 소개하고, 간단한 주제별 문서 분류 예제를 통해 NBC의 학습 및 동작 원리를 설명할 것이다.
<br />
### Conditional probability and Bayes' theorem
확률 이론에서 *conditional probability* $$P(B|A)$$는 어떠한 사건 A가 발생했을 때, 사건 B가 발생할 확률을 의미한다. 이 경우, conditional probability $$P(B|A)$$는 식 (1)과 같이 두 사건 A, B가 동시에 발생할 확률을 사건 A가 발생할 확률로 나눈 값과 같다.

$$
P(B|A) = \frac{P(A,B)}{P(A)}
$$

이러한 conditional probability는 Bayes's theorem 이라는 것에 의해 아래의 식 (2)와 같이 계산될 수 있다.

$$
P(B|A) = \frac{P(A|B)P(B)}{P(A)}
$$

NBC는 입력 데이터에 포함된 어떤 요소가 나타날 때, 주어진 입력이 어떠한 class에 속할 확률을 비교하여 가장 높은 확률을 갖는 class로 데이터를 할당하도록 동작한다 [그림 1].
<div class="wrapper-img">
<img src="https://cloud.githubusercontent.com/assets/26436995/24494478/5d391d68-156d-11e7-8416-cd5d9f2f7238.png" width="500" />
<p>그림 1. The model structure of Naive Bayes Classifier</p>
</div>
이러한 동작은 <a href="https://ngs00.github.io/Bayesian-Decision-Theory/">앞의 글</a>에서 설명한 *discriminant function*기반의 network와 매우 유사하다. NBC는 주어진 입력을 분류하기 위해 Bayes' theorem을 이용하기 때문에 Naive "Bayes" Classifier라는 명칭이 붙었다.
<br />
### Naive assumption
주어진 데이터 $$\mathcal{D}$$에 대하여 해당 데이터를 어떠한 class $$c_i$$로 분류하는 NBC를 설계한다면, 우리는 $$P(c_i|\mathcal{D})$$를 계산해야 할 것이다. 만약 $$\mathcal{D}$$가 다수의 요소 $${e_1, e_2, ..., e_N}$$으로 구성된 데이터라면, 우리는 아래의 식 (3)과 같은 확률을 계산해야 한다.

$$
P(c_i|\mathcal{D}) = P(c_i|e_1, e_2, ..., e_N)
$$

NBC의 기본 원리는 식 (3)으로 표현되는 확률을 모든 class에 대해 계산하여 확률이 가장 높은 class로 입력을 분류하는 것이다. 위의 식 (3)에 Bayes' theorem을 적용하면, 우리는 계산 가능한 형태로 표현되는 식 (4)를 얻을 수 있으며, 분모에 위치한 $$P(\mathcal{D})$$는 모든 class가 같은 값을 갖기 때문에 생략할 수 있다.

$$
P(c_i|\mathcal{D}) = P(c_i|e_1, e_2, ..., e_N) = \frac{P(e_1, e_2, ..., e_N|c_i)P(c_i)}{P(\mathcal{D})}
$$

그러나 식 (3)을 계산할 때 해결이 불가능할 수도 있는 큰 문제가 발생하는데, $$P(e_1, e_2, ..., e_N|c_i)$$의 계산이 불가능하거나 또는 막대한 양의 계산이 필요할 수 있다. 이러한 문제를 해결하기 위해 NBC는 **입력을 구성하는 각각의 요소가 관측될 확률은 서로 독립적이다**라는 **naive**한 가정을 한다. 예를 들어, 어떠한 문서 $$\mathcal{D}$$에 "컴퓨터"라는 단어와 "사람"이라는 단어가 나타날 확률은 서로 독립적이라고 가정하는 것이다. NBC의 이러한 극단적으로 단순한 가정에도 불구하고, NBC는 여러 복잡한 문제에서도 잘 동작하며, 2004년의 한 논문 [1]에서는 NBC의 이러한 능력에는 명확한 이론적 이유가 있음을 보여주었다.
<br />
위에서 설명한 naive한 가정을 식 (4)에 적용하면, 우리는 계산 가능한 형태의 식 (3)을 얻을 수 있다. 이 식에서 $$P(\mathcal{D})$$는 모든 class가 같은 값을 갖기 때문에 생략되었다.
$$
\frac{P(e_1, e_2, ..., e_N|c_i)P(c_i)}{P(\mathcal{D})} \approx P(e_1|c_i)P(e_2|c_i) ... P(e_N|c_i)P(c_i)
$$
위의 식에서 $$P(e_j|c_i)$$는 class $$c_i$$일 때, 어떠한 요소 $$e_j$$가 나타날 확률이므로 비교적 쉽게 계산할 수 있다.


