# MLOps Tiny Stories

## Project description

### Goal
A replication of parts of the [Tiny Stories](https://arxiv.org/abs/2305.07759) paper.
### Framework
Since we are working with language models, the hugging face [`transformers`](https://huggingface.co/docs/transformers/index) package will be very useful. The package provides a variety of pre-trained models, but since we are training a model from scratch, this is not in it self useful. Luckilly, the package also provides methods of creating new models based on the architecture of the pre-trained models while also allowing customizing the configuration (e.g. number of layers or attention heads). This is what we will use, as it will allow us to very easily try out many different configurations.
### Data
We will use the original dataset from the paper, which can be found on [Huggingface](https://huggingface.co/datasets/roneneldan/TinyStories), the dataset contains more than 2 million stories generated by ChatGPT4 with the instructions to use the vocabulary of a child. Most of the stories are around 500-1000 characters long.
### Models
Here again we will copy the paper. They use a GPT Neo architecture (supported by the Huggingface `transformers` package) to make small languge models (less then a billion paremeters) that can be trained on a single GPU core. This could be models with only a single layer, or a few small layers.We will initially use the same architecture, but may try out other things if we find interesting ideas. While training a language model would usually be a terrible idea for this project due to the complexity and resource requirements, the small models described in the paper can realistically be trained within a few hours. This is evidenced by this [replication](https://medium.com/@kl.yap/replicating-tinystories-paper-38839d03ec81). As the replication shows, getting the performance shown in the original paper is not trivial, but this is fine since that is not the goal.
### Motivation
Large language models have proven to be useful, but they are very resource intensive, which is prohibitive for most researchers. By implementing the small language models described in the paper we can use ML ops to create a playground for students and researchers where new ideas can be quickly tested on consumer grade hardware.

## Project structure

The directory structure of the project looks like this:

```txt

├── Makefile             <- Makefile with convenience commands like `make data` or `make train`
├── README.md            <- The top-level README for developers using this project.
├── data
│   ├── processed        <- The final, canonical data sets for modeling.
│   └── raw              <- The original, immutable data dump.
│
├── docs                 <- Documentation folder
│   │
│   ├── index.md         <- Homepage for your documentation
│   │
│   ├── mkdocs.yml       <- Configuration file for mkdocs
│   │
│   └── source/          <- Source directory for documentation files
│
├── models               <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks            <- Jupyter notebooks.
│
├── pyproject.toml       <- Project configuration file
│
├── reports              <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures          <- Generated graphics and figures to be used in reporting
│
├── requirements.txt     <- The requirements file for reproducing the analysis environment
|
├── requirements_dev.txt <- The requirements file for reproducing the analysis environment
│
├── tests                <- Test files
│
├── mlops-tinystories  <- Source code for use in this project.
│   │
│   ├── __init__.py      <- Makes folder a Python module
│   │
│   ├── data             <- Scripts to download or generate data
│   │   ├── __init__.py
│   │   └── make_dataset.py
│   │
│   ├── models           <- model implementations, training script and prediction script
│   │   ├── __init__.py
│   │   ├── model.py
│   │
│   ├── visualization    <- Scripts to create exploratory and results oriented visualizations
│   │   ├── __init__.py
│   │   └── visualize.py
│   ├── train_model.py   <- script for training the model
│   └── predict_model.py <- script for predicting from a model
│
└── LICENSE              <- Open-source license if one is chosen
```

Created using [mlops_template](https://github.com/SkafteNicki/mlops_template),
a [cookiecutter template](https://github.com/cookiecutter/cookiecutter) for getting
started with Machine Learning Operations (MLOps).

# Profiling

In order to profile training of the model, run the `profile_training.py` script.
This is a Hydra script and requires any training config.
The full command is therefore
```bash
python mlopstinystories/profile_training.py --config-name debug
```
where `debug` is a config that trains for only 50 steps.
Note that the script assumes that the raw data has already been downloaded using the `data/fetch_raw_data.py` script.

To view the profiling results, run
```bash
tensorboard --logdir=./profiling
```
