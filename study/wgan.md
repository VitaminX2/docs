# Wasserstein GAN

* Martin Arjovsky, Soumith Chintala, Leon Bottou
* Courant Institute of Mathematical Science, Facebook AI Research
* ICML 2017
* additional ref:
    * http://www.alexirpan.com/2017/02/22/wasserstein-gan.html 
    * https://vincentherrmann.github.io/blog/wasserstein/

## Abstract
* GAN의 Training의 Stability 향상, 
* Mode Collapse 제거
* 의미있는 학습 곡선 제공
* 확률분포함수간의 거리이론을 통한 최적화에 대한 이론적 토대제공

## Introduction
* Unsupervised Learning 또는 Learn a probability distribution를 하는 고전적인 접근은 곧 learn a probability density다.
* 보통 density에 대한 parametric family를 정의한뒤, 주어진 데이터에 대해서 likelihood를 최대화하는 방식으로 Parameter를 Estimation한다. (Maximum Likelihood)

* Real data example을 $\{ x^{(i)} \}^m_{i=1}$라 할때,

    > $\max_{\theta \in \mathbb{R}^d} \frac{1}{m} \sum^m_{i=1} log P_\theta(x^{(i)})$

    > 형태로 표현한다.

* 만약 real data distribtuion $\mathbb{P}_r$이 density로 표현 가능하고(admit), $\mathbb{P}_\theta$가 parameterized density $P_\theta$의 distribution이라면, Asymtotically Kullback-Leibler Divergence $KL(\mathbb{P}_r||\mathbb{P}_\theta)$를 minimize하는 것과 마찬가지다.

    > $\lim_{N\rightarrow\infty} \max_\theta \frac{1}{N} \sum^N_{i=1} log P_\theta(x^{(i)})$

    > $= \max_\theta \int P_r(x) log P_\theta (x) dx$

    > $=\min_\theta \{ \int P_r(x) log P_r(x) dx - \int P_r(x) log P_\theta(x) dx \}$

    > $=\min_\theta KL(\mathbb{P}_r || \mathbb{P}_\theta)$

    > $KL(\mathbb{P}_r||\mathbb{P}_\theta) = \int P_r log \frac{P_r}{P_\theta} dx$ 

    > *data ${x^{(i)}}$가 무한히 많이 들어오면 결국 $P_r$을 대체하여 비슷한 형태가 된다는 의미로 해석*

* 이게 성립하려면 일단, model density $P_\theta$가 존재해야 한다. 그런데 우리가 다루는 저차원 manifolds에서는 이게 꼭 그렇다고 보장이 안된다. 그래서 model manifold하고 true distribution간에 유의미한 intersection이 없기 쉽다. 
    * 결과적으로 KL distance?는 정의되지 않는다거나, 
    * 또는 값이 무한하다? (*동치적 표현인듯*)

* 이에 대한 전형적인 해결책이 model distribution에 noise term을 첨가하는 것이다. 이것이 기존의 거의 모든 학습기반 Generative Model들이 noise 항을 포함하는 이유다.
    * 가장 단순한 예로, Gaussian noise with relatively high bandwidth를 가정하여 모든 예제를 커버한다.
    * 물론, 이러한 접근은 결국 image 생성의 경우 Blur한 영상을 만들어서 Quality를 떨어뜨린다.
    * Wu et al. 2016의 경우를 예로 들면, Maximizing Likelihood시에 0.1 근방의 noise를 Generate 영상에 추가한다. pixel이 [0,1]로 normalize된 것을 생각하면 꽤 큰 Noise다.

* 저자들은 존재하는지도 모르는 $\mathbb{P}_r$의 density를 estimate하기 보다는, a random variable $Z$와 distribution $p(z)$를 정의한뒤, Parametric funtion $g_\theta : \mathcal{Z} \rightarrow \mathcal{X}$을 통과시켜서 Sample을 Generation 시키고 이게 특정 분포 $\mathbb{P}_\theta$를 따르도록 하겠다. *많이 듣던 스토리죠?*
    * $\theta$를 조절해서 분포를 변화시킬수 있고, $\mathbb{P}_r$에 가깝게 만들 수 있다.
    * density 와 달리 distribution을 저차원 manifold에 가둘 수 있다.
    * Sample 생성이 용이한 것이 종종 density value를 아는 것보다 훨씬 유용하다.
        * 일반적으로, 임의의 고차원 밀도 함수가 주어졌을때, 샘플을 생성하는 것은 Computationally Difficult하다.

