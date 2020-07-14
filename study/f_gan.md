# f-GAN: Training Generative Neural Samplers using Variational Divergence Minimization
* NIPS 2016
* Sebastian Nowozin, Botond Cseke, Ryota Tomioka
* Microsoft Research Cambridge
* 제목 설명: VDM 방식으로 Generative Neural Sampler를 학습하는 방법론에 대해 다룸. 
    * 이런 접근 방식을 f-GAN으로 이름 짓겠음.

## Abstract
* Generative neural sampler
    * 뉴럴 네트웍으로 구현된 확률 모델
    * Random Input Vector를 받아서 뉴럴 네트웍의 Parameter가 정의하는 확률 모델에서 Sample을 추출
    * 매우 유용하지만, 일종의 Blackbox같은 상태라 Marginalization이나 Likelihood를 계산 할 수 없다.
* Generative Adversarial Training
    * 추가적인 Discriminator 네트웍을 통해 Neural Sampler의 Training을 가능하게 했다.
* 저자들은 Generative Adversarial Approach가 더 일반적인 Variataional Divergence Estimation Approach에 Special Case라는 걸 보일 계획임
    * 특히 아무 f-divergence 함수들을 사용해 Generative Neural Sampler를 training할 수 있음을 보일 계획

* 요약
    * GAN이 Variational Divergence Estimation Approach 중 하나에 불과하다.
      * f-Divergence를 Jensen-Shannon이 되도록 정한 것이다..
      * f-div family이기만 하면.. 머든 된다. KL이든 머든
      * 그러니 앞으로 GAN의 Objective 바꿔보는 정도는 이 논문이 Coverage 안에 있다..
        * f-div 아닌 거라면 모르겠지만...

## Introduction
* Probabilistic generative models
    * describe a probability distribution over a given domain $\chi$
    * 주로 어떤 data set의 확률 분포를 학습했을 것.
* Generative Model Q from a class $\mathcal{Q}$ of possible models
    * Sampling: Produce a sample from Q
    * Estimation: Given samples from unkown true distribution P, find best $Q \in \mathcal {Q}$
    * Point-wise likelihood evaluation: Given a sample x, evaluate the likelihood Q(x)

* Generative nueral sampler는 Neural Network으로 구현된 Generative Model Q라고 보면 되겠다.
    * 특징은, exact sampling과 approximate estimation(Estimation=Training, GAN)이 가능함. 
    * GAN의 Generator나, VAE의 Generator가 다 여기에 해당된다.

* Ian Goodfellow가 제안한 원래의 GAN을 다시 살펴보자.
    * Original GAN 에서 제안한 Neural Sampler의 Estimation은 Jensen-Shannon divergence의 Approximate minimization이라는 것이 해당 논문이 증명되어있다.
    * $D_{js} (P || Q) = \frac{1}{2} D_{KL} (P || \frac{1}{2}(P+Q)) + \frac{1}{2} D_{KL} (Q || \frac{1}{2}(P+Q))$
    * 여기서, KL은 Kullback-Leibler divergence다.
    * GAN training에서 핵심은 사실 Disciminator를 도입해서 같이 Optimization하는 점이다.
    * $D_{js}$는 적절한 divergence measure이므로 training sample이 충분하고 model class $\mathcal{Q}$가 충분히 P를 모델링할 수 있다면 잘 Approximation할 수 있다.

* 저자들은 GAN의 Principle이 사실 더 Generalize가 가능함을 보임.
    * Variational Divergence Estimation (by Nguyen et al.[25]) 로 GAN의 Training Objective를 유도할 것임.
    * 그리고 어떤 Objective를 쓸 수 있는지 arbitrary f-divergence를 통해 소개할 것임.

* 실제 논문에서 정리하는 Contributions
    * Derive the GAN training objectives for all f-divergences
    * Simplify the saddle-point opimization procedure
    * Experimental insight into which divergence function is suitable for estimating ...

## Method

### The f-divergence family
* KL같이 2개의 Probability Distribution의 Difference를 Measure하는 것들을 Statistical Divergence라고 부르는데, f-divergence라고 하는 잘 알려진 큰 Family가 있다.
* P, Q 두개의 확률 분포가 주어지면, probability density function이 p, q일때, f-divegence는 아래와 같다.
    * $D_f(P||Q) = \int_{\chi} q(x) f(\frac{p(x)}{q(x)}) dx$
    * 이때, $f: \mathbb{R}_{+} \rightarrow \mathbb{R}$, Convex, Lower-semicontinuous function이고, f(1)=0을 만족하면 된다.
    * Lower-Semi continuous는 일부 Discontinuous도 괜찮은데 다만, minimization direction으로 만큼은 continuous 해야만 한다는 조건이다. 최저값과 연결은 되어 있어야 된다는 정도로 해석하면 될거 같다.
    * f의 예시는 Table.1 참조

### Variational Estimation of f-divergence
* $D_f$가 다 좋은데 어찌되었든 구할려면 $p(x)$든 $q(x)$든 분포의 형태(formula)를 알아야 한다.
* 하지만 Nueral Sampler는 알다시피 black-box고 sampling만 가능하다. 결국 f-div를 못구한다는 의미가 ?
* 이걸 구할 수 있는 방법을 제시한 것이 Nguyen et al.[25]이고, 그 방법 이름이 Variational Estimation 되겠다.
* [25]가 실제로 한 일은 derive a general variation method to estimate f-divergences given only samples from P and Q.
* **f-gan 저자들은 이걸 확장해서 Nerual Sampler의 Parameter를 추정하는데 쓰겠다는 것이다**
* 요 Method를 variational divergence minimization(VDM)이라고 부르겠다.
* 이 과정에서 Original GAN의 방법론이 VDM의 Special Case임을 확인할 것이다.


