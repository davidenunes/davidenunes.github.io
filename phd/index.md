---
title: Semantic Grounding with Incremental Neural Representation Learning
layout: splash
share: false
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/glitch.gif
  #actions:
  #  - label: "Download"
  #    url: "/"
#excerpt: "summary goes here"
---

{% include feature_row id="intro" type="center" %}

## Summary
Language is continuously changing. This makes human discourse a
particularly challenging domain for computational language technology.
Computational semantics aims to automate the construction of meaningful
representations from natural language expressions. Computational models of
representation learning are fundamental to solve real-world problems such as
automatic speech recognition or statistical machine translation. They are also
prime candidates for mechanisms that enable meaning acquisition and grounding.
The meaning of a representation for a given expression depends on how this
expression is used, therefore, the conceptual space determining how different
concepts or words relate to each other is shaped by the task the representations
are meant to solve. 

This thesis explores techniques to ground the meaning of words by learning
representations acquired from text corpora . Artificial Neural Network (ANN)
models are capable of learning robust distributed representation and generalise
to unseen data, however, existing neural network-based approaches require
knowledge about vocabulary size. This not only makes them computationally
expensive for large vocabularies, but also prevents them from being deployed in
an incremental, and cumulative fashion. This work solves this problem by
compressing the input space with random projections, and puts forward a new
estimator inspired by optimal transport theory. The contributions of this thesis
allow for ANN models to learn representations and solve tasks incrementally in
open domains. This makes these models capable of tackling language dynamics in
large scale settings, furthermore, and contrary to existing approaches, the
achieved computational and memory efficiency allows the models to be deployed in
resource-limited settings.  

The solutions are evaluated in the context of language modelling tasks in
multiple text corpora. The proposed _compressive prediction_ ANN
architectures make a trade-off between an acceptable amount of error, and the
capacity to encode words from a vocabulary of unknown size. Competitive results
are achieved with more compact models when compared with state-of-the-art
language models. The presented mechanisms can be seen as a form of
auto-associative memory that provides a different view for self-supervised
machine learning. In this view, continuous representation learning and
forgetting forms the basis for lifelong adaptation. The proposed principles are
applicable beyond natural language domains and open new research directions that
would allow us not only to learn representations for complex discrete
structured inputs, but also to make structured predictions in complex open-endend environments.


<!--

### Incremental Semantic Grounding
### Meaning, Language and Grounding
### Computational Semantics
### Representation Learning as Grounding
### Neural Networks and Probabilistic Modelling
### Distributed Representations and Random Projections
### Research Questions and Contributions

## Background and Related Work
### Distributional Semantics and Language Modelling
### Vector Space Models
### Dimensionality Reduction and Random Projections
### Neural Language Models
### Energy-based models and contrastive learning

## Neural Random Projections

### Neural Probabilistic Language Modelling
### Computational Bottlenecks and Unnormalized Models
### Random Projections
### Experimental Setup
### Model Performance and Compression
### Conclusion

## Compressive Predictions

### Maximum Likelihood Bottleneck
### Estimating Unnormalized Neural Language Models 
### Contrastive Estimation and Optimal Transport
### Experimental Setup
### Noise Contrastive Estimation vs Partial Optimal Transport
### Conclusion and Future Prospects

## Conclusion

### Summary of Contributions
### Challenges and Open Questions
### Compression as a Theory of the Mind
### Future Prospects in Structured Inference

-->






