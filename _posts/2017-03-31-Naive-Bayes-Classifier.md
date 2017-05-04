---
type: post
title: Naive Bayes Classifier
category: Pattern Classification
---

이 글에서는 가장 단순한 형태의 Naive Bayes Classifier (NBC)에 대해 설명한다. NBC는 <a href="https://ngs00.github.io/Bayesian-Decision-Theory/">앞의 글</a>에서 설명한 Bayes decision theory에 어떠한 가정을 추가하여 만들어진 모델이다. NBC는 확률 기반 모델의 classifier를 만들 수 있는 간단한 방법중 이다. 그러나 간단한 원리와 구조로 설계되었음에도 NBC는 여러 복잡한 문제에서도 잘 작동한다.
<br />
NBC는 1960 년대 초에 text mining 분야에 소개된 이후로 스팸 분류, 주제별 문서 분류 등에 광범위하게 이용되고 있다. 이 글에서는 NBC에 적용된 Bayes decision theory와 NBC의 설계 원리에 대해 소개하고, 간단한 주제별 문서 분류 예제를 통해 NBC의 학습 및 동작 원리를 설명할 것이다.
<br />
### Conditional probability and Bayes' theorem
확률 이론에서 *conditional probability* $$P(B|A)$$는 어떠한 사건 A가 발생했을 때, 사건 B가 발생할 확률을 의미한다. 이 경우, conditional probability $$P(B|A)$$는 식 (1)과 같이 두 사건 A, B가 동시에 발생할 확률을 사건 A가 발생할 확률로 나눈 값과 같다.

$$
P(B|A) = \frac{P(A,B)}{P(A)}
$$

이러한 conditional probability는 Bayes's theorem에 의해 아래의 식 (2)와 같이 계산될 수 있다.

$$
P(B|A) = \frac{P(A|B)P(B)}{P(A)}
$$

NBC는 입력 데이터가 주어졌을 때 주어진 입력이 어떠한 class에 속할 확률들을 비교하여 가장 높은 확률을 갖는 class로 입력 데이터를 분류하도록 동작한다 [그림 1].
<div class="wrapper-img">
<img src="https://cloud.githubusercontent.com/assets/26436995/24494478/5d391d68-156d-11e7-8416-cd5d9f2f7238.png" width="500" />
<p>그림 1. The model structure of Naive Bayes Classifier</p>
</div>
이러한 동작은 <a href="https://ngs00.github.io/Bayesian-Decision-Theory/">앞의 글</a>에서 설명한 *discriminant function*기반의 network와 매우 유사하다. NBC는 주어진 입력을 분류하기 위해 Bayes' theorem을 이용하기 때문에 Naive "Bayes" Classifier라는 명칭이 붙었다.
<br />
### Naive assumption
주어진 데이터 $$\mathcal{D}$$에 대하여 해당 데이터를 어떠한 class $$c_i$$로 분류하는 NBC를 설계한다면, 우리는 $$P(c_i|\mathcal{D})$$를 계산해야 할 것이다. 만약 $$\mathcal{D}$$가 다수의 요소 $${e_1, e_2, ..., e_M}$$으로 구성된 데이터라면, 우리는 아래의 식 (3)과 같은 확률을 계산해야 한다.

$$
P(c_i|\mathcal{D}) = P(c_i|e_1, e_2, ..., e_M)
$$

NBC의 기본 원리는 식 (3)으로 표현되는 확률을 모든 class에 대해 계산하여 확률이 가장 높은 class로 입력을 분류하는 것이다. 위의 식 (3)에 Bayes' theorem을 적용하면, 우리는 계산 가능한 형태로 표현되는 식 (4)를 얻을 수 있으며, 분모에 위치한 $$P(\mathcal{D})$$는 모든 class가 같은 값을 갖기 때문에 생략할 수 있다.

$$
P(c_i|\mathcal{D}) = P(c_i|e_1, e_2, ..., e_M) = \frac{P(e_1, e_2, ..., e_M|c_i)P(c_i)}{P(\mathcal{D})}
$$

그러나 식 (3)을 계산할 때 매우 큰 문제가 발생하는데, $$P(e_1, e_2, ..., e_M|c_i)$$의 계산이 불가능하거나 또는 막대한 양의 계산이 필요할 수 있다. 이러한 문제를 해결하기 위해 NBC는 **"입력을 구성하는 각각의 요소가 관측될 확률은 서로 독립적이다"**라는 **"naive"**한 가정을 한다. 예를 들어, 어떠한 문서 $$\mathcal{D}$$에 "컴퓨터"라는 단어와 "사람"이라는 단어가 나타날 확률은 서로 독립적이라고 가정하는 것이다. NBC의 이렇게 극단적으로 단순한 가정에도 불구하고, NBC는 여러 복잡한 문제에서도 잘 동작하며, 2004년의 한 논문 [2]에서는 NBC의 이러한 능력에는 명확한 이론적 이유가 있음을 보여주었다.
<br />
위에서 설명한 naive한 가정을 식 (4)에 적용하면, 우리는 계산 가능한 형태의 식 (3)을 얻을 수 있다. 이 식에서 $$P(\mathcal{D})$$는 모든 class가 같은 값을 갖기 때문에 생략되었다.