#### Nguyen's divergence estimation
* **Frenchel conjugate:** every convex, lower-semicontinuous function f의 convex conjugate function $f^*$
    * $f^*(t) = \sup_{u \in dom_f} \{ ut - f(u) \}.$
    * 이러면 $f^*$ 도 convex, lower-semicontinuous고, $(f,f^*)$는 dual 관계라서 $f^{**}=f$다.
    * 결국, $f(u) = \sup_{t \in dom_{f^*}} \{tu - f^*(t) \}$
    * 자세한건 Convex Analysis를 공부해야 하는 거 같다. 그림은 한번 보고 가자.
* [25]는 위식을 variational lower bound에 적용한 것임.
* 해보자
    * $D_f(P||Q) = \int_{\chi} q(x) f(\frac{p(x)}{q(x)}) dx$
    * $= \int_{\chi} q(x) \sup_{t\in dom_{f^*}} \{t \frac{p(x)}{q(x)} - f^* (t) \} dx$
    * $\geq \sup_{T \in \mathcal{T}} \left(\int_{\chi} p(x) T(x) dx - \int_\chi q(x) f^*(T(x)) dx \right)$
    * $=\sup_{T \in \mathcal{T}} \left( \mathbb{E}_{x \sim P} [T(x)] - \mathbb{E}_{x \sim Q} [f^* (T(x))] \right)$
        * 이때, $\mathcal{T}$는 $\mathcal{T}: \chi \rightarrow \mathbb{R}$인 아무 함수들이다.
* [25]에 의하면, Lower bound의 variation을 취해서 계산해보면, 적절한 조건하에 Lower bound는 Tight하고, 그때,
    * $T^* (x) = f' \left( \frac{p(x)}{q(x)} \right)$
    * 가 되더라... 이 조건들이 결국 f를 선택하는 가이드 라인이 된다.
* reverse KL을 예로 살펴보면, $f(u)=-log(u)$ 인데, $T^* (X) = -q(x)/p(x)$가 된다. 자주사용하는 divergence들의 f와 T의 pair는 Table.5에 있다.

#### Variational Divergence Minimization (VDM)
* 이제 우리가 원래 관심있는 문제, "estimate a generative model Q given a true distribution P"로 돌아오자.

* Original GAN과 기본적인 Setting은 동일하다.
    * 2 neural networks Q and T
    * Q takes a random vector and outputs a sample
    * Q is parameterized by $\theta$
    * T is a variational function, taking a sample and returning a scalar
    * T is parameterized by $\omega$
* Finding $Q_\theta$ = finding a saddle-point of the f-GAN objective
    * $F(\theta, \omega) = \mathbb{E}_{x \sim P} [T_\omega (x)] - \mathbb{E}_{x \sim Q_\theta}[f^* (T_\omega(x))].$

#### Representation for the Variational Function
* 한가지 주의할점은 $f^*$의 domain이 함수마다 좀 다를 수 있다는 점이다. 예를 들면 어떤함수는 [0,$\infty$), (-$\infty$, +$\infty$), 또는 [-1, 1] 인 경우도 있다.
* 원래 T는 arbitrary 함수였지만, $f^*$를 위해 T의 range가 $f^*$의 domain에 포함되도록 Treatment가 필요하다.
* 그래서 $T_\omega (x) = g_f(V_\omega(x))$의 형태로 변경한다. $g_f$는 일종의 activation function(sigmoid, tanh, ...) 되어 T의 range를 조절한다.
* 최종 f-GAN objective는 다음과 같이 된다.
    * $F(\theta, \omega) = \mathbb{E}_{x \sim P} [g_f(V_\omega(x))] + \mathbb{E}_{x \sim Q_\theta}[-f^* (g_f(V_\omega(x)))].$
* 위 식과 원래 GAN objective를 나란히 살펴보자.
    * $F(\theta, \omega) = \mathbb{E}_{x \sim P} [logD_\omega (x)] + \mathbb{E}_{x \sim Q_\theta}[log(1-D_\omega(x))].$
    * Discriminator의 최종 activation을 sigmoid로 할 경우 $D_\omega (x) = 1/(1+e^{-V_\omega (x)})$ 가 되고 이는 $g_f (v) = -log(1+e^{-v})$로 정의한 f-GAN과 같다.

* Table1~5를 잘 살펴보면서 대입해보면 동일하게 유도 가능하다.

## Algorithms for variational divergence minimization (VDM)
* numerical method to find saddle points of the objective
    * alternating method (by Goodfellow)
    * single-step method (by f-GAN)
* alternating method = double-loop method
    * internal loop는 divergence lower bound로 tighten
    * outer loop는 generator를 향상시킴
* alternating method가 대개 single inner step만 쓰는데 그냥 잘된다. 그래서 저자들은 아예 single-step을 제안해본다.
* Algorithm 1 보자.
    * Gradient는 한번만 계산
    * $\omega$ 방향으로는 maximize
    * $\theta$ 방향으로는 minimize
* Ian Goodfellow는 사실 Practical하게는 아래 방식을 권유하고 현재 대부분의 GAN 구현이 아래를 따른다.
    * min $\mathbb{E}_{x \sim Q_\theta} [log (1-D_\omega(x))]$ 이거를 아래로...
    * max $\mathbb{E}_{x \sim Q_\theta} [logD_\omega (X)]$

* f-GAN관점에서 생각하면, 아래라고 생각하면 되겠다.
    * $\theta^{t+1} = \theta^t + \eta \nabla_\theta \mathbb{E}_{x \sim Q_\theta^t} [g_f(V_\omega^t (x))]$
