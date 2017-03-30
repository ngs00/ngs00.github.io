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

NBC는 입력 데이터에 포함된 어떤 요소가 나타날 때, 주어진 입력이 어떠한 class에 속할 확률을 비교하여 가장 높은 확률을 갖는 class로 데이터를 할당하도록 동작한다. 이러한 동작은 <a href="https://ngs00.github.io/Bayesian-Decision-Theory/">앞의 글</a>에서 설명한 *discriminant function*기반의 network와 매우 유사하다. NBC는 주어진 입력을 분류하기 위해 Bayes' theorem을 이용하기 때문에 Naive "Bayes" Classifier라는 명칭이 붙었다.
