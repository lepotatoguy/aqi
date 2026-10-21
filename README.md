# Uncovering local aggregated air quality index with smartphone captured images leveraging efficient deep convolutional neural network

## Table of Contents

- [Overview](#overview)
- [Implementations](#implementations)
- [Paper](#paper)
- [Dataset](#dataset)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Dataset Setup](#dataset-setup)
- [Usage](#usage)
- [About the PyTorch Version](#about-the-pytorch-version)
- [Results](#results)
- [Citation](#citation)
- [License](#license)

## Overview

In this research, we vigorously analyze the difficulties of predicting location-specific PM2.5 concentration from photos captured by smartphone cameras. Here, we particularly focus on Dhaka, the capital of Bangladesh, considering its very high level of air pollution exposure to a huge number of its dwellers. In our research, we develop a Deep Convolutional Neural Network (DCNN) and train it using more than a thousand outdoor photos captured and labeled by us. We capture the photos at various locations in Dhaka, Bangladesh, and label them based on PM2.5 concentration data extracted from the local US consulate as computed by the NowCast algorithm. During training with the dataset, our model learns a correlation index through supervised learning, which improves the model's ability to act as a Picture-based Predictor of PM2.5 Concentration (PPPC) making it capable of detecting comparable daily aggregated AQI index from a photo captured by a smartphone. The code and dataset is made publicly available [here](#).

## Implementations

The code is available in two independent implementations. Choose the one you want to work with; each folder has its own notebooks, conda environment and GPU notes, and neither depends on the other (if following strictly based on the research, please follow the Tensorflow one).

| Framework | Folder | Conda environment | Notes |
|---|---|---|---|
| TensorFlow / Keras | [`Codes/TensorFlow`](Codes/TensorFlow) | `aqi-net` | Original code used for the paper |
| PyTorch | [`Codes/PyTorch`](Codes/PyTorch) | `aqi-net-pytorch` | Port of the TensorFlow code with the same pipeline and hyperparameters |

Both folders contain the same three notebooks:

| Notebook | Description |
|---|---|
| `2.0airwire_optimum-final-documented.ipynb` | Proposed DCNN: data loading, sky cropping, training, evaluation and plots |
| `2.0airwire_optimum-k-fold-results - Compiled.ipynb` | 10-fold cross-validation of the proposed DCNN |
| `2.0airwire_optimum_other_models-with-pretrained-weights.ipynb` | Comparison with ImageNet-pretrained InceptionV3, ResNet50, VGG19, EfficientNetB7 and MobileNetV2 |

## Paper

* Published to Nature Scientific Reports. [Link](https://www.nature.com/articles/s41598-023-51015-1?).

## Dataset

The dataset is available via [Zenodo](https://zenodo.org/record/7878371).

## Repository Structure

```
aqi/
├── Codes/
│   ├── TensorFlow/
│   │   ├── 2.0airwire_optimum-final-documented.ipynb
│   │   ├── 2.0airwire_optimum-k-fold-results - Compiled.ipynb
│   │   ├── 2.0airwire_optimum_other_models-with-pretrained-weights.ipynb
│   │   ├── environment.yml
│   │   ├── for tensorflow in ubuntu.txt
│   │   └── k-fold-results.txt
│   └── PyTorch/
│       ├── 2.0airwire_optimum-final-documented.ipynb
│       ├── 2.0airwire_optimum-k-fold-results - Compiled.ipynb
│       ├── 2.0airwire_optimum_other_models-with-pretrained-weights.ipynb
│       ├── environment.yml
│       └── for pytorch in ubuntu.txt
├── LICENSE
└── README.md
```

## Installation

### 1. Clone the repository

```
git clone -b pytorch https://github.com/lepotatoguy/aqi.git
cd aqi
```

### 2. Install dependencies

Create the environment of the framework you chose.

#### TensorFlow (In research, we use this version)

```
conda env create -f Codes/TensorFlow/environment.yml
conda activate aqi-net
```

We use these commands below to create the environment.

```
conda create -n aqi-net python=3.10
conda activate aqi-net
conda install -y -c conda-forge tensorflow-gpu seaborn pandas matplotlib opencv ipykernel tqdm scikit-learn
```

For the CUDA/XLA setup in Ubuntu, see [`Codes/TensorFlow/for tensorflow in ubuntu.txt`](Codes/TensorFlow/for%20tensorflow%20in%20ubuntu.txt).

#### PyTorch

```
conda env create -f Codes/PyTorch/environment.yml
conda activate aqi-net-pytorch
```

Or create it manually:

```
conda create -n aqi-net-pytorch python=3.10
conda activate aqi-net-pytorch
conda install -y -c conda-forge pytorch torchvision seaborn pandas matplotlib opencv ipykernel tqdm scikit-learn openpyxl
```

On a machine with an NVIDIA driver, conda-forge installs the CUDA build of `pytorch`. PyTorch does not need the CUDA/XLA setup that TensorFlow needs; [`Codes/PyTorch/for pytorch in ubuntu.txt`](Codes/PyTorch/for%20pytorch%20in%20ubuntu.txt) has a cell to check that the GPU is used. The pretrained models in the comparison notebook are downloaded by `torchvision` on first use.

## Dataset Setup

```
# Download the dataset
wget https://zenodo.org/api/records/7878371/files-archive -O files-archive.zip

# Unzip the downloaded dataset
unzip files-archive.zip

# Move into the extracted folder
cd files-archive

# Dependecies for unrar dataset folder
sudo apt install unrar -y

# Extract the RAR archive (assumes `unrar` is installed)
unrar x dataset.rar

# Move the dataset into the folder of the framework you chose
FRAMEWORK=PyTorch   # or TensorFlow
mv dataset2.0 ../Codes/$FRAMEWORK/dataset
mv dataset.xlsx ../Codes/$FRAMEWORK/
```

The final documented notebook reads `dataset` and `dataset.xlsx` (set by `DATASET` and `DATASET_EXCEL` at its top). The k-fold and comparison notebooks read `dataset2.0` and `dataset_2.0.xlsx`; change the `path` and `read_excel` lines in those notebooks, or name the files accordingly.

## Usage

```
# Go to the folder of the framework you chose
cd ../Codes/$FRAMEWORK/

## Launch Jupyter Notebook or VS Code or your prefered software to launch the code
# jupyter notebook 2.0airwire_optimum-final-documented.ipynb
# code 2.0airwire_optimum-final-documented.ipynb
```

Select the `aqi-net` kernel for TensorFlow or the `aqi-net-pytorch` kernel for PyTorch.

## About the PyTorch Version

The PyTorch notebooks follow the TensorFlow notebooks cell by cell: the same preprocessing, splits, architecture, loss, learning rate, batch size, epochs, metrics and plots. Where PyTorch defaults differ from Keras, the Keras behaviour is set explicitly:

- Glorot-uniform weight initialisation and zero biases.
- BatchNormalization with `epsilon=1e-3` and `momentum=0.99`.
- `KerasAdam`, an Adam optimizer with the Keras update rule.
- `fit`, `evaluate` and `predict` helpers that match Keras, including `validation_split` (the last 10% of the training data) and epoch-averaged loss, MSE and RMSE.
- Frozen pretrained backbones keep BatchNorm in inference mode, as with Keras `trainable=False`.

Known differences:

- The pretrained comparison models use the `torchvision` ImageNet weights, which differ from the Keras weights, so their results will differ.
- In the k-fold notebook, each fold gets a fresh optimizer. The TensorFlow version reused one Adam instance across all folds.

## Results

[`Codes/TensorFlow/k-fold-results.txt`](Codes/TensorFlow/k-fold-results.txt) lists the 10-fold cross-validation scores (MAE loss, MSE and RMSE) of the TensorFlow run.

## Citation

If you have used this dataset, please cite the following paper: 

[1] Mondal, J.J., Islam, M.F., Islam, R. et al. Uncovering local aggregated air quality index with smartphone captured images leveraging efficient deep convolutional neural network. Sci Rep 14, 1627 (2024). https://doi.org/10.1038/s41598-023-51015-1

```
@article{Mondal_2024,
   title={Uncovering local aggregated air quality index with smartphone captured images leveraging efficient deep convolutional neural network},
   volume={14},
   ISSN={2045-2322},
   url={http://dx.doi.org/10.1038/s41598-023-51015-1},
   DOI={10.1038/s41598-023-51015-1},
   number={1},
   journal={Scientific Reports},
   publisher={Springer Science and Business Media LLC},
   author={Mondal, Joyanta Jyoti and Islam, Md. Farhadul and Islam, Raima and Rhidi, Nowsin Kabir and Newaz, Sarfaraz and Manab, Meem Arafat and Islam, A. B. M. Alim Al and Noor, Jannatun},
   year={2024},
   month=jan }
```

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
