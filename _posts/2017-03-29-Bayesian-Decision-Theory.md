---
type: post
title: Bayesian Decision Theory
category: Pattern Classification
---

Bayesian decision theory는 패턴 인식 문제를 풀기 위한 통계적 방법론이다. 이 방법론은 어떠한 선택을 했을 때 발생하는 비용을 토대로 선택과 비용 간의 tradeoff를 계산하여 어떠한 선택을 할지를 결정한다. Bayesian decision theory는 가장 근본적이면서도 이상적인 방법론으로써, 우리가 결정을 내리기 위해 필요한 모든 확률을 알고 있다는 가정 하에 최적의 선택을 결정할 수 있다.
</br>
&nbsp;&nbsp;&nbsp;&nbsp; 예를 들어, 이 세상에 동물은 고양이와 개만 있다는 가정 하에 두 동물을 분류하는 classifier를 설계한다고 생각하자. 이러한 classifier를 설계하기 위해서 state of nature라고 불리는 변수 $$w$$를 도입한다. 이 변수가 $$w_1$$일 경우에는 고양이를 의미하며, $$w_2$$일 경우에는 개를 의미한다. 우리의 목적은 어떠한 입력이 주어졌을 때, $$w$$의 값을 결정하는 것이다. 그러나 우리는 모든 것을 다 알고 있는 절대자가 아니기 때문에 $$w$$를 특정한 값으로 결정할 수 없다. 따라서, 주어진 입력과 다른 확률적 지식을 바탕으로 $$w$$를 확률적으로 정의한다.