* VAE랑 GAN이 이러한 접근에 대한 대표적인 예시다.
    * VAE는 Likelihood 근사에 초점을 맞추고 있어서, 표준적인 모델이 갖고 있는 한계를 그대로 공유한다. 즉 추가적인 noise term이 사용된다.
    * GAN은 JSD, f-divergence 등에 의해 훨씬 더 flexible하다. 하지만 Training이 까다롭고 불안정하다. (왜그런지는 Arjovsky & Bottou 2017)

* WGAN 논문은 model distribution하고 real distribution이 얼마나 가까운지 measure하는 다양한 방법에 집중할 것이다.
    * 결국 distance or divergence $\rho(\mathbb{P}_\theta, \mathbb{P}_r)$을 정의하는 다양한 방법에 집중한다는 얘기
    * 이러한 distance간의 결정적인 차이는 확률 분포 Sequence들의 convergence가 끼치는 영향에 있다.
        * A sequence of distributions $(\mathbb{P}_t)_{t\in\mathbb{N}}$ 가 converge 한다 $\Leftrightarrow$ $\rho(\mathbb{P}_t, \mathbb{P}_\infty)$ 가 zero로 접근하는 a distribution $\mathbb{P}_\infty$ 이 존재한다. 따라서   $\rho$가 어떻게 정의되었는가가 관련있다. 
        * 엄밀하진 않지만, a distance $\rho$ 가 a sequence of distribution의 converge를 더 쉽게 만든다면 이를 weaker topology를 induce한다고 표현한다.
        * 좀더 정확하게는, $\rho$하의 set of convergent sequences가 $\rho'$ 하의 set of convergent sequence의 superset 이면, $\rho$가 induce한 topology가 $\rho'$ 이 induce한 topology보다 weaker 하다.
* 또한, $\theta$를 최적화하기 위해선, model distribution $\mathbb{P}_\theta$를 정의할때, mapping $\theta \mapsto \mathbb{P}_\theta$가 Continuous하는게 좋다.
* 여기서 continuity가 의미하는 것은 
    * a sequence of parameters $\theta_t$ 가 $\theta$ 로 converge하면, the distributions $\mathbb{P}_{\theta_t}$ 도 $\mathbb{P}_\theta$ 로 converge함을 의미한다.
    * 아까와 마찬가지로, convergence of the distributions $\mathbb{P}_{\theta_t}$ 는 어떤 방식으로 분포간의 distance를 계산하는가에 달려있다.
    * 이 distance가 weak할수록, distribution의 converge가 용이해지므로, $\theta$-space 에서 $\mathbb{P}_\theta$-space로 가는 Continuous Mapping의 정의가 용이해진다.

* Contributions of this paper
    * Theoretical analysis of EM distance in the context of learning distributions
    * define WGAN that minimizes a reasonable and efficient approximation of the EM distance
    * Show the corresponding optimization problem is sound.
    * Empirically show that WGAN cure the main training problems of GANs. 
        * do not require maintaining a carefule balance in training of the discriminator and the generator
        * do not requre a carefule design of the network architecture
        * reduces the mode dropping
        * continuously estimate the EM distance by training the discriminator to optimality

## Different Distances
* Notations
    * $\mathcal{X}$ : a compact metric set
        * ex.) the space of images $[0, 1]^d$
    * $\sum$ : the set of all the Borel subsets of $\mathcal{X}$
    * $\textbf{Prob}(\mathcal{X})$ : the space of probability measures defined on $\mathcal{X}$

