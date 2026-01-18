# openadmet-expansionrx-challenge-2026

## How it works aka Methodology

### Description of the Model

The features from the molecules (each represented as a smiles string) are extracted with Transformer model. 

### Performance comments

...

## Project structure

### package part

This project itself can be installed as a package. Installable part is used to share pieces of code between separate kaggle notebooks and to reuse them. To install, run
```
git clone https://github.com/latticetower/chemney.git
cd chemney
pip install .
```
This installs the package `chemney`, which contains models, preprocessing code, etc. with their dependencies.

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

These jupyter notebooks are run locally.

As a basis I use the same environment available on kaggle at the moment. When running script locally, I use python 3.11 and packages listed at `requirements.txt`