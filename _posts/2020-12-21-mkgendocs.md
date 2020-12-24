---
title: "Python Autodocs with MkGenDocs"
excerpt_separator: "<!--more-->"
sidebar: False
comments: True
toc: true
toc_label: Table of Contents
toc_sticky: true
#classes: wide
categories:
  - tools
  - python
  - technical
  - documentation 
  - api
---

[mkgendocs](https://github.com/davidenunes/mkgendocs){:target="_blank"} is a
Python package for automatically generating documentation pages in **markdown**
from Python source files, by parsing Google-style **docstring**.

If you search for automated documentation generators for Python (or any language
really), half the Web will tell you that relying on auto-documentation tools
instead of writing good useful documentation by hand, is a bad practice. 
They are partially right, but real projects are not a salty thread from Reddit, so take all the advice, like anything else, critically!

Open source project success can depend on well you communicate about it with your community. Nevertheless, there is something to be said about generating useful documentation from existing source files. In the case of Python, **docstrings** are at the frontline of your project documentation. Note that not every project will benefit from a publicly documented API, but some (e.g. like a library of re-usable components) will. Creating reference documentation is essencial for foundational, especially when the number of components in the library is extensive. Examples of such projects include:

* [NumPy Reference](https://numpy.org/doc/stable/reference/index.html)
* [Tensorflow API](https://www.tensorflow.org/api_docs/python/tf)
* [PyTorch API](https://pytorch.org/docs/stable/)
* [scikit-learn API](https://scikit-learn.org/stable/modules/classes.html)

These examples include API reference documentation that is generated and linked to the original source files (usually hosted on publicly available online repositories).

## From Sphinx to MkDocs
The most feature-complete documentation generator for Python is without a doubt [Sphinx](https://www.sphinx-doc.org/en/master/index.html). It is robust and feature rich, it can be made to work with other markdown languages besides [reStructuredText](https://docutils.sourceforge.io/rst.html), and different styles of docstrings with extensions like [napoleon](Support for NumPy and Google style docstrings). 

[MkDocs](https://www.mkdocs.org/) on the other hand, is a static documentation website generator from Markdown. It has less features, but being more simple it also requires less extensions and configuration. Overall, it has a lower barrier of entry for someone wanting to start deploying documentation for a project. One of the downsides of MkDocs, is the fact that doesn't include robust plugins for automatically building API documentation from docstrings. This led me to create [mkgendocs](https://github.com/davidenunes/mkgendocs){:target="_blank"}.

## Beautiful Documentation for Python Projects
I was leaning towards MkDocs because of themes like [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/). The theme follows the spirit of Google’s material design guidelines. It is responsive, great on mobile devices, and highly customizable. It also offers a search bar with live updates, automated table of contents, among other features such as support for [MathJax](https://squidfunk.github.io/mkdocs-material/reference/mathjax/), [Admonitions](https://squidfunk.github.io/mkdocs-material/reference/admonitions/), etc. This makes for documentation that is not only good looking, but easy to navigate, comprehend, and generally pleasant to use.

[mkgendocs](https://github.com/davidenunes/mkgendocs){:target="_blank"} provides a simple tool that takes existing Python source files and generates API documentation pages that work well with Material for MkDocs. 

For now it only supports Google style docstrings.
{: .notice--warning}


<figure>
    <a href="/assets/images/posts/mkgendocs.png"><img src="/assets/images/posts/mkgendocs.png"></a>
    <figcaption>Example of API documentation generation for <a href="https://tensorx.org">TensorX</a> library.</figcaption>
</figure>

![]()

If a repository url is provided, mkgendocs uses this to automatically link the generated API documentation pages to the source files online. 

For now I only added support for Github. Should not be difficult to add and test the source code linking feature for other providers.
{: .notice--warning}

## Getting started with mkgendocs

### Installation
Install mkgendocs from [PyPI](https://pypi.org/project/mkgendocs/)

```python
pip install mkgendocs
```

### Usage

```
gendocs --config mkgendocs.yml
```

A sources directory is created with the documentation that was automatically generated.
Any examples in a "examples" directory are automatically copied over to the documentation, 
the module level docstrings of any example source files are also copied and converted to markdown. 


### Configuration Example

````yaml
sources_dir: docs/sources
templates_dir: docs/templates
repo: https://github.com/davidenunes/tensorx  #link to sources on github
version: master                               #link to sources on github

pages:
  - page: "api/train/model.md"
    source: "tensorx/train/model.py"
    methods:
      - Model:
          - train
          - set_optimizer
  
  - page: "api/layers/core.md"
    source: 'tensorx/layers.py'
    classes:
      - Linear:
        - compute_shape
      - Module
  - page: "math.md"
    source: 'tensorx/math.py'
    functions:
      - sparse_multiply_dense

  # creates an index page based on everything from target source
  - page: "api/layers/index.md"
    source: "tensorx/layers.py"
    index: True
````

* **sources_dir**: directory where the resulting markdown files are created
* **templates_dir**: directory where template files can be stored. All the folders and files are 
copied to the `sources_dir`. Any markdown files are used as templates with the 
tag `{{autogenerated}}` in the template files being replaced by the generated documentation.
* **repo**: repository to create `view source` links automatically for each class, function, and method;
* **version**: defaults to "master" to create link to sources in the form `https://repo/blob/version/file.py/#L1425`;
* **pages**: list of pages to be automatically generated from the respective source files and templates:
    * **page**: path for page template / sources dir for the resulting page;
    * **source**: path to source file from which the page is to be generated;
    * **classes**: list of classes to be fully documented; a list of method names can be passed for each class, the default is
      to generate all methods; 
    * **functions**: list of functions to be documented.
    * **index**: if True creates an index page for the given sources, you can also specify classes and functions, but not methods
  

## Thank you!
Thank you for your interest. This is very much a work in progress, evolving according to my own needs, but if you find any of this useful, consider getting me some coffee, coffee is great!
<br/><br/>
<a href='https://ko-fi.com/Y8Y0RZO6' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi3.png?v=0' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

