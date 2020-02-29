<p align="center">
  <img src="docs/source/logo.png" height="150">
</p>

<h1 align="center">
  PyKEEN
</h1>

<p align="center">
  <a href="https://travis-ci.com/mali-git/POEM_develop">
    <img src="https://travis-ci.com/mali-git/POEM_develop.svg?token=2tyMYiCcZbjqYscNWXwZ&branch=master"
         alt="Travis CI">
  </a>

  <a href='https://opensource.org/licenses/MIT'>
    <img src='https://img.shields.io/badge/License-MIT-blue.svg' alt='License'/>
  </a>
</p>

<p align="center">
    <b>PyKEEN</b> (<b>P</b>ython <b>K</b>nowl<b>E</b>dge <b>E</b>mbeddi<b>N</b>gs) is a Python package designed to
    train and evaluate knowledge graph embedding models (incorporating multi-modal information). It is part of the
    <a href="https://github.com/SmartDataAnalytics/PyKEEN">KEEN Universe</a>.
</p>

<p align="center">
  <a href="#installation">Installation</a> •
  <a href="#quickstart">Quickstart</a> •
  <a href="#models">Models</a> •
  <a href="#datasets">Data Sets</a> •
  <a href="#supporters">Support</a>
</p>


## Installation

The development version of PyKEEN can be downloaded and installed from
[GitHub](https://github.com/mali-git/POEM_develop) on Python 3.7+ with:

```bash
$ git clone https://github.com/mali-git/POEM_develop.git pykeen
$ cd pykeen
$ pip install -e .
$ # Install pre-commit
$ pip install pre-commit
$ pre-commit install
```

## Contributing

Contributions, whether filing an issue, making a pull request, or forking, are appreciated. 
See [CONTRIBUTING.md](/CONTRIBUTING.md) for more information on getting involved.

## Quickstart

This example shows how to train a model on a data set and test on another data set.

The fastest way to get up and running is to use the pipeline function. It
provides a high-level entry into the extensible functionality of this package.
The following example shows how to train and evaluate the TransE model on the
Nations dataset. By default, the training loop uses the open world assumption
and evaluates with rank-based evaluation.

```python
from pykeen.pipeline import pipeline
result = pipeline(
    model='TransE',
    dataset='nations',
)
```

The results are returned in a dataclass that has attributes for the trained
model, the training loop, and the evaluation.

PyKEEN is extensible such that:

- Each model has the same API, so anything from ``pykeen.models`` can be dropped in
- Each training loop has the same API, so ``pykeen.training.LCWATrainingLoop`` can be dropped in
- Triples factories can be generated by the user with ``from pykeen.triples.TriplesFactory``

## Implementation

Below are the models, data sets, training modes, evaluators, and metrics implemented
in pykeen. These markdown tables can be regenerated with `pykeen ls`.

### Models (23)

| Name                | Reference                           | Citation                     |
|---------------------|-------------------------------------|------------------------------|
| ComplEx             | `pykeen.models.ComplEx`             | Trouillon *et al.*, 2016     |
| ComplExLiteral      | `pykeen.models.ComplExLiteral`      | Agustinus *et al.*, 2018     |
| ConvE               | `pykeen.models.ConvE`               | Dettmers *et al.*, 2018      |
| ConvKB              | `pykeen.models.ConvKB`              | Nguyen *et al.*, 2018        |
| DistMult            | `pykeen.models.DistMult`            | Yang *et al.*, 2014          |
| DistMultLiteral     | `pykeen.models.DistMultLiteral`     | Agustinus *et al.*, 2018     |
| ERMLP               | `pykeen.models.ERMLP`               | Dong *et al.*, 2014          |
| ERMLPE              | `pykeen.models.ERMLPE`              | Sharifzadeh *et al.*, 2019   |
| HolE                | `pykeen.models.HolE`                | Nickel *et al.*, 2016        |
| KG2E                | `pykeen.models.KG2E`                | He *et al.*, 2015            |
| NTN                 | `pykeen.models.NTN`                 | Socher *et al.*, 2013        |
| ProjE               | `pykeen.models.ProjE`               | Shi *et al.*, 2017           |
| RESCAL              | `pykeen.models.RESCAL`              | Nickel *et al.*, 2011        |
| RGCN                | `pykeen.models.RGCN`                | Schlichtkrull *et al.*, 2018 |
| RotatE              | `pykeen.models.RotatE`              | Sun *et al.*, 2019           |
| SimplE              | `pykeen.models.SimplE`              | Kazemi *et al.*, 2018        |
| StructuredEmbedding | `pykeen.models.StructuredEmbedding` | Bordes *et al.*, 2011        |
| TransD              | `pykeen.models.TransD`              | Ji *et al.*, 2015            |
| TransE              | `pykeen.models.TransE`              | Bordes *et al.*, 2013        |
| TransH              | `pykeen.models.TransH`              | Wang *et al.*, 2014          |
| TransR              | `pykeen.models.TransR`              | Lin *et al.*, 2015           |
| TuckER              | `pykeen.models.TuckER`              | Balazevic *et al.*, 2019     |
| UnstructuredModel   | `pykeen.models.UnstructuredModel`   | Bordes *et al.*, 2014        |

### Regularizers (5)

| Name     | Reference                                 | Description                                              |
|----------|-------------------------------------------|----------------------------------------------------------|
| combined | `pykeen.regularizers.CombinedRegularizer` | A convex combination of regularizers.                    |
| lp       | `pykeen.regularizers.LpRegularizer`       | A simple L_p norm based regularizer.                     |
| no       | `pykeen.regularizers.NoRegularizer`       | A regularizer which does not perform any regularization. |
| powersum | `pykeen.regularizers.PowerSumRegularizer` | A simple x^p based regularizer.                          |
| transh   | `pykeen.regularizers.TransHRegularizer`   | Regularizer for TransH's soft constraints.               |

### Losses (7)

| Name            | Reference                           | Description                                                                                                                                  |
|-----------------|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| bce             | `torch.nn.BCELoss`                  | Creates a criterion that measures the Binary Cross Entropy between the target and the output:                                                |
| bceaftersigmoid | `pykeen.losses.BCEAfterSigmoidLoss` | A loss function which uses the numerically unstable version of explicit Sigmoid + BCE.                                                       |
| crossentropy    | `pykeen.losses.CrossEntropyLoss`    | Evaluate cross entropy after softmax output.                                                                                                 |
| marginranking   | `torch.nn.MarginRankingLoss`        | Creates a criterion that measures the loss given inputs :math:`x1`, :math:`x2`, two 1D mini-batch `Tensors`,                                 |
| mse             | `torch.nn.MSELoss`                  | Creates a criterion that measures the mean squared error (squared L2 norm) between each element in the input :math:`x` and target :math:`y`. |
| nssa            | `pykeen.losses.NSSALoss`            | An implementation of the self-adversarial negative sampling loss function proposed by [sun2019]_.                                            |
| softplus        | `pykeen.losses.SoftplusLoss`        | A loss function for the softplus.                                                                                                            |

### Datasets (8)

| Name      | Reference                   | Description                                                                                        |
|-----------|-----------------------------|----------------------------------------------------------------------------------------------------|
| fb15k     | `pykeen.datasets.FB15k`     | The FB15k data set.                                                                                |
| fb15k237  | `pykeen.datasets.FB15k237`  | The FB15k-237 data set.                                                                            |
| kinships  | `pykeen.datasets.Kinships`  | The Kinship data set.                                                                              |
| nations   | `pykeen.datasets.Nations`   | The Nations data set.                                                                              |
| umls      | `pykeen.datasets.Umls`      | The UMLS data set.                                                                                 |
| wn18      | `pykeen.datasets.WN18`      | The WN18 data set.                                                                                 |
| wn18rr    | `pykeen.datasets.WN18RR`    | The WN18-RR data set.                                                                              |
| yago310   | `pykeen.datasets.YAGO310`   | The YAGO3-10 data set is a subset of YAGO3 that only contains entities with at least 10 relations. |

### Training Modes (2)

| Name   | Reference                          | Description                                                  |
|--------|------------------------------------|--------------------------------------------------------------|
| lcwa   | `pykeen.training.LCWATrainingLoop` | A training loop that uses the local closed world assumption. |
| owa    | `pykeen.training.OWATrainingLoop`  | A training loop that uses the open world assumption.         |

### Optimizers (6)

| Name     | Reference              | Description                                                             |
|----------|------------------------|-------------------------------------------------------------------------|
| adadelta | `torch.optim.Adadelta` | Implements Adadelta algorithm.                                          |
| adagrad  | `torch.optim.Adagrad`  | Implements Adagrad algorithm.                                           |
| adam     | `torch.optim.Adam`     | Implements Adam algorithm.                                              |
| adamax   | `torch.optim.Adamax`   | Implements Adamax algorithm (a variant of Adam based on infinity norm). |
| adamw    | `torch.optim.AdamW`    | Implements AdamW algorithm.                                             |
| sgd      | `torch.optim.SGD`      | Implements stochastic gradient descent (optionally with momentum).      |

### Evaluators (2)

| Name      | Reference                              | Description                                   |
|-----------|----------------------------------------|-----------------------------------------------|
| rankbased | `pykeen.evaluators.RankBasedEvaluator` | A rank-based evaluator for KGE models.        |
| sklearn   | `pykeen.evaluators.SklearnEvaluator`   | An evaluator that uses a Scikit-learn metric. |

### Metrics (6)

| Metric                  | Description                                                                                                        | Evaluator   | Reference                                  |
|-------------------------|--------------------------------------------------------------------------------------------------------------------|-------------|--------------------------------------------|
| Roc Auc Score           | The area under the ROC curve between [0.0, 1.0]. Higher is better.                                                 | sklearn     | `pykeen.evaluation.SklearnMetricResults`   |
| Average Precision Score | The area under the precision-recall curve, between [0.0, 1.0]. Higher is better.                                   | sklearn     | `pykeen.evaluation.SklearnMetricResults`   |
| Mean Rank               | The mean over all ranks: mean_i r_i. Lower is better.                                                              | rankbased   | `pykeen.evaluation.RankBasedMetricResults` |
| Mean Reciprocal Rank    | The mean over all reciprocal ranks: mean_i (1/r_i). Higher is better.                                              | rankbased   | `pykeen.evaluation.RankBasedMetricResults` |
| Hits At K               | The hits at k for different values of k, i.e. the relative frequency of ranks not larger than k. Higher is better. | rankbased   | `pykeen.evaluation.RankBasedMetricResults` |
| Adjusted Mean Rank      | The mean over all chance-adjusted ranks: mean_i (2r_i / (num_entities+1)). Lower is better.                        | rankbased   | `pykeen.evaluation.RankBasedMetricResults` |

### HPO Samplers (2)

| Name   | Reference                       | Description                                                     |
|--------|---------------------------------|-----------------------------------------------------------------|
| random | `optuna.samplers.RandomSampler` | Sampler using random sampling.                                  |
| tpe    | `optuna.samplers.TPESampler`    | Sampler using TPE (Tree-structured Parzen Estimator) algorithm. |

## Reproduction

PyKEEN includes a set of curated experimental settings for reproducing past landmark
experiments. They can be accessed and run like:

```bash
python -m pykeen.experiments reproduce tucker balazevic2019 fb15k
```

Where the three arguments are the model name, the reference, and the data set. The
output directory can be optionally set with `-d`.

## Acknowledgements

### Supporters

This project has been supported by several organizations:

- [Smart Data Analytics (University of Bonn)](http://sda.cs.uni-bonn.de)
- [Fraunhofer Institute for Intelligent Analysis and Information Systems](https://www.iais.fraunhofer.de)
- [Bonn Aachen International Center for IT (University of Bonn)](http://www.b-it-center.de)
- [Fraunhofer Institute for Algorithms and Scientific Computing](https://www.scai.fraunhofer.de)
- [Fraunhofer Center for Machine Learning](https://www.cit.fraunhofer.de/de/zentren/maschinelles-lernen.html)
- [Munich Center for Machine Learning (MCML)](https://mcml.ai/)
- [Technical University of Denmark - DTU Compute - Section for Cognitive Systems](https://www.compute.dtu.dk/english/research/research-sections/cogsys)
- [Technical University of Denmark - DTU Compute - Section for Statistics and Data Analysis](https://www.compute.dtu.dk/english/research/research-sections/stat)

### Logo

The PyKEEN logo was designed by Carina Steinborn.
