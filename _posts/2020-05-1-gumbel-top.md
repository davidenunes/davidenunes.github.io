---
title: "Gumbel-Max Trick"
excerpt_separator: "<!--more-->"
sidebar: False
comments: true
classes: wide
categories:
  - neural networks
  - technical
  - sampling
  - probability
---

## How to vectorize sampling from a discrete distribution 
If you work with libraries such as [NumPy](https://numpy.org/),
[Jax](https://jax.readthedocs.io/en/latest/)
[Tensorflow](https://www.tensorflow.org/), or [PyTorch](https://pytorch.org/)
you (should) end-up writing a lot of vectorization code. Instead of using
control-flow operations (e.g. for loops), you write code that operates on an
entire set of values at once. Inputs and outputs of your functions are
multidimensional arrays or tensors. Lower-level libraries optimized for linear
algebra operations (such as matrix multiplications) make dramatic performance
improvements, especially when aided by modern hardware with direct support for
vector-based instructions.

In libraries like `NumPy` or `Tensorflow` sampling from a discrete distribution
without replacement is not vectorized because it requires bookkeeping. In other
words, sampling from a population depends on the values we already sampled.

So some time ago, I came across a set of re-parametrization tricks that allow us
to vectorize sampling from discrete distributions. This peaked my interest
because I was looking for a way to build stochastic neural networks where neuron
activations could be modelled with certain types of discrete distributions
parametrized by unnormalized log-probabilities.

To get a probability distribution from unconstrained vectors, usually we use the
_softmax_ function:

\\[ \sigma(y) = \frac{e^{y_i}}{\sum_{j=1}^N e^{y_j}} \\]

We would then use the resulting distribution to sample classes from it, for
example, using the [inverse transform
sampling](https://en.wikipedia.org/wiki/Inverse_transform_sampling): this takes
uniform samples of a number \\(u\\ \in \[0,1\)\\), interpreted as a probability,
and then returns the largest number \\(y\\) from the domain of the distribution
\\(P(Y)\\) such that \\(P(-\\infty < Y < y) \\le u \\). What we are doing is
randomly choosing a proportion of the area under the curve and returning the
number in the domain such that exactly this proportion of the area occurs to the
left of that number.

## The Gumbel-Max Trick

The **Gumbel-Max** trick can be used to sample from the previous discrete
distribution without marginalizing all the unnormalized log probabilities (i.e,
without \\(\sum_{j=1}^N e^{y_j}\\)). The procedure consists in taking the
unnormalized log probabilities \\(y_i\\), adding noise \\(z_i \sim~Gumbel(0,1)
\\) (i.i.d. from a Gumbel distribution)
and taking _arg max_. In other words:

\\[
    y = \underset{ i \in K }{\operatorname{arg max}} x_i + z_i    
\\]

This eliminates the need for the marginalization (which can be expensive for
high-dimensional vectors). Another consequence of doing away with the
computation of a _normalized_ probability distribution, is the fact that we
don't need to see all of the data before it can start partially sampling. Thus,
**Gumbel-Max** can be used for **weighted sampling from a stream**
(see [this](http://utopia.duth.gr/~pefraimi/research/data/2007EncOfAlg.pdf)). The
[Gumbel Distribution](https://en.wikipedia.org/wiki/Gumbel_distribution) is used
to model the distribution of the maximum (or the minimum) of a number of samples
of various distributions and, as it turns out, \\(z_i\\) is distributed
according to a softmax function \\(\sigma(y)\\).


<figure class="half">
    <a href="/assets/images/posts/gumbel_density.svg"><img src="/assets/images/posts/gumbel_density.svg"></a>
    <a href="/assets/images/posts/gumbel_cumulative.svg"><img src="/assets/images/posts/gumbel_cumulative.svg"></a>
    <figcaption>Gumbel Probability Density Function (PDF) and  Cumulative Distribution Function (CDF) respectively.</figcaption>
</figure>


Gumbel distribution with location parameter \\(\\alpha\\) and unit scale
parameter as the following Cumulative Distribution Function (CDF):

\\[
    F(z;\alpha) =  \exp \left[ -\exp\left[-(z-\alpha) \right]\right]
\\]

If \\(z_k\\) is the \\(k^{th}\\) element of the Gumbel distribution with
location \\(\alpha\_k\\), the probability that all of the other \\(z\_{k'\neq
k}\\) are less than \\(z_k\\) is:

\\[
    Pr(k > k' | z_k, \\{ \alpha_{k'}\\}\_{k'=1}^K) = \prod\_{k'\neq k} \exp \left[ -\exp\left[-(z_k-\alpha\_{k'}) \right] \right]
\\]

integrating the marginal distribution over \\(z_k\\) we have an integral which
has the closed form:

\\[
    Pr(k > k' | \\{ \alpha\_{k'}\\}) = \frac{\exp\left [\alpha\_k \right]}{\sum_{k'=1}^K \exp\left [\alpha\_{k'} \right]} 
\\]

which is exactly the softmax function. 

## The Gumbel-Top Trick
If we look at the **Gumbel-Max** trick as form of weighted reservoir sampling,
we can see that if instead of _arg max_ we take the _top-k_ args, we are
instead, sampling without replacement from the discrete categorical
distribution. We can call this the [**Gumble-Top**
trick](http://disq.us/p/1xber9v). 

## The Reparameterization Trick in Neural Networks
The [reparameterization trick](https://arxiv.org/pdf/1611.00712.pdf) allows for
the optimization of stochastic computation graphs via gradient descent. The
essence of the trick is to refactor each stochastic node into a differentiable
function of its parameters and a random variable with fixed distribution. As we
have seen previously, some closed formed densities have a simple
reparameterization. The choice of noise (e.g. Gumbel) gives the trick its name. 

Generally speaking, this trick consists in sampling from \\(p_\phi(x)\\) by
first sampling \\(Z\\) from some fixed distribution \\(q(z)\\) and then
transforming the sample using some function \\(g\phi(z)\\). This two step
process is precisely what we call _reparameterization trick_, and it is what
makes it possible to reduce the problem of estimating the gradient w.r.t.
parameters of a distribution to the simpler problem of estimating the gradient
w.r.t. parameters of a deterministic function. Once we reparameterized
\\(p_\phi(x)\\), one can now express the objective as an expectation w.r.t.q(z): 

\\[
    L(\theta, \phi)=\mathbb{E}\_{X \sim p\_{\phi}(x)} \left[ f\_{\theta}(X) \right]=\mathbb{E}_{Z \sim q(z)} \left[ f\_{\theta}\left(g\_{\phi}(Z) \right) \right]
\\]

This trick was introduced in the context of variational inference independently
by [\[Kingma & Welling 2014\]](https://arxiv.org/pdf/1312.6114.pdf), [\[Rezende
et al. 2014\]](https://arxiv.org/abs/1401.4082), and [\[Titsias & L
́azaro-Gredilla\]](http://proceedings.mlr.press/v32/titsias14.pdf).


## Implementation

### Sampling from the Gumbel Distribution
We can sample \\(z_i \sim \mathit{Gumbel(0,1)}\\) as follows:

\\[
    \begin{eqnarray}
    x_i \sim \mathit{Uniform(0,1)} \nonumber \\\\\\
    z_i = -\log(-\log(x_i)) \nonumber
    \end{eqnarray}
\\]

### NumPy Gumbel-Top 
To finish this post and get you an idea of how simple the vectorized procedure
is, here's an implementation using `NumPy`.

<!-- 
    """ top_k
    
    Args:
        x (ndarray): an array of elements  
        k (int): number of elements to return

    Returns:
        an array (ndarray[int]): with the indices of the largest elements in x
    """

    """ samples_k

    Args:
        logits: ndarray with unnormalized log probabilities
        k: number of classes to be sampled
    
    Returns:
        an array (ndarray[int]): with the indices of the sampled classes
    """
-->

```python
import numpy as np

def top_k(x, k):
    return np.argpartition(x, k)[..., -k:]

def sample_k(logits, k):
    u = np.random.uniform(size=np.shape(logits))
    z = -np.log(-np.log(u))
    return np_top_k(logits + z, k)
```


<!--
from tensorflow 
```
// Guesstimate of cost; 4*N*log(K) 
    // If K == N, assume the cost is N*log(K + 1).
```
or topk parallel partial sort http://on-demand.gputechconf.com/gtc/2009/presentations/1034-Multi-GPU-Recommendation-System.pdf
https://code.google.com/archive/p/ggks/
Facebook implementation over cuda using radixSelect:
https://github.com/facebook/fbcuda/blob/master/TopKElements.cuh
most implementations use radix select
https://github.com/facebook/fbcuda/blob/master/TopKElements.cuh

To get the top-k elements in sorted order in this way takes O(n + k log k) time.
-->

## References

### Blog Posts

1. Vieira, Tim. "Gumbel-Max-Trick". 2014
   [url](https://timvieira.github.io/blog/post/2014/07/31/gumbel-max-trick/). 

2. Ryans, Adam. "The Gumbel-Max Trick for Discrete Distributions"
   [url](https://lips.cs.princeton.edu/the-gumbel-max-trick-for-discrete-distributions/)
   
3. Mena, Gonzalo. "The Gumbel-Softmax Trick for Inference of Discrete
   Variables". 2017
   [url](https://casmls.github.io/general/2017/02/01/GumbelSoftmax.html)

### Papers

3. Maddison, Chris J., Daniel Tarlow, and Tom Minka. "A* sampling." _Advances in
   Neural Information Processing Systems_. 2014. [url](https://papers.nips.cc/paper/5449-a-sampling.pdf)

4. Kusner, Matt J., and José Miguel Hernández-Lobato. "Gans for sequences of
   discrete elements with the gumbel-softmax distribution. [url](https://arxiv.org/abs/1611.04051)

5. Jang, Eric, Shixiang Gu, and Ben Poole. "Categorical reparameterization with
   gumbel-softmax." [url](https://arxiv.org/pdf/1611.01144.pdf)

6. Efraimidis, Pavlos S., and Paul G. Spirakis. "Weighted random sampling with a
   reservoir." Information Processing Letters 97.5 (2006): 181-185.
   [url](http://utopia.duth.gr/~pefraimi/research/data/2007EncOfAlg.pdf)

7. Kool, Wouter, Herke Van Hoof, and Max Welling. "Stochastic beams and where to
   find them: The gumbel-top-k trick for sampling sequences without
   replacement." (2019). [url](https://arxiv.org/abs/1903.06059)
