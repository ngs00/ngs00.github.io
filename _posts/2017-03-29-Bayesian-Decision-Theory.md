---
type: post
title: Bayesian Decision Theory
category: Pattern Classification
---

Bayesian decision theory는 패턴 인식 문제를 풀기 위한 통계적 방법론이다. 이 방법론은 어떠한 선택을 했을 때 발생하는 비용을 토대로 선택과 비용 간의 tradeoff를 계산하여 어떠한 선택을 할지를 결정한다. Bayesian decision theory는 가장 근본적이면서도 이상적인 방법론으로써, 우리가 결정을 내리기 위해 필요한 모든 확률을 알고 있다는 가정 하에 최적의 선택을 결정할 수 있다.
<br />
예를 들어, 이 세상에 동물은 고양이와 개만 있다는 가정 하에 두 동물을 분류하는 classifier를 설계한다고 생각하자. 이러한 classifier를 설계하기 위해서 state of nature라고 불리는 변수 $$w$$를 도입한다. 이 변수가 $$w_1$$일 경우에는 고양이를 의미하며, $$w_2$$일 경우에는 개를 의미한다. 우리의 목적은 어떠한 입력이 주어졌을 때, $$w$$의 값을 결정하는 것이다. 그러나 우리는 모든 것을 다 알고 있는 절대자가 아니기 때문에 $$w$$를 특정한 값으로 결정할 수 없다. 따라서, 주어진 입력과 다른 확률적 지식을 바탕으로 $$w$$를 확률적으로 정의한다.
<br />
우리가 어떠한 확률론적 결정을 내리기 위해 필요한 것 중 하나는 priori probability라고 불리는 확률이다. 여기에서 말하는 priori는 "선험적인"이라는 뜻을 갖는 단어로써, "이 세상에는 고양이보다 개가 많다" 또는 "이 세상에는 개보다 고양이가 많다"는 것과 같은 선험적인 지식을 말한다. 앞의 예에서 $$P(w_1)$$는 주어진 입력이 고양이일 확률을 나타내며, $$P(w_2)$$는 개일 확률을 나타낸다. 즉, priori probability는 우리가 입력을 관측하기 전에 알고 있는 지식을 의미한다.
<br />
우리의 목적은 어떠한 입력 $$x$$가 주어졌을 때 state of nature $$w$$의 값을 결정하는 것으로, 이를 확률적으로 나타내면 $$P(w|x)$$라는 조건부확률로 나타낼 수 있다. Bayesian decision theory에서는 이 조건부확률을 class-conditional probability라고 하며, 우리의 목적은 $$P(w|x)$$를 잘 계산하는 classifier를 설계하는 것이다.
<br />
### Probability error and decision
두 개의 클래스만 존재하는 패턴인식 문제에서 Bayesian decision theory의 기본 개념은 $$P(w_1|x)$$가 $$P(w_2|x)$$보다 크다면 $$w_1$$을 선택하고, 그렇지 않으면 $$w_2$$를 선택하는 것이다.

$$
f(n) =
\begin{cases}
n/2,  & \text{if $n$ is even} \\
3n+1, & \text{if $n$ is odd}
\end{cases}
$$