* Elementary distance & divergence between 2 distributions $\mathbb{P}_r, \mathbb{P}_g \in \mathbf{Prob} (\mathcal{X})$
    * The Total Variation (TV) Distance

        > $\delta(\mathbb{P}_r, \mathbb{P}_g) = \sup_{A \in \sum} | \mathbb{P}_r(A) - \mathbb{P}_g(A)|$

    * The Kullback-Leibler (KL) Divergence

        > $KL(\mathbb{P}_r||\mathbb{P}_g) = \int \log ( \frac{P_r(x)}{P_g(x)} ) P_r(x) d \mu (x)$

        > Assymetric한 특성과 $P_g(x)=0$ 이고 $P_r(x)>0$일때 infinity가 됨.

        > $\mathbb{P}_r$, $\mathbb{P}_g$ 모두 $\mathcal{X}$에서 정의된 Measure $\mu$에 대하여 density를 admit함을 가정한다.

        > **Note on density admit**
        > * A probability distribution $\mathbb{P}_r \in Prob(\mathcal{X})$ admits a density $P_r(x)$ with respect to $\mu$, that is, $\forall A \in \sum$, $\mathbb{P}_r(A)=\int_A P_r(x) d\mu (x)$, if and only if it is absolutely continuous with respect to $\mu$, that is, $\forall A \in \sum$, $\mu(A) = 0 \Rightarrow \mathbb{P}_r(A)=0$ 

    * The Jensen-Shannon (JS) Divergence

        > $JS(\mathbb{P}_r, \mathbb{P}_g) = KL(\mathbb{P}_r || \mathbb{P}_m) + KL(\mathbb{P}_g || \mathbb{P}_m)$

        > $\mathbb{P}_m = (\mathbb{P}_r + \mathbb{P}_g)/2$
        
        > symmetric하고, $\mu=\mathbb{P}_m$으로 선택하면 항상 정의가능하다.

    * The Earth-Mover (EM) Distance or Wasserstein-1

        > $W(\mathbb{P}_r, \mathbb{P}_g)=\inf_{\gamma \in \Pi(\mathbb{P}_r, \mathbb{P}_g)} \mathbb{E}_{(x,y) \sim \gamma} [ ||x-y|| ]$

        > $\Pi(\mathbb{P}_r, \mathbb{P}_g)$ : the set of all joint distributions $\gamma(x,y)$ whose marginals are respectively $\mathbb{P}_r$ and $\mathbb{P}_g$

        > $\gamma(x,y)$ 가 가리키는 것은 얼마나 많은 "mass"를 x에서 y로 옮기면 $\mathbb{P}_r$를 $\mathbb{P}_g$로 Transform할 수 있는가다. *그래서 이름이 Earth-Mover(흙옮기기)*

        > 이때, EM Distance는 최적 "Cost"를 말한다. *수많은 moving 방법중에서 최소 Cost인 것*

* 논문 제목도 그렇고, 내용도 그렇고 EM이 다른 distance나 divergence보다 우월함을 확인하는 과정이다.

* EM은 Converge하는데 다른 것은 Converge 하지 않는 Simple Sequences를 살펴보자.
    * $Z \sim U[0,1]$: Uniform distribution on the unit interval
    * $\mathbb{P}_0$: distribution of $(0,Z) \in \mathbb{R}^2$
        * 원점을 지나는 Uniform 수직선
    * $g_\theta(z)=(\theta, z)$, $\theta$는 실수 Parameter
* 이때, 각 divergence를 살펴보면,
    * $W(\mathbb{P}_0, \mathbb{P}_\theta) = |\theta|$
    * $JS(\mathbb{P}_0, \mathbb{P}_\theta)$
        * $= log2$ if $\theta \neq 0$
        * $= 0$ if $\theta = 0$
    * $KL(\mathbb{P}_0 || \mathbb{P}_\theta))$
        * $=+\infty$ if $\theta \neq 0$
        * $=0$ if $\theta = 0$
    * $\delta(\mathbb{P}_0, \mathbb{P}_\theta)$
        * $=1$ if $\theta \neq 0$
        * $=0$ if $\theta = 0$

* 척봐도 W 빼고는 다 Discontinuous하다.
* 특히, $\theta_t \rightarrow 0$에 따라 EM은 $(\mathbb{P}_{\theta_t})_{t\in\mathbb{N}}$은 $\mathbb{P}_0$ Converge하는 것이 확인되지만 나머지는 그렇지 않다.
* 그림 1을 살펴보자.

* Theorem1. $\mathbb{P}_r$이 $\mathcal{X}$ 위에서 fixed distribution이고, $Z$가 $\mathcal{Z}$ 에서의 random variable이고, $\mathbb{P}_\theta$가 $g_\theta(Z)$의 distribution일때, $g:(z,\theta) \in \mathcal {Z} \times \mathbb{R}^d \mapsto g_\theta(z) \in \mathcal{X}$ 이면,
    * if g is continuous, so is W()
    * if g is locally Lipschitz and satisfies regularity assumption 1, then W() is continuous everywhere, and differentiable almost everywhere
    * aboves are false for JS, and all KLs

