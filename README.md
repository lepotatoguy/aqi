# Uncovering local aggregated air quality index with smartphone captured images leveraging efficient deep convolutional neural network

## Table of Contents

- [Overview](#overview)
- [Paper](#paper)
- [Dataset](#dataset)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Dataset Setup](#dataset-setup)
- [Usage](#usage)
- [Citation](#citation)
- [License](#license)

## Overview

In this research, we vigorously analyze the difficulties of predicting location-specific PM2.5 concentration from photos captured by smartphone cameras. Here, we particularly focus on Dhaka, the capital of Bangladesh, considering its very high level of air pollution exposure to a huge number of its dwellers. In our research, we develop a Deep Convolutional Neural Network (DCNN) and train it using more than a thousand outdoor photos captured and labeled by us. We capture the photos at various locations in Dhaka, Bangladesh, and label them based on PM2.5 concentration data extracted from the local US consulate as computed by the NowCast algorithm. During training with the dataset, our model learns a correlation index through supervised learning, which improves the model's ability to act as a Picture-based Predictor of PM2.5 Concentration (PPPC) making it capable of detecting comparable daily aggregated AQI index from a photo captured by a smartphone. The code and dataset is made publicly available [here](#).

## Paper

* Published to Nature Scientific Reports. [Link](https://www.nature.com/articles/s41598-023-51015-1?).

## Dataset

The dataset is available via [Zenodo](https://zenodo.org/record/7878371).

## Repository Structure

```
aqi/
├── Codes/
│   ├── 2.0airwire_optimum-final-documented.ipynb
│   ├── 2.0airwire_optimum-k-fold-results - Compiled.ipynb
│   ├── 2.0airwire_optimum_other_models-with-pretrained-weights.ipynb
│   ├── for tensorflow in ubuntu.txt
│   └── k-fold-results.txt
├── environment.yml
├── LICENSE
└── README.md
```

## Installation

### 1. Clone the repository

```
git clone https://github.com/lepotatoguy/aqi.git
cd aqi
```

### 2. Install dependencies

```
conda env create -f environment.yml
```

We use these commands below to create the environment.

```
conda create -n aqi-net python=3.10
conda activate aqi-net
conda install -y -c conda-forge tensorflow-gpu seaborn pandas matplotlib opencv ipykernel tqdm scikit-learn
```

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

# Move the dataset to codes directory
mv dataset2.0 ../Codes/dataset
mv dataset.xlsx ../Codes/
```

## Usage

```
# Go to the Codes directory
cd ../Codes/

## Launch Jupyter Notebook or VS Code or your prefered software to launch the code
# jupyter notebook 2.0airwire_optimum-final-documented.ipynb
# code 2.0airwire_optimum-final-documented.ipynb
```

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
