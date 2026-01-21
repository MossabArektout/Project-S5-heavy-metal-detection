# AI Detection of Heavy Metals in Agricultural Water

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)

A machine learning system for detecting and quantifying heavy metal contamination (Chromium and Arsenic) in agricultural water using SERS (Surface-Enhanced Raman Spectroscopy) data.

## Overview

This project implements a **hierarchical classification architecture** combining SERS spectroscopy with machine learning to:
- **Identify** the type of contamination (Chrome Cr⁶⁺ / Arsenic As / Control)
- **Quantify** the concentration level (9-13 classes per metal)
- **Compare** multiple ML algorithms (SVM, Random Forest, KNN, Gradient Boosting)

### Key Results

| Model | Task | Accuracy | F1-Score |
|-------|------|----------|----------|
| Model 1 | Metal Type Classification | **92.01%** | 0.92 |
| Model 2a | Chrome Concentration (9 classes) | **90.60%** | 0.91 |
| Model 2b | Arsenic Concentration (13 classes) | **94.23%** | 0.91 |

## Architecture

```
                    ┌─────────────┐
                    │   Input     │
                    │  Spectrum   │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │   Model 1   │
                    │  Type Class │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
    ┌─────▼─────┐    ┌─────▼─────┐    ┌─────▼─────┐
    │  Control  │    │  Chrome   │    │  Arsenic  │
    │   (Safe)  │    │   (Cr)    │    │   (As)    │
    └───────────┘    └─────┬─────┘    └─────┬─────┘
                           │                │
                    ┌──────▼──────┐  ┌──────▼──────┐
                    │  Model 2a   │  │  Model 2b   │
                    │ Cr Conc.    │  │ As Conc.    │
                    └─────────────┘  └─────────────┘
```

## Project Structure

```
heavy-metal-detection/
│
├── README.md                 # This file
├── requirements.txt          # Python dependencies
├── .gitignore               # Git ignore rules
├── LICENSE                  # MIT License
│
├── data/
│   ├── raw/
│   │   ├── chromium/        # Cr⁶⁺ SERS spectra (10 classes)
│   │   └── arsenic/         # As SERS spectra (14 classes)
│   └── README.md            # Data download instructions
│
├── notebooks/
│   ├── 01_chromium_analysis.ipynb      # Chrome analysis & Model 2a
│   ├── 02_arsenic_analysis.ipynb       # Arsenic analysis & Model 2b
│   └── 03_hierarchical_model.ipynb     # Combined hierarchical system
│
├── docs/
│   ├── report.pdf           # Full project report (French)
│   ├── presentation.pdf     # Defense presentation
│   └── reference_paper.pdf  # Wei et al. 2023 paper
│
└── figures/                 # Generated visualizations
```

## Installation

### Prerequisites
- Python 3.8 or higher
- pip package manager

### Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/heavy-metal-detection.git
cd heavy-metal-detection

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Data Setup

The raw SERS data files are not included due to size (~2.2 GB). See [data/README.md](data/README.md) for download instructions.

## Usage

### Running the Notebooks

```bash
# Start Jupyter
jupyter notebook

# Then open notebooks in order:
# 1. notebooks/01_chromium_analysis.ipynb
# 2. notebooks/02_arsenic_analysis.ipynb
# 3. notebooks/03_hierarchical_model.ipynb
```

### Quick Start

```python
# Example: Load and preprocess data
import numpy as np
from scipy.signal import savgol_filter
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.svm import SVC

# Preprocessing pipeline
# 1. Baseline correction (ALS)
# 2. Smoothing (Savitzky-Golay)
# 3. Normalization (StandardScaler)
# 4. Dimensionality reduction (PCA: 1011 → 22)

# Train SVM classifier
model = SVC(kernel='rbf', C=1.0, gamma='scale')
model.fit(X_train, y_train)
```

## Methodology

### Data Processing Pipeline

1. **Baseline Correction (ALS)** - Remove fluorescence background
2. **Smoothing (Savitzky-Golay)** - Reduce high-frequency noise while preserving peaks
3. **Normalization (StandardScaler)** - Zero mean, unit variance
4. **Dimensionality Reduction (PCA)** - 1011 → 22 components (95% variance retained)
5. **Class Balancing (SMOTE)** - Synthetic oversampling for minority classes

### Algorithms Compared

| Algorithm | Principle | Best For |
|-----------|-----------|----------|
| **SVM (RBF)** | Optimal hyperplane in high-dimensional space | Spectral data |
| **Random Forest** | Ensemble of decision trees | Robust, interpretable |
| **KNN** | k-nearest neighbors voting | Simple, fast |
| **Gradient Boosting** | Sequential error correction | High accuracy |

## Dataset

**Source**: Wei et al. (2023) - PNAS

| Property | Value |
|----------|-------|
| Technique | SERS (Surface-Enhanced Raman Spectroscopy) |
| Biosensor | *E. coli* bacteria |
| Total Spectra | 42,400 |
| Points per Spectrum | 1,011 |
| Wavenumber Range | 508 - 1640 cm⁻¹ |
| Chromium Classes | 10 (Control + 9 concentrations) |
| Arsenic Classes | 14 (Control + 13 concentrations) |

## Results

### Model 1: Metal Type Classification
- **Best Algorithm**: SVM (92.01%)
- **Task**: Classify spectra as Control / Chromium / Arsenic

### Model 2a: Chromium Concentration
- **Best Algorithm**: SVM (90.60%)
- **Classes**: 9 concentration levels (0.68 pM → 68 μM)

### Model 2b: Arsenic Concentration
- **Best Algorithm**: SVM (94.23%)
- **Classes**: 13 concentration levels (5×10⁻³ pM → 5 M)

## Why Classification Instead of Regression?

The concentrations are **logarithmically spaced** (pM → nM → μM → M) with no intermediate values in the dataset. Classification naturally fits this discrete structure, while regression would assume a continuity that doesn't exist in the experimental data.

## Limitations

- Data from laboratory conditions (no field validation)
- Single-metal detection only (no multi-contamination)
- Error propagation risk in hierarchical architecture
- Confusion between adjacent concentration classes

## Future Work

- Extension to other heavy metals (Pb, Cd, Hg)
- Multi-contamination detection
- Deep Learning (CNN 1D) exploration
- Streamlit web interface
- Portable IoT sensor integration

## References

```bibtex
@article{wei2023decoding,
  title={Decoding the metabolic response of Escherichia coli for sensing trace heavy metals in water},
  author={Wei, Hong and Huang, Yixin and Santiago, Peter J and others},
  journal={Proceedings of the National Academy of Sciences},
  volume={120},
  number={7},
  pages={e2210061120},
  year={2023},
  publisher={National Acad Sciences}
}
```

## Author

**Mossab Arektout**
- Email: arektoutmossab@gmail.com
- Institution: ENSIAS, Université Mohammed V de Rabat
- Program: Data Science and IoT (IDSIT)
- Supervisor: Pr. MOUMANE Karima

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*Project S5 - January 2025*