* 결론적으로 minimizing EM 을 통한 학습이 neural networks에 make sense하다. 

    > **Note on Lipschitz**
    > * Given two metric spaces (X, dx) and (Y, dy), where dx denotes the metric on the set X and dy is the metric on set Y, a function $f:X \rightarrow Y$ is called Lipschitz continuous if there exists a real constant $K \geq 0$ such that, for all $x_1$ and $x_2$ in X $d_Y(f(x_1), f(x_2)) \leq Kd_X(x1, x2)$ 
    > * any such K is referred to as a Lipschitz constant for the function f
    > * A real-valued function $f:R\rightarrow R$ is called Lipschitz continuous if there exists a positive real constant K such that, for all real $x_1$ and $x_2$, $|f(x_1) - f(x_2)| \leq K |x_1 - x_2|$
    > * A function is called locally Lipschitz continuous if for every x in X there exists a neighborhood U of x such that f restricted to U is Lipschitz continuous.

* Corollary1. 

    > $g_\theta$ : any feed forward neural network parameterized by $\theta$, 

    > $p(z)$ a prior over z such that $\mathbb{E}_{z \sim p(z) } [|z|] < \infty$

    > Then, assumption 1 is satisfied and therefore $W(\mathbb{P}_r, \mathbb{P}_\theta)$ is continuous everywhere and differentiable almost everywhere

    > **Note on feed-forward nueral net and Lipschitz**
    > * By a feedforward neural network we mean a function composed of affine transforms and component-wise Lipschitz nonlinearity (sigmoid, tanh, elu, softplus, etc).
    > * A similar but more techincal proof is required for ReLUs.

* 이런 모든 것들로부터 EM이 훨씬 괜찮은 Cost Function이라는 걸 알수있다.

* Theorem2. Relative strength of the topologies induced by these distances and divergences (KL > JS > TV > EM)
    * $\mathbb{P}$ a distribution on a compact space $\mathcal{X}$
    * $(\mathbb{P}_n)_{n\in\mathbb{N}}$ a sequence of distributions on $\mathcal{X}$
    * Considering all limits as $n \rightarrow \infty$
    1. the following statesments are equivalent
        * $\delta(\mathbb{P}_n, \mathbb{P}) \rightarrow 0$
        * $JS(\mathbb{P}_n, \mathbb{P}) \rightarrow 0$
    1. the following statements are equivalent
        * $W(\mathbb{P}_n, \mathbb{P}) \rightarrow 0$
        * $\mathbb{P}_n \rightarrow^{D} \mathbb{P}$ where $\rightarrow^D$ represents convergence in distribution for random variables.
    1. $KL(\mathbb{P}_n || \mathbb{P}) \rightarrow 0$ or $KL(\mathbb{P}||\mathbb{P}_n) \rightarrow 0$ $\Rightarrow$ the statements in (1)
    1. The statements (1) $\Rightarrow$ the statements in (2)

* 위 정리는 해석하자면 KL, Reverse-KL, TV, JS로 converge되는 모든 분포는 Wasserstein divergence로도 converge 된다는 뜻이다.

* 최종적으로 이 정리를 통해 KL, JS, TV, EM중에 EM이 최고라는 의미다.


## Wasserstein GAN
* 하지만, EM은 optimization form($\inf$)으로 정의되어 있어서 highly intractable하다.
* 여기서 Kantrovich-Rubinstein Duality(Villani 2009, 1000page짜리 Reference, 저자는 필드메달 수상자)가 등장
* $W(\mathbb{P}_r, \mathbb{P}_\theta) = \sup_{||f||_{L\leq 1}} \mathbb{E}_{x\sim \mathbb{P}_r}[f(x)] - \mathbb{E}_{x\sim\mathbb{P}_\theta}[f(x)]$
    * 여기서 sup는 모든 1-Lipschitz functions $f:\mathcal{X} \rightarrow \mathbb{R}$에 대해서이다.
