# Efficient Training of Robust Decision Trees Against Adversarial Examples (GROOT) - Experiments

This repository contains the scripts to reproduce the experiments from the paper 'Efficient Training of Robust Decision Trees Against Adversarial Examples' about the GROOT algorithm.

To **install** the required depencies run:
```
pip install requirements.txt
```

## Classifier comparison
To reproduce the comparison with previous works there are three main scripts:

`train_kfold_models.py`: This script fits all models in parallel pools and writes the fitted models in XGBoost JSON format under `out/trees` and `out/forests`. It also outputs a file `out/runtimes.csv` that keeps track of how long it took to fit each model. The script first runs all fast forest models, then all fast tree models, then provably boosting and finally TREANT in parallel pools.

`fit_chen_xgboost.py`: Since Chen et al. have their own implementation of robust boosting built on top of XGBoost, we have a separate script to generate results for their method. Please follow their installation instructions at https://github.com/chenhongge/RobustTrees and copy the built `xgboost` binary to this directory then run `fit_chen_xgboost.py`. This will output trained models under `out/forests/` and a separate runtime file `out/chenboost_runtimes.csv`.

`generate_kfold_results.py`: This script uses the exported models from `out/trees` and `out/forests`, then runs the MILP attack by Kantchelian et al. on them. It outputs result figures under `out/`.

## Image experiments
In the paper we ran experiments on a binary classification version of MNIST and Fashion-MNIST. The commands below train and evaluate the ensemble models on these datasets and visualize some optimal adversarial examples. The trained models and images output under `out/mnist_ensembles/` and `out/fmnist_ensembles/`. You can run:
```
python image_experiments.py --dataset mnist --epsilon 0.4
python image_experiments.py --dataset fmnist --epsilon 0.1
```

## Visualize threat models
GROOT has support for some flexibility in terms of the threat model that it assumes. Specifically the perturbation range of each feature can be set separately as the `attack_model` parameter. To plot the effect of changing this parameter on the learned models you can run `visualize_threat_models.py`. This script will output visualizations of the trees' decision regions on a 2D dataset under `out/`.

## Dataset summary
To print a quick summary table of the datasets used in the paper run `summarize_datasets.py`.