$$
\frac{P(e_1, e_2, ..., e_M|c_i)P(c_i)}{P(\mathcal{D})} \approx P(e_1|c_i)P(e_2|c_i) ... P(e_M|c_i)P(c_i)
$$

위의 식에서 $$P(e_m|c_i)$$는 class $$c_i$$일 때, 어떠한 요소 $$e_m$$가 나타날 확률이므로, 주어진 training set을 이용하여 비교적 쉽게 계산할 수 있다.
<br />
### Example dataset
이 항목에서는 간단한 형태의 NBC를 이용하여 주제별 문서 분류 classifier 설계 방법에 대해 서술한다. 먼저, classifier를 학습시키기 위한 training set이 다음과 같이 주어졌다고 가정한다.
<div class="wrapper-table">
    <table>
        <thead>
            <tr>
                <th>Training instance</th>
                <th>Elements</th>
                <th>Target class</th>
            </tr>
        </thead>

        <tbody>
            <tr>
                <td>instance 1</td>
                <td>function, class, struct, pointer</td>
                <td>C/C++</td>
            </tr>
            <tr>
                <td>instance 2</td>
                <td>method, class, int</td>
                <td>Java</td>
            </tr>
            <tr>
                <td>instance 3</td>
                <td>pointer, int, struct, float</td>
                <td>C/C++</td>
            </tr>
            <tr>
                <td>instance 4</td>
                <td>final, int, float</td>
                <td>Java</td>
            </tr>
            <tr>
                <td>instance 5</td>
                <td>string, array, synchronized</td>
                <td>Java</td>
            </tr>
        </tbody>
    </table>
    <p>표 1. Training example</p>
</div>
우리의 목적은 training set으로부터 결정을 내리기 위해 필요한 $$P(e_m|c_i)$$와 $$P(c_i)$$를 계산하는 것이다. 먼저, $$P(e_m|c_i)$$는 class $$c_i$$일 때, $$e_m$$이라는 요소가 나타날 확률을 의미하므로, $$c_i$$에 포함된 전체 요소 중에 $$e_m$$에 해당하는 요소가 몇 개 있는지를 알면 쉽게 계산할 수 있다. 예를 들어, class Java에 대한 string과 int라는  확률은 다음과 같이 계산될 수 있다.

$$
P(string|Java) = \frac{1}{9}
$$

$$
P(int|Java) = \frac{2}{9}
$$

그러나 NBC를 실제로 구현할 때는 위와 같은 방법으로 확률을 계산하지 않는데, 그 이유는 아래와 같다.

<ul>
    <li>Training set에 제시되어 있지 않은 요소에 대한 확률은 항상 0으로 계산된다.</li>
    <li>확률은 항상 1보다 작거나 같은 값을 갖기 때문에 training instance들을 구성하는 element의 수가 많아지면, 확률의 곱으로 표현되는 식 (5)의 값이 너무 작아져서 비교가 어려워진다.</li>
</ul>

첫 번째 문제를 해결하기 위한 방법으로는 Laplace smoothing이 있으며, 두 번째 문제에 대해서는 Log probability를 이용함으로써 해결한다.
<br />
### Laplace smoothing
Training set에 제시되어 있지 않은 요소에 대한 확률이 항상 0으로 계산되어 다른 정보들이 모두 무시되는 문제를 해결하기 위해 NBC에서는 Laplace smoothing을 이용한다. Laplace smoothing을 적용한 $$P(e_m|c_i)$$는 아래의 식 (8)과 같이 정의된다 [3].

$$
P(e_m|c_i) = \frac{count(e_m, c_i) + 1}{|c_i| + |u_i|}
$$

위의 식에서 $$count(e_m, c_i)$$는 training set에서 $$e_m$$이 $$c_i$$일 때 나타나는 횟수이고, &#124;$$c_i$$&#124;와 &#124;$$u_i$$&#124;는 각각 class $$c_i$$에 있는 모든 element의 수와 모든 클래스에서 유일한 element의 수이다. 예를 들어, Laplace smoothing을 적용한 $$P(int$$&#124;$$Java)$$는 다음과 같이 계산된다.

$$
P(int|Java) = \frac{2 + 1}{9 + 11} = \frac{3}{20}
$$

또한, training set에 포함되어 있지 않은 *while*이라는 요소에 대해서도 다음과 같이 0이 아닌 아주 작은 값의 확률을 갖는다.

$$
P(while|Java) = \frac{0 + 1}{9 + 11}
$$

이와 같이 Laplace smoothing을 적용함으로써 training set에 나타나지 않은 요소가 있어도 계산된 확률이 0이 되는 문제를 해결할 수 있다.
<br />
### Log probability
많은 경우에 확률에 Log 함수를 적용하여 확률 및 머신러닝 문제를 해결하는  볼 수있다. NBC에서는 식 (9)와 (10) 같은 작은 값을 갖는 확률들이 계속 곱해져서 전체 확률이 컴퓨터상에서 표현이 불가능할 정도로 작아지는 문제를 해결하기 위해 Log 함수를 이용한다. 이외에도 Log 함수는 확률과 관련된 문제를 풀 때 많은 이점을 갖고 있는데, 크게 세 가지로 정리하면 다음과 같다.

