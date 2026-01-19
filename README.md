# OpenADMET ExpansionRX Blind Challenge

My solution for https://openadmet.ghost.io/openadmet-expansionrx-blind-challenge/

## How it works. Methodology
### Data preparation

I've used the provided training dataset, no additional data sources. 
As a preliminary step I've removed from the train the compound with `I` atom, since it was only one sample present in the train dataset (and it was absent in blind test molecules).

The molecules were converted from isomeric SMILES to non-isomeric.

Splitted the data to 2 folds with stratification and groups, as a groups I've used molecular Murcko scaffolds, stratification is done based on non-null targets (to keep the number of non-null values more or less the same between the folds for each targets).

Two parts out of 10 were used as a validation and test datasets, the remaining 8 were used during training.

Corresponding code: https://github.com/latticetower/chemney/blob/main/kaggle/0-data-split/openadmet2026-data-split.ipynb

### Description of the Model

#### Pretraining

To produce target-specific latent spaces, I've used Chemberta 2 model with 77M parameters (DeepChem/ChemBERTa-77M-MLM) with multiple heads (each head corresponds to one of 9 available regression targets). 

To morph the latent space, I've used Rank-and-Contrast loss (https://github.com/kaiwenzha/Rank-N-Contrast) during pretraining. 

The pretraining was run 40 epochs, I've used sentence transformers framework to implement the model and kaggle notebook with P100 GPU to run it.

Corresponding code for pretraining: https://github.com/latticetower/chemney/blob/main/kaggle/1-pretrain-rncloss/openadmet2026-pretrain-rncloss.ipynb

#### Training

The chemberta model finetuned during pretraining is used as a source of features. I combine them with corresponding MACCS fingerprints and use to train simple models from scikit-learn (RidgeRegressor and KNeighborsRegressor), to predict each of the target variables separately.

Corresponding code: https://github.com/latticetower/chemney/blob/main/kaggle/2-train-v1/openadmet2026-train-v1.ipynb
Inference on blind test: https://github.com/latticetower/chemney/blob/main/kaggle/3-inference-v1/openadmet2026-inference-v1.ipynb


## Project structure

### `kaggle`

Subset on notebooks run and executed at kaggle platform.
The structure of the directory is the following: subdirectory `input` is used to store local files and data which is used during notebooks execution, different subfolders correspond to kaggle notebooks.

The environment corresponds to the one currently used at kaggle, additional packages may be installed in the notebooks.

The data is linked with kaggle notebooks (via kaggle api) stored at subfolders, with the commands:
```bash
kaggle kernels pull -m latticetower/openadmet2026-data-split -p 0-data-split
kaggle kernels pull -m latticetower/openadmet2026-pretrain-rncloss -p 1-pretrain-rncloss
kaggle kernels pull -m latticetower/openadmet2026-train-v1 -p 2-train-v1
kaggle kernels pull -m latticetower/openadmet2026-inference-v1 -p 3-inference-v1
```
After that, I've added linking to github via kaggle, to show changes made in the kaggle notebooks in corresponding github files.

The prefix in the notebook's subfolder name indicates the order in which they are supposed to be executed. 

### `notebooks`

These jupyter notebooks were run locally.

As a basis I use the same environment available on kaggle at the moment. When running script locally, I use python 3.11 and packages listed at `requirements.txt`