---
type: post
title: Bayesian Decision Theory
category: Pattern Classification
---

Bayesian decision theory는 패턴 인식 문제를 풀기 위한 통계적 방법론이다. 이 방법론은 어떠한 선택을 했을 때 발생하는 비용을 토대로 선택과 비용 간의 tradeoff를 계산하여 어떠한 선택을 할지를 결정한다. Bayesian decision theory는 가장 근본적이면서도 이상적인 방법론으로써, 우리가 결정을 내리기 위해 필요한 모든 확률을 알고 있다는 가정 하에 최적의 선택을 결정할 수 있다.
<br />
예를 들어, 이 세상에 동물은 고양이와 개만 있다는 가정 하에 두 동물을 분류하는 classifier를 설계한다고 생각하자. 우리는 classifier를 설계하기 위해서 *state of nature*라고 불리는 변수 $$w$$를 도입할 수 있다. 이 변수가 $$w_1$$일 경우에는 고양이를 의미하며, $$w_2$$일 경우에는 개를 의미한다. 우리의 목적은 어떠한 입력이 주어졌을 때, $$w$$의 값을 결정하는 것이다. 그러나 우리는 모든 것을 다 알고 있는 절대자가 아니기 때문에 $$w$$를 특정한 값으로 결정할 수 없다. 따라서, 주어진 입력과 다른 확률적 지식을 바탕으로 $$w$$를 확률적으로 정의한다.
<br />
우리가 어떠한 확률론적 결정을 내리기 위해 필요한 것 중 하나는 *priori probability*라고 불리는 확률이다. 여기에서 말하는 priori는 "선험적인"이라는 뜻을 갖는 단어로써, "이 세상에는 고양이보다 개가 많다" 또는 "이 세상에는 개보다 고양이가 많다"는 것과 같은 선험적인 지식을 말한다. 앞의 예에서 $$P(w_1)$$는 주어진 입력이 고양이일 확률을 나타내며, $$P(w_2)$$는 개일 확률을 나타낸다. 즉, priori probability는 우리가 입력을 관측하기 전에 알고 있는 지식을 의미한다.
<br />
우리의 목적은 어떠한 입력 $$x$$가 주어졌을 때 state of nature $$w$$의 값을 결정하는 것으로, 이를 확률적으로 나타내면 $$P(w|x)$$라는 조건부확률로 나타낼 수 있다. Bayesian decision theory에서는 이 조건부확률을 *class-conditional probability*라고 하며, 우리의 목적은 $$P(w|x)$$를 잘 계산하는 classifier를 설계하는 것이다.
<br />
### Probability error and probabilistic decision
두 개의 class만 존재하는 패턴인식 문제에서 Bayesian decision theory의 기본 개념은 $$P(w_1|x)$$가 $$P(w_2|x)$$보다 크다면 $$w_1$$을 선택하고, 그렇지 않으면 $$w_2$$를 선택하는 것이다. 이러한 방법을 따를 경우, 우리가 결정을 내릴 때마다 발생하는 *probability error* $$P(error|x)$$는 다음과 같이 정의된다.

$$
P(error|x) =
\begin{cases}
P(w_2|x),  & \text{if $P(w_1|x)$ > $P(w_2|x)$} \\
P(w_1|x), & \text{otherwise}
\end{cases}
$$

한 가지 생각해볼 점은 이러한 결정 방법이 "probability error의 평균을 최소화하는 것인가?"이다. 모든 probability error를 수학적으로 표현하면 아래의 식 (2)와 같다.

$$
P(error) = \int_{-\infty}^\infty P(error, x) dx = \int_{-\infty}^\infty P(error|x)p(x) dx
$$

Probability error는 항상 0보다 크거나 같은 값을 갖기 때문에 $P(error|x)$를 최소화하는 것은 식 (2)를 최소화 하는 것과 같다. 따라서, 식 (1)과 같이 결정하는 것은 전체적인 probability error를 최소화한다.
<br />
위에서 설명한 바와 같이 $P(w|x)$를 이용하여 결정을 내리면, 우리는 probability error를 최소화 할 수 있다. 따라서, 이러한 최적의 결정을 내리기 위해서는 입력이 주어질 때마다 정확한 $P(w|x)$를 계산할 수 있어야 한다. 현실적으로 $P(w|x)$를 직접 계산하는 것은 매우 어렵거나 불가능하기 때문에 우리는 식 (3)과 같이 정의되는 *Bayes' theorem*을 이용하여 간접적으로 $P(w|x)$를 계산한다.

$$
P(w|x) = \frac{p(x|w)P(w)}{p(x)}
$$

Bayes' theorem을 이용하면, 식 (1)을 다음과 같이 변경할 수 있다.

$$
P(error|x) =
\begin{cases}
P(w_2|x),  & \text{if $p(x|w_1)P(w_1)$ > $p(x|w_2)P(w_2)$} \\
P(w_1|x), & \text{otherwise}
\end{cases}
$$

많은 경우에 Bayes' theorem을 이용하여 식 (1)을 식 (3)과 같은 형태로 변형하면, 계산이 불가능했던 식이 계산 가능한 형태로 변형된다.
<br />
### Generalization of Bayesian decision theory
지금까지는 두 개의 class (state of nature)만 있는 경우에 대해서 probability error를 설명하였다. 지금부터는 $$C$$개의 class와 $$K$$개의 action이 존재하는 패턴인식 문제를 예로 들어 앞에서 설명한 방법론을 일반화할 것이다. 여기에서 action은 "$$w_1$$을 선택한다", "밥을 먹는다", "1을 더한다" 등과 같은 행동들을 의미한다. 또한, 입력 $$x$$를 bold text로 표기할 것인데, 이는 데이터의 feature가 여러개라는 것을 의미하며, $$x$$는 해당 데이터의 feature vector이다. 따라서, $$\boldsymbol{x}$$는 데이터의 feature들을 벡터의 형태로 나타낸 것과 같다. 마지막으로, 직관적인 이해를 위해 class와 state를 혼용하여 사용할 것이다.
<br />
어떠한 state $$w_j$$에서 특정한 action $$\alpha_i$$를 할 때의 *loss*은 $$\lambda(\alpha_i|w_j)$$로 정의되며, loss의 expected value를 *risk* (또는 *expected loss*)라고 한다. 주어진 입력 $$x$$에 대한 risk를 의미하는 *conditional risk*는 아래의 식 (5)와 같이 정의된다.

$$
R(\alpha_i|\boldsymbol{x}) = \sum_{j=1}^{C} \lambda(\alpha_i|w_j)P(w_j|x)
$$

우리의 목적은 식 (6)으로 나타내어지는 *overall risk*를 최소화하는 *decision rule* $$\alpha(\boldsymbol{x})$$를 찾는 것이다. 여기에서 $$\alpha(\boldsymbol{x})$$는 주어진 데이터 $$\boldsymbol{x}$$에 대해 하나의 action $$\alpha_i$$를 결정하는 함수이다.

$$
R = \int R(\alpha(\boldsymbol{x})|\boldsymbol{x})p(\boldsymbol{x}) d\boldsymbol{x}
$$

어떠한 행동을 나타내는 $$\alpha_i$$를 추가함으로써, 우리가 풀고자 하는 문제는 probability error를 최소화하는 것에서 확률과 loss를 동시에 고려하는 overall risk 최소화 문제로 변형되었다.
<br />
### Discriminant functions and classifiers
어떠한 classifier를 설계할 때, 가장 직관적이고 단순한 방법은 *discriminant function* $$g(\boldsymbol{x}, w_i)$$를 이용하는 것이다. 만약, 주어진 입력 $$\boldsymbol{x}$$에 대해서 다음의 식 (7)이 성립한다면, classifier는 $$\boldsymbol{x}$$를 $$w_i$$로 할당할 것이다.

$$
g(\boldsymbol{x}, w_i) > g(\boldsymbol{x}, w_j) \qquad \text{for all} i /neq j
$$

Discriminant function $$g(\boldsymbol{x}, w_i)$$는 classifier의 설계 목적에 따라 여러 형태로 정의될 수 있다. 앞에서 정의한 conditional risk를 이용하여 정의한다면, $$g(\boldsymbol{x}, w_i) = -R(\alpha_i|\boldsymbol{x})$$의 형태로 discriminant function을 표현할 수 있다. 만약 어떠한 함수 $$f$$가 단조증가함수 (monotonically increasing function)라면, discriminant function을 $$f(g(w_i|\boldsymbol{x}))$$로 정의하여도 decision rule은 변하지 않는다. 이러한 변환은 classifer의 결과에 영향을 주지 않으면서, 계산을 쉽게 만들기 위해 많이 사용된다.
<br />
만약 $$C$$개의 discriminant function이 있다면, 이 함수들은 *decision region*이라고 정의되는 공간을 $$C$$개의 영역으로 분할할 것이다 [그림 2].

위의 그림에서 각 decision region의 경계를 *decision boundary*라고 하며, 이러한 경계는 discriminant function $$g(\boldsymbol{x}, w_i)$$에 의해 정의된다.
<br />
Classifier를 discriminant function 계산하여 계산된 값으로 class를 결정하는 network 또는 machine이라고 생각하면, 우리가 설계하고자 하는 classifier는 그림 3과 같은 구조로 표현된다.

이러한 관점은 매우 일반적이어서, Restricted Boltzmann Machine을 비롯한 많은 확률기반의 머신러닝 모델들은 그림 3과 같은 구조를 갖는다.