* 논문을 이걸 풀기위한 작업으로 1-Lipschitz를 K-Lipschitz로 Relaxation함.
* 만약 $||f||_L \leq 1$을 $||f||_L \leq K$로 바꾼다면, $K\cdot W(\mathbb{P}_r, \mathbb{P}_g)$로 귀결된다.
* 따라서, parameterized family of functions $\{f_w\}_{w\in \mathcal{W}}$ 가 있고, 모두 K-Lipschitz 라면, 다음 문제를 푸는 거라고 볼수 있다.
* $\max_{w \in \mathcal{W}} \mathbb{E}_{x\sim \mathbb{P}_r}[f_w(x)] - \mathbb{E}_{z\sim p(z)}[f_w(g_\theta(z))]$
* (강한 가정이긴하나) Kantrovich-Rubinstein duality의 sup가 위식의 어떤 $w\in\mathcal{W}$에 의해서 만족되었다고 보면, 요 과정이 $W(\mathbb{P}_r, \mathbb{P}_\theta)$ 를 multiplicative constant까지 계산하는 것으로 볼 수 있다.
* 또한 $W(\mathbb{P}_r, \mathbb{P}_\theta)$ 를 미분하는 것도 $\mathbb{E}_{z \sim p(z)}[\nabla_\theta f_w (g_\theta(z))]$를 Estimate한 것을 Back-Propagration하는 것으로 고려해볼만 하다.
* 지금까지는 다 직관적으로 설명했지만, 모두 optimality assumption하에 증명할 것이다.
    * 증명은 다 Appendix에 있음

* 정리3.
    * $\mathbb{P}_r$ any distribution
    * $\mathbb{P}_\theta$ the distribution of $g_\theta(Z)$ with $Z$ a random variable with density $p$ 
    * $g_\theta$ a function satisfying assumption 1
    * Then, there is a solution $f:\mathcal{X} \rightarrow \mathbb{R}$ to the problem
    * $\max_{||f||_L \leq 1} \mathbb{E}_{x \sim \mathbb{P}_r}[f(x)] - \mathbb{E}_{x \sim \mathbb{P}_\theta}[f(x)]$
    * and we have,
    * $\nabla_\theta W(\mathbb{P}_r, \mathbb{P}_\theta) = - \mathbb{E}_{z\sim p(z)}[\nabla_\theta f(g_\theta(z))]$
    * when both terms are well-defined.

* 전형적인 GAN Technique으로 Training하면 된다.
* 단, $\mathcal{W}$ 가 compact하므로, 모든 $f_w$는 K-Lipschitz 하고 $\mathcal{W}$에만 의존하며, 개별 weight와는 무관하다.
* 따라서, W를 크게 상관없는 scaling factor와 $f_w$의 capacity까지 근사한다.
* $w$가 compact space에 있도록 하기위해 단순하게 weight를 fixed box로 clamp한다. 

    > $\mathcal{W} = [-0.01, 0.01]^l$

* 이쯤에서 algorithm 1을 보면 좋을거 같다.

* EM distance가 continuous하고 differentiable하다는 사실은 곧 우리가 critic(discriminator)를 optimality까지 train할수 있다는 의미다.
* 논리는 단순한데, critic을 더 train 할수록 더 optimal하다. (overfitting 아니고...)
* JS의 discriminator 경우는 gradient가 reliable 할수록 좋아지는데, JS가 locally saturated 해서 vanishing gradient 현상으로 true gradient는 0이 되어버린다.
    * Figure 1 재참조
* Figure3에서 GAN의 discriminator와 WGAN의 critic을 optimal까지 train한 경우를 보여준다.
    * GAN의 discriminator는 매우 빨리 fake와 real을 구별하게 되지만 gradient는 vanishing하고 generator는 학습이 어렵게 된다.
    * 반면에 WGAN의 critic은 saturate할수없고, 선형함수에 converge하여 clean한 gradient를 everywhere에서 제공한다.
    * The fact that we constrain the weights limits the possible growth of the function to be at most linear in different parts of the space, forcing the optimal critic to have this behavior.
* 아마 더 중요한점은 critic을 optimal까지 training할 수 있기때문에 mode-collapse가 불가능하게 만들 수 있다는 점이다.
    * mode-collapse는 고정된 discriminator에 대한 optimal **generator**는 a sum of deltas on the points the discriminator assigns the highest values 인데서 기인한다. (*Unrolled GAN에서 다룬 내용*)




---
This document is created and maintained by Nahyup Kang(nhkang@gmail.com, nahyup.kang@samsung.com)