---
title: PTNews Corpus for Language Modelling 
layout: splash
share: false
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/news.jpg
  caption: By [Davide Nunes](https://davidenunes.com/about/)

#http://hemerotecadigital.cm-lisboa.pt/Periodicos/AImprensa_1919/N01/N01_item1/P1.html
#https://opensource.google/projects/lm-benchmark
#https://blog.einstein.ai/the-wikitext-long-term-dependency-language-modeling-dataset/
#https://arxiv.org/pdf/1905.12616.pdf
---

{% include feature_row id="intro" type="center" %}

The **PTNews Corpus** is a collection of over 19 million tokens extracted fom 10
years of political news articles from the Portuguese newspaper
[PÚBLICO](https://www.publico.pt/). The corpus is available under the [Creative
Commons Attribution-NonCommercial-ShareAlike Licence](## Licence). 

The corpus sizes between the preprocessed version of Penn Treebank (PTB) and
[WikiText-103](https://blog.einstein.ai/the-wikitext-long-term-dependency-language-modeling-dataset/).
Similarly to WikiText, PTNews has a larger vocabulary than PTB and retains the
original case, punctuation and numbers. This corpus contains over 31000 publicly
available full articles which makes it well suited for models that can take
advantage of long-term dependencies.

## Description and Examples
**PTNews** is preprocessed to be used as a word level **Portuguese corpus**.
It is available as a series of words or other tokens (like punctuation, numbers,
etc). The corpus was preprocessed so that users can access the tokens by
splitting by whitespace. 

In this processed version, the words with less than 3 occurrences are mapped to
the `<unk>` token. Each sentence in an article body occupies a single line of
the dataset and the end of paragraph is marked with the `<eop>` token at the end
of its last sentence. Portuguese words resulting from contractions like
__desta__, ou __nesta__ are separated into _d_, _esta_, _n_, _esta_,
respectively.

Some basic statistics for the ptnews corpus with the respective splits:

|----------+---------------+-------------|
|Vocabulary|  OoV Tokens    | OoV Rate   |
|:--------:|:--------------:|:----------:|
|68 318    |  95.043        |    0.5%    |
|----------+----------------+------------|


|----------+---------------+------------+------------+-----------| 
|          | Total         |Train       | Valid      | Test      |
|---------:|:--------------|:-----------|:-----------|:----------|
| Articles |  31 919       | 25 537     |3 191       | 3 191     |
| Tokens   |  19 021 661   | 15 242 995 |1 895 184   | 1 883 482 |
|----------+---------------+------------+------------+-----------|


Example article:
```
Carlos César : Cavaco " cansado e sem entusiasmo " quis afastar responsabilidades sobre a crise
https://publico.pt/2010/06/10/politica/noticia/carlos-cesar-cavaco-cansado-e-sem-entusiasmo-quis-afastar-responsabilidades-sobre-a-crise-1441369
2010-06-10 15:38:00

O presidente do Governo Regional dos Açores , Carlos César , considerou hoje que Cavaco Silva esteve " cansado e sem entusiasmo " no discurso do Dia de Portugal , onde afastou responsabilidades sobre a actual crise . <eop>
" O país ouviu um Presidente cansado e sem entusiasmo , que andou às voltas com os papéis para dizer que não tinha nada a ver com as razões da crise " , afirmou Carlos César , num comentário à Lusa sobre o discurso do Presidente da República na cerimónia oficial do 10 de Junho , realizada em Faro . <eop>
Carlos César considerou , no entanto , " positivo " que Cavaco Silva tenha feito " um discurso alinhado com um tema recorrente na apreciação do momento que vivemos , o da coesão e da corresponsabilização " . <eop>
No mesmo sentido , manifestou concordância com o apelo que Cavaco Silva fez " à responsabilidade dos empregadores e empregados " , mas deixou um alerta relativamente à referência do Presidente da República à necessidade de " limpar Portugal " . <eop>
Para Carlos César , se essa referência " for despida de conteúdo institucional útil , tratou-se de mais um discurso que se perderá na babugem política d aquilo que Cavaco Silva entendeu recordar como o ' rectângulo ' " . <eop>
```


## Download & Utils

There are two datasets available: ptnews and ptnews-origin.

* [Download PTNews word level dataset](https://zenodo.org/record/3908507/files/ptnews.tar.gz?download=1)

The **word level ptnews dataset** archive contains 3 files:
`ptnews.train.tokens`, `ptnews.valid.tokens`, and `ptnews.test.tokens` with the
__train__, __validation__, and __test__ **splits** respectively. Each of these
only contains titles and news article bodies. For the full information about
each article date and url from which it was extracted, see the _origin_ dataset.


* [Download PTNews origin dataset](https://zenodo.org/record/3908507/files/ptnews_origin.tar.gz?download=1)

The __origin__ dataset file contains a single file with articles containing a
title, both the dates and urls from which the news articles were extracted, and
the article body. 

### Python interface to PTNews
A Python interface to the PTNews interface for convenient access to the PTNews
dataset is also available through the `nldata` python package. The package can
be installed from [PyPI](https://pypi.org/project/nldata/) using `pip` or other
package manager such as [Poetry](https://python-poetry.org/) or
[Pipenv](https://pipenv-fork.readthedocs.io/en/latest/).

```bash
pip install nldata
```

The `PTNews` corpus class class can be used to read the dataset files downloaded
above from a given directory, or, to download, extract, and cache them directly

```python
from nldata.corpora import PTNews

# reads from a local directory
ptnews = PTNews(data_dir=...)

# alternatively download and cache the files
ptnews = PTNews()

# get 4 sentences from the train split file (default to all sentences)
for sentence in ptnews.split("train",n=4):
  # do something with the sentence 
  print(sentence)
```
If the corpus needs to be downloaded (because no `data_dir` was specified) and
the files are not in cacheThe, a file download progress is printed to the
`stdout` (using [`tqdm`](https://tqdm.github.io/)). If you run this a second
time, the files are already in cache and can be used.

```bash
Downloading: 100%|██████████| 35.9M/35.9M [00:27<00:00, 1.28MB/s]
['“', 'Descoloniza', '”', ':', 'estátua', 'de', 'Padre', 'António', 'Vieira', 'vandalizada', 'em', 'Lisboa']
['A', 'estátua', 'do', 'Padre', 'António', 'Vieira', ',', 'no', 'Largo', 'Trindade', 'Coelho', ',', 'em', 'Lisboa', ',', 'foi', 'vandalizada', 'com', 'a', 'palavra', '“', 'descoloniza', '”', 'pintada', 'a', 'vermelho', '.']
['A', 'boca', ',', 'mãos', 'e', 'hábito', 'do', 'clérigo', 'foram', 'tingidas', 'de', 'vermelho', 'e', 'no', 'peito', 'das', 'crianças', 'indígenas', 'que', 'estão', 'representadas', 'à', 'sua', 'volta', 'foi', 'pintado', 'um', 'coração', '.']
['Durante', 'a', 'noite', 'd', 'esta', 'quinta-feira', ',', 'a', 'Câmara', 'de', 'Lisboa', 'procedeu', 'à', 'limpeza', 'da', 'estátua', '.', '“']
```

If no split `name` is provided, the default is to merge all the splits:
```python
# the default for the split is "full" which will merge all the splits into a single iterator
for sentence in ptnews.split(n=4):
  print(sentence)

```


## Lexical Analysis
The table below shows the frequency distribution for the 20 most common tokens
in ptnews, and the most common without including [stop
words](https://en.wikipedia.org/wiki/Stop_words){:target="_blank"} or
punctuation. The most common token is the the __comma__, which occurs over 1
million times. Note that words starting with an upper case letter are considered
distinct from the ones starting with lower case letters.


|----+-------------------+-----------|+-------------------+--------|
|Rank| (All Words)       | Freq      ||(No Stop/No Punct)| Freq   |
|---:|:------------------|:----------||:------------------|:-------|
|  1 | ,                 | 1 116 769 || Governo           | 57 886 |
|  2 | de                | 748 916   || PS                | 47 530 |
|  3 | a                 | 572 820   || PSD               | 44 775 |
|  4 | que               | 555 783   || Portugal          | 34 056 |
|  5 | .                 | 550 757   || República         | 28 773 |
|  6 | o                 | 453 748   || disse             | 28 207 |
|  7 | e                 | 405 687   || país              | 26 573 |
|  8 | do                | 344 723   || António           | 24 661 |
|  9 | <eop>             | 292 959   || partido           | 24 565 |
| 10 | da                | 291 415   || política          | 24 432 |
| 11 | “                 | 233 492   || Costa             | 22 982 |
| 12 | ”                 | 233 031   || presidente        | 21 017 |
| 13 | para              | 191 028   || líder             | 20 854 |
| 14 | "                 | 186 934   || Presidente        | 20 759 |
| 15 | em                | 173 211   || PCP               | 18 569 |
| 16 | os                | 167 225   || primeiro-ministro | 18 439 |
| 17 | não               | 151 092   || afirmou           | 17 675 |
| 18 | um                | 137 056   || CDS               | 17 525 |
| 19 | com               | 133 778   || Passos            | 16 890 |
| 20 | uma               | 131 811   || ministro          | 16 433 |
|----+-------------------+-----------|+-------------------+--------|

[Zipf's law](https://en.wikipedia.org/wiki/Zipf%27s_law){:target="_blank"}
states that given some corpus, the frequency of any word is inversely
proportional to its rank in the frequency table. Thus the most frequent word
will occur approximately twice as often as the second most frequent word, three
times as often as the third most frequent word, etc. 

One of the problems with existing corpus like the PTB (frequently used for
language modeling) is that all the tokens are lower case, stripped of any
punctuation, and limited to a vocabulary of only 10k words. This preprocessing
means that there is a lack of words in the long tail of the frequency
distribution, and these, can be important to applications that have to deal with
rare words such as named entities.

<figure class="half">
    <a href="/assets/images/ptnews/zipf.svg"><img src="/assets/images/ptnews/zipf.svg"></a>
    <a href="/assets/images/ptnews/zipf_nostop.svg"><img src="/assets/images/ptnews/zipf_nostop.svg"></a>
    <figcaption>Zipfian log-log plot showing the relationship between word rank and
frequency for the 100k most frequent words, with (left) and without (right) stop words and punctuation.</figcaption>
</figure>


## Citation

```
@dataset{Nunes2020_3908507,
  author       = {Nunes, Davide},
  title        = {PTNews Corpus},
  month        = jun,
  year         = 2020,
  publisher    = {Zenodo},
  version      = 1,
  doi          = {10.5281/zenodo.3908507},
  url          = {https://doi.org/10.5281/zenodo.3908507}
}
```

## Results & Resources
If you wish to report results or other resources obtained on the PTNews contact
[Davide Nunes](mailto:davidenunes@pm.me) with the following information: 
* **Task**: e.g. Language Modelling, Semantic Similarity, etc;
* **Publication URL**: url to published article or preprint;
* **Type of Model**: LSTM Neural Network, n-grams, GloVe vectors, etc;
* **Evaluation Metrics**: e.g. validation and testing perplexities in the case of language modelling.

## Contact Information
If you have questions about the corpus or want to report benchmark results,
contact [Davide Nunes](mailto:davidenunes@pm.me).

## Licence
[PTNews Corpus](({{site.url}}/about)) by [Davide Nunes]({{site.url}}/about) is
licensed under CC BY-NC-SA 4.0 <a
href="https://creativecommons.org/licenses/by-nc-sa/4.0"><img
style="height:22px!important;margin-left: 3px;"
src="https://mirrors.creativecommons.org/presskit/icons/cc.svg" /><img
style="height:22px!important;margin-left: 3px;"
src="https://mirrors.creativecommons.org/presskit/icons/by.svg" /><img
style="height:22px!important;margin-left: 3px;"
src="https://mirrors.creativecommons.org/presskit/icons/nc.svg" /><img
style="height:22px!important;margin-left: 3px;"
src="https://mirrors.creativecommons.org/presskit/icons/sa.svg" /></a>. To view
a copy of this license, visit
[cc-by-nc-sa/4.0](https://creativecommons.org/licenses/by-nc-sa/4.0). The
material contained on the PTNews Corpus is &copy; 2010-2020 [PÚBLICO Comunicação
Social SA](https://www.publico.pt/).

<!--http://static.publico.pt/homepage/site/nos/copyright.asp-->