<ul>
	<li>곱으로 표현되는 식에 Log 함수를 적용하면, 식이 합으로 표현되기 때문에 작은 값을 갖는 확률들이 계속 곱해져서 전체 확률이 컴퓨터상에서 표현이 불가능할 정도로 작아지는 문제를 해결할 수 있다.</li>
    <li>곱을 합으로 표현하는 Log 함수의 특성은 데이터의 손실을 방지하는 것뿐만 아니라, 식의 계산 또는 유도시에 식을 더욱 이해하기 쉬운 형태로 변형시켜준다.</li>
    <li>함수의 정의에 의해 Log 함수는 단조증가함수 (monotonically increasing function)이기 때문에 원래의 확률에 Log 함수를 적용하여도 그 결과는 변하지 않는다.</li>
</ul>

식 (5)에 Log 함수를 적용하면, 식 (5)는 아래의 식 (11)로 변환된다.

$$
\ln P(e_1|c_i) + \ln P(e_2|c_i) + ... + \ln P(e_M|c_i) + \ln P(c_i)
$$

NBC를 구현할 때는 식 (5)가 아니라 식 (11)을 이용하여 확률을 계산하도록 구현한다.
<br />
### Classification problem example
이 예제에서는 앞의 표 1에 있는 training set을 이용하여, 문장 (단어들의 나열)이 주어졌을 때 해당 입력이 C/C++ 또는 Java에 속하는지를 판별하는 NBC의 동작 과정을 설명한다. 예를 들어, *"It has struct, int, float"*이라는 입력 $$s$$가 입력되었을 때, NBC는 다음과 같이 동작한다.
<br class="br-small" />
<div class="title-small">1) Conditional probability 계산</div>
앞의 표 1에서 주어진 training set에는 C/C++, Java라는 두 개의 class가 명시되어 있다. conditional probability를 계산하기 전에 입력 $$s$$에 대해 적절한 전처리를 하여 입력 $$s$$를 [it, has, struct, int, float]라는 5차원 벡터로 변형하였다고 가정한다. 이러한 전처리 또한 매우 활발히 연구되고 있는 분야이며, 적절한 전처리를 머신러닝 모델의 성능을 크게 향상시킬 수 있다. 전처리된 데이터에 대해 식 (11)을 이용하여 $$P(c_i|\mathcal{D})$$를 다음과 같이 계산한다. 이 예제에서 $$c_1$$은 C/C++, $$c_2$$는 Java를 의미한다.

$$
\begin{aligned}
\log P(c_1|\mathcal{D}) \approx & \enspace \log P(it|c_1) + \log P(has|c_1) + \log P(struct|c_1) \\
+ & \enspace \log P(int|c_1) + \log P(float|c_1) + \log P(c_1) \\
= & \enspace \log(\frac{0 + 1}{8 + 11}) + \log(\frac{0 + 1}{8 + 11}) + \log(\frac{2 + 1}{8 + 11}) \\
+ & \enspace \log(\frac{1 + 1}{8 + 11}) + \log(\frac{1 + 1}{8 + 11}) + \log(\frac{2}{5}) \\
\fallingdotseq & \enspace -13.1536
\end{aligned}
$$

<br class="br-small" />

$$
\begin{aligned}
\log P(c_2|\mathcal{D}) \approx & \enspace \log P(it|c_2) + \log P(has|c_2) + \log P(struct|c_2) \\
+ & \enspace \log P(int|c_2) + \log P(float|c_2) + \log P(c_2) \\
= & \enspace \log(\frac{0 + 1}{9 + 11}) + \log(\frac{0 + 1}{9 + 11}) + \log(\frac{0 + 1}{9 + 11}) \\
+ & \enspace \log(\frac{2 + 1}{9 + 11}) + \log(\frac{1 + 1}{9 + 11}) + \log(\frac{3}{5}) \\
\fallingdotseq & \enspace -13.6977
\end{aligned}
$$

<div class="title-small">2) Conditional probability 비교</div>
앞에서 계산한 log-conditional probability 중에서 가장 큰 값은 C/C++에 대한 값인 -13.1536이므로, 주어진 문장 *"It has struct, int, float"*은 C/C++로 분류된다.
<br />
#### References
[1] Richard O. Duda, Peter E. Hart, David G. Stork (2001). *Pattern Classification (2nd edition).* John Wiley & Sons.
<br class="br-small" />
[2] Harry Zhang, *"The Optimality of Naive Bayes"*, Proceedings of the Seventeenth International Florida Artificial Intelligence Research Society Conference, Miami Beach, Florida, USA, 2004.
<br class="br-small" />
[3] Wenyuan Dai, Gui-Rong Xue, Qiang Yang and Yong Yu, *"Transferring Naive Bayes Classifiers for Text Classification"*, Proceeding
AAAI'07 Proceedings of the 22nd national conference on Artificial intelligence, Vol. 1, pp. 540-545, 2007.
<br />
