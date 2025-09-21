# Kepler DR25 Preprocessing Pipeline

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Altair](https://img.shields.io/badge/Altair-Grammar_of_Graphics-brightgreen.svg)](https://altair-viz.github.io/)

A comprehensive data preprocessing pipeline for Kepler Data Release 25, designed for exoplanet detection and classification research. This pipeline transforms raw astronomical data into research-grade datasets suitable for machine learning applications while maintaining strict scientific standards. Compatible with ExoMiner methodology and other machine learning frameworks.

## 🌟 Overview

The Kepler Space Telescope's Data Release 25 represents nine years of unprecedented exoplanet discovery. This preprocessing pipeline standardizes raw Threshold Crossing Event (TCE) and Kepler Objects of Interest (KOI) catalogs from NASA's MAST archive into formats suitable for modern machine learning frameworks including ExoMiner and other detection algorithms.

### Key Features

- **📊 Interactive Data Analysis**: Altair-powered statistical visualizations using grammar of graphics for comprehensive data exploration
- **🔬 Scientific Rigor**: Follows NASA Exoplanet Archive standards and published methodologies
- **🤖 ML-Ready Outputs**: Optimized datasets for supervised learning and discovery applications
- **📈 Quality Control**: Systematic removal of known artifacts and data validation
- **🔄 Dual-Purpose Architecture**: Separate training and discovery datasets for maximum scientific impact
- **📝 Comprehensive Documentation**: Complete workflow with scientific references and ExoMiner compatibility notes

## 🚀 Quick Start

### Prerequisites

```bash
# Required packages
pip install pandas numpy matplotlib seaborn altair vega_datasets jupyter
```

### Data Requirements

**📂 Data Source Attribution**: All data files in this repository's `data/` folder originate from NASA's Exoplanet Archive and are provided for reproducibility purposes only. These represent a specific snapshot used in the original analysis.

**🔬 For Research Use**: Researchers should download the latest data directly from NASA to ensure current results and discoveries.

Download the required Kepler DR25 data files from the NASA Exoplanet Archive:

**🔗 Data Source**: Visit the [Kepler Mission Data Products](https://exoplanetarchive.ipac.caltech.edu/docs/KeplerMission.html#:~:text=%C2%B9%20The%20Cumulative%20table%20is,catalog%20(Slawson%20et%20al)) page

**📥 Download Instructions**:
1. Navigate to the Kepler Mission Data Products page
2. Locate the **Interactive Tables** section (Note: API calls are no longer supported)
3. Download the following datasets using the **interactive table interface**:
   - **TCE Table**: Threshold Crossing Events - Download **ALL COLUMNS** → Save as `q1_q17_dr25_tce_raw.csv` (~54MB)
   - **KOI Table**: Kepler Objects of Interest - Download **ALL COLUMNS** → Save as `q1_q17_dr25_koi_raw.csv` (~4.6MB)

**⚠️ Important**:
- Use the **interactive table** interface only (API service discontinued)
- Download **ALL COLUMNS** to ensure compatibility with the preprocessing pipeline
- Save files with the exact names specified above
- **For research purposes**: Always use the latest data from NASA for current analysis
- **For reproducibility**: The included data files represent the specific snapshot used in this analysis

Place these files in the `data/` directory.

### Usage

```bash
# Clone the repository
git clone https://github.com/akira921x/kepler-dr25-data-pipeline.git
cd kepler-dr25-data-pipeline

# Launch Jupyter notebook
jupyter notebook kepler_dr25_exominer_preprocessing.ipynb
```

**Note**: This is an independent preprocessing pipeline compatible with ExoMiner methodology, not an official ExoMiner project.

## Project Structure

```
kepler-dr25-data-pipeline/
├── kepler_dr25_exominer_preprocessing.ipynb    # Main analysis notebook
├── data/                                       # Data directory (NASA source)
│   ├── q1_q17_dr25_tce_raw.csv                 # Input: Raw TCE data (from NASA)
│   ├── q1_q17_dr25_koi_raw.csv                 # Input: Raw KOI data (from NASA)
│   ├── q1_q17_dr25_tce_train_output.csv        # Output: Training dataset
│   └── q1_q17_dr25_tce_candidate_output.csv    # Output: Discovery dataset
├── requirements.txt                            # Python package dependencies
├── LICENSE                                     # MIT License
├── .gitignore                                  # Git ignore rules
└── README.md                                   # This file
```

## Scientific Methodology

### Data Processing Pipeline

The pipeline implements a rigorous 3-stage process:

#### Stage 1: TCE Quality Control
- **Rogue TCE Filtering**: Removes systematic artifacts (tce_rogue_flag = 1)
- **UID Generation**: Creates standardized Kepler identifiers (`kplrKKKKKKKKK-PP`)
- **Data Validation**: Ensures scientific integrity and completeness

#### Stage 2: KOI Classification
- **Strategic Partitioning**: Separates candidates for discovery applications
- **Disposition Analysis**: Processes CONFIRMED, FALSE POSITIVE, and CANDIDATE classifications
- **Training Optimization**: Excludes candidates to ensure clean labels

#### Stage 3: Cross-Catalog Integration
- **Left-Join Strategy**: Preserves complete TCE detection catalog
- **ExoMiner Quality Filters**: Applies filters following Valizadegan et al. (2022) methodology:
  - MES > 8.0 (high-significance detections)
  - Stellar temperature > 3500K (reliable stellar characterization)
  - Transit count ≥ 3 (sufficient observational data)
- **Binary Labeling**: Assigns PC (Planet Candidate) vs AP (Astrophysical Phenomenon) labels
- **Column Standardization**: Implements consistent ordering and formatting

### Output Datasets

#### Training Dataset (`q1_q17_dr25_tce_train_output.csv`)
- **Purpose**: Supervised machine learning with definitive labels
- **Labels**: PC (CONFIRMED KOIs) vs AP (all other TCEs)
- **Quality Filters**: ExoMiner-compliant filtering (MES > 8.0, T_eff > 3500K, ≥3 transits)
- **Size**: High-quality labeled examples after filtering
- **Applications**: ExoMiner training, algorithm development, performance evaluation

#### Discovery Dataset (`q1_q17_dr25_tce_candidate_output.csv`)
- **Purpose**: Future exoplanet discovery through classification
- **Content**: KOI candidates excluded from training
- **Size**: ~4,000+ candidate objects
- **Applications**: Model application, validation studies, new discoveries

## Visualization Features

The notebook includes comprehensive interactive visualizations:

- **Scientific Dashboard**: 6-panel analysis with data flow, quality metrics, and impact indicators
- **Interactive Charts**: Altair-powered exploration with hover details and statistical graphics using grammar of graphics
- **Quality Heatmaps**: Color-coded data completeness and integrity assessment
- **Class Balance Analysis**: Visual representation of training data distribution
- **Historical Comparison**: Benchmarking against original ExoMiner study results

## Scientific References

### Primary Reference
**Valizadegan, H., et al. 2022, *Astrophysical Journal*, 926, 120**
"ExoMiner: A Highly Accurate and Explainable Deep Learning Classifier for Exoplanet Detection"
[arXiv:2111.10009](https://arxiv.org/abs/2111.10009)

### Supporting Documentation
- **NASA Exoplanet Archive**: [https://exoplanetarchive.ipac.caltech.edu/](https://exoplanetarchive.ipac.caltech.edu/)
- **Kepler Data Processing Handbook**: Thompson et al. 2016, KSCI-19081-003
- **DR25 Catalog Papers**: Thompson et al. 2018, ApJS 235, 38
- **Robovetter Documentation**: Coughlin et al. 2016, ApJS 224, 12

## Performance Metrics

### Data Processing Efficiency
- **Input Processing**: ~200,000+ astronomical records
- **Quality Filtering**: Systematic artifact removal (rogue TCEs)
- **Strategic Partitioning**: Optimized train/discovery split
- **Processing Time**: ~5-10 minutes on standard hardware

### Scientific Impact
- **Class Balance**: Maintains realistic astronomical ratios
- **Data Currency**: Uses most recent NASA archive updates
- **Reproducibility**: Standardized methodology with full documentation
- **Compatibility**: Works with modern ML frameworks (TensorFlow, PyTorch, scikit-learn)

## Technical Requirements

### Software Dependencies
- **Python**: 3.8+ (recommended: 3.9+)
- **Core Libraries**: pandas, numpy, matplotlib, seaborn
- **Visualization**: altair, vega_datasets, jupyter
- **Memory**: 8GB+ RAM recommended for full dataset processing
- **Storage**: 2GB+ available space for data and outputs

### System Compatibility
- **Operating Systems**: macOS, Linux, Windows
- **Jupyter**: Notebook Server 6.0+
- **Browser**: Modern browsers with JavaScript support for interactive visualizations

## Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests.

### Development Guidelines
1. **Scientific Accuracy**: All changes must maintain scientific rigor
2. **Documentation**: Update documentation for any methodology changes
3. **Testing**: Verify processing with sample datasets
4. **Standards**: Follow NASA Exoplanet Archive conventions

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- **NASA Exoplanet Archive** for maintaining authoritative astronomical data products
- **Kepler Mission Team** for nine years of groundbreaking exoplanet science
- **ExoMiner Authors** for establishing machine learning methodology standards
- **Open Source Community** for providing excellent tools for scientific computing

## Support

For questions about the scientific methodology or technical implementation:

- **Issues**: [GitHub Issues](https://github.com/akira921x/kepler-dr25-data-pipeline/issues)
- **Documentation**: See the Jupyter notebook for detailed methodology and implementation
- **NASA Resources**: [Exoplanet Archive Help](https://exoplanetarchive.ipac.caltech.edu/docs/)

---

## Current Status

**Version 1.0 - Production Ready**

- Complete preprocessing pipeline implementation
- Interactive Altair visualization dashboard
- Comprehensive scientific documentation
- Dual-purpose dataset architecture
- NASA standards compliance
- ExoMiner methodology compatibility

**Ready for research applications, educational use, and machine learning development!**

---

*This project represents a significant advancement in automated exoplanet detection preprocessing, providing the astronomical community with modern, efficient tools for exploring the cosmos and discovering new worlds.*