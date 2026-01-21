# Data Directory

## Overview

This directory contains the SERS spectral data used for heavy metal detection. Due to file size limitations on GitHub, the raw data files are not included in this repository.

## Data Source

The data comes from:

**Wei, H., Huang, Y., Santiago, P. J., et al. (2023)**
*"Decoding the metabolic response of Escherichia coli for sensing trace heavy metals in water"*
PNAS, 120(7), e2210061120
DOI: [10.1073/pnas.2210061120](https://doi.org/10.1073/pnas.2210061120)

## How to Download

1. Visit the [PNAS supplementary materials](https://www.pnas.org/doi/10.1073/pnas.2210061120)
2. Download the SERS spectral data files
3. Place them in the following structure:

```
data/
├── raw/
│   ├── chromium/
│   │   ├── 00_Control.txt
│   │   ├── 01_0.68 pM.txt
│   │   ├── 02_6.8 pM.txt
│   │   ├── 03_68 pM.txt
│   │   ├── 04_0.68 nM.txt
│   │   ├── 05_6.8 nM.txt
│   │   ├── 06_68 nM.txt
│   │   ├── 07_0.68 uM.txt
│   │   ├── 08_6.8 uM.txt
│   │   └── 09_68 uM.txt
│   │
│   └── arsenic/
│       ├── 00_Control.txt
│       ├── 01_5x10^-3 pM.txt
│       ├── 02_5x10^-2 pM.txt
│       ├── ... (14 concentration levels)
│       └── 13_5 M.txt
```

## Data Structure

Each `.txt` file contains SERS spectra with the following columns:
- **Columns 1-2**: Position information (x, y coordinates)
- **Column 3**: Wavenumber (cm⁻¹) - range: 508-1640 cm⁻¹
- **Column 4**: Intensity (arbitrary units)

## Dataset Statistics

| Metal | Spectra | Classes | Concentration Range |
|-------|---------|---------|---------------------|
| Chromium (Cr⁶⁺) | 18,800 | 10 | 0.68 pM - 68 μM |
| Arsenic (As) | 23,600 | 14 | 5×10⁻³ pM - 5 M |
| **Total** | **42,400** | **24** | - |

## Note

The data files are approximately 2.2 GB in total and are excluded from version control via `.gitignore`.
