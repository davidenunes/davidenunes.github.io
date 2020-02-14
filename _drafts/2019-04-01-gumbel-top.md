---
title: "Gumbel-Top Trick"
excerpt_separator: "<!--more-->"
sidebar: False
comments: true
classes: wide
categories:
  - updates
---

## Vectorized code 
If you work with libraries such as NumPy or Tensorflow, etc, you (should) end-up
writing a lot of vectorization code. This is, instead of using control-flow
operations (e.g. for loops), you write code that operates on an entire set of
values at once. Inputs are vectors, matrices or tensors, and your functions
apply transformations outputing vectors, matrices, or tensors. Lower-level
libraries optimized for linear algebra operations such as matrix multiplications
make dramatic performance improvements, especially when aided by modern hardware
with direct support for vector-based instructions.

Vectorizing code can be particularly challenging, especially for things that
require bookkeeping --when computations on one element depend on the results of
previous computations.

## Goal: sample k classes without replacement
My goal is to sample  a batch of _k_ classes without replacement from a discrete
distribution, parametrized by unnormalized log-probabilities.

### Vectorizing sampling without replacement
`NumPy` `random.choice` comes to mind. We want to sample 3 elements from a given
population, say of size 5 without replacement. I would also like to do this in
parallel, so we want 2 of such samples. Writing:
```python
import numpy as np
np.random.choice(5, [2,3], replace=False)
```
This doesn't quite work as intended. The parameter `size=[2,3]` only defines the
output shape for the operation, in this case a 2x3 matrix.

```diff
- ValueError: Cannot take a larger sample than population when 'replace=False'
```

Trying to sample 6 elements from a population of 5 without replacement is
obviously not what I want. Other libraries like `Tensorflow` don't have a
vectorized version of sampling without replacement either for the simple reason
that sampling without replacement requires bookkeeping. In other words, sampling
from a population depends on the values we already sampled, so we cannot do it
in parallel, **unless** we trade off memory for the ability to do just that. To
do that we can use the **Gumbel-top** trick. 

Instead of sampling sequentially, we start with a vector with the probabilities
for each element of the population $p_i$. Bonus: these probabilities do not need to be
normalized # is this too much of a leap_ if the probabilities are normalized we
can use the exp normalize trick
http://timvieira.github.io/blog/post/2014/02/11/exp-normalize-trick/

```python
def top_k(x, n):
    """Returns the n largest indices from a numpy array."""
    return np.partition(x, n)[..., -n:]

def sample_k(logits):
    noise = random_uniform()
    return top-k(logits - tf.log(-tf.log(noise)), 1)
```

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

Tipically, when we use neural networks to build probabilistic models, we want to parameterize a discrete distribution in terms of a unconstrained vector whose values are not confined to the simplex. These vectors give us _unnormalized log probabilities_ that can be transformed into a discrete distribution using the **softmax** function:

 $$ \pi_k = \frac{\exp\(x_k\){\sum_k'=1^K\exp(x_{k'})} $$


Once we have this transformation, we can draw samples from the resulting distribution. It turns out that we don't need to construct this distribution to sample from it.

... insert image of gumbel distribution
... insert image

### Gumbel-Max Trick

I was working on a project using Tensorflow and I needed to generate batches of k class samples without replacement from a uniform distribution. I first turned to Tensorflow [candidate sampling ops](https://www.tensorflow.org/extras/candidate_sampling.pdf)


#  provides a simple and efficient wayto draw sampleszfrom a categorical distribution with class probabilitiesπ


### Gumbel-TopK Trick


## References

1. Jang, Eric, Shixiang Gu, and Ben Poole. **"Categorical reparameterization with gumbel-softmax."** _[arXiv preprint](https://arxiv.org/pdf/1611.01144.pdf)_


https://timvieira.github.io/blog/post/2014/07/31/gumbel-max-trick/

https://lips.cs.princeton.edu/the-gumbel-max-trick-for-discrete-distributions/

https://www.quora.com/What-is-Noise-Contrastive-estimation-NCE

"GPUs are designed with one goal in mind: process graphics really fast. Since this is the only concern they have, there have been some specialized optimizations in place that allow for certain calculations to go a LOT faster than they would in a traditional processor."

https://casmls.github.io/general/2017/02/01/GumbelSoftmax.html

https://arxiv.org/pdf/1611.01144.pdf

http://blog.shakirm.com/2015/10/machine-learning-trick-of-the-day-4-reparameterisation-tricks/

# this post is very good
http://amid.fish/humble-gumbel

https://casmls.github.io/general/2017/02/01/GumbelSoftmax.html

http://anotherdatum.com/gumbel-gan.html

https://en.wikipedia.org/wiki/Softmax_function

https://en.wikipedia.org/wiki/Gumbel_distribution

https://arxiv.org/pdf/1704.00805.pdf

https://arxiv.org/abs/1611.00712

### gumbel sampling as counter examples
### original distribution can be the average random projection...can I sample a ternary vector from that if I don't separate the components?
