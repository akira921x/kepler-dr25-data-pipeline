# Kepler DR25 Preprocessing Pipeline

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Altair](https://img.shields.io/badge/Altair-Grammar_of_Graphics-brightgreen.svg)](https://altair-viz.github.io/)

A comprehensive data preprocessing pipeline for Kepler Data Release 25, designed for exoplanet detection and classification research. This pipeline transforms raw astronomical data into research-grade datasets suitable for machine learning applications while maintaining strict scientific standards. Compatible with ExoMiner methodology and other machine learning frameworks.

## Overview

The Kepler Space Telescope's Data Release 25 represents nine years of unprecedented exoplanet discovery. This preprocessing pipeline standardizes raw Threshold Crossing Event (TCE) and Kepler Objects of Interest (KOI) catalogs from NASA's MAST archive into formats suitable for modern machine learning frameworks including ExoMiner and other detection algorithms.

### Key Features

- **Interactive Data Analysis**: Altair-powered statistical visualizations using grammar of graphics for comprehensive data exploration
- **Scientific Rigor**: Follows NASA Exoplanet Archive standards and published methodologies
- **ML-Ready Outputs**: Optimized datasets for supervised learning and discovery applications
- **Quality Control**: Systematic removal of known artifacts and data validation
- **Unified Architecture**: Single training dataset with complete CANDIDATE preservation for maximum scientific accuracy
- **Comprehensive Documentation**: Complete workflow with scientific references and ExoMiner compatibility notes

## Quick Start

### Prerequisites

```bash
# Required packages
pip install pandas numpy matplotlib seaborn altair vega_datasets jupyter
```

### Data Requirements

**Data Source Attribution**: All data files in this repository's `data/` folder originate from NASA's Exoplanet Archive and are provided for reproducibility purposes only. These represent a specific snapshot used in the original analysis.

**For Research Use**: Researchers should download the latest data directly from NASA to ensure current results and discoveries.

Download the required Kepler DR25 data files from the NASA Exoplanet Archive:

**Data Source**: Visit the [Kepler Mission Data Products](https://exoplanetarchive.ipac.caltech.edu/docs/KeplerMission.html#:~:text=%C2%B9%20The%20Cumulative%20table%20is,catalog%20(Slawson%20et%20al)) page

**Download Instructions**:

1. Navigate to the Kepler Mission Data Products page
2. Locate the **Interactive Tables** section (Note: API calls are no longer supported)
3. Download the following datasets using the **interactive table interface**:
   - **TCE Table**: Threshold Crossing Events - Download **ALL COLUMNS** → Save as `q1_q17_dr25_tce_raw.csv` (~54MB)
   - **KOI Table**: Kepler Objects of Interest - Download **ALL COLUMNS** → Save as `q1_q17_dr25_koi_raw.csv` (~4.6MB)

**Important**:

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

```text
kepler-dr25-data-pipeline/
├── kepler_dr25_exominer_preprocessing.ipynb    # Main analysis notebook
├── data/                                       # Data directory (NASA source)
│   ├── q1_q17_dr25_tce_raw.csv                 # Input: Raw TCE data (from NASA)
│   ├── q1_q17_dr25_koi_raw.csv                 # Input: Raw KOI data (from NASA)
│   └── q1_q17_dr25_tce_train_output.csv        # Output: Unified training dataset
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

#### Stage 2: KOI Processing

- **UID Generation**: Creates standardized identifiers for all KOI records
- **Disposition Analysis**: Processes all KOI classifications (CONFIRMED, FALSE POSITIVE, CANDIDATE)
- **Complete Preservation**: Includes all KOI data for unified training approach

#### Stage 3: TCE-Based Integration with Quality Control

- **Data Validation (DV) Completeness Check**: Ensures robust astronomical data quality
  - Key DV metrics: period, MES, depth, model SNR
  - Optional metrics: centroid offset, odd-even statistics (if available)
  - Physical validity: positive period, positive MES, positive depth
- **Configurable Quality Filters**: Flexible filtering approach for different research needs:
  - MES threshold (configurable, typically > 8.0 for high-significance detections)
  - Stellar temperature filtering (configurable, typically > 3500K for reliable characterization)
  - Transit count ≥ 3 (sufficient observational data)
- **Left-Join Strategy**: Uses TCE as base table, enriched with KOI data where available
- **Enhanced Labeling**: koi_pdisposition='CANDIDATE' → PC, 'FALSE POSITIVE' → AFP, others → NTP
- **Consistent Row Count**: Output maintains TCE record count without creating additional rows

### Output Dataset

#### Unified Training Dataset (`q1_q17_dr25_tce_train_output.csv`)

- **Purpose**: Comprehensive machine learning dataset with complete CANDIDATE preservation
- **Labels**: PC (koi_pdisposition='CANDIDATE'), AFP (koi_pdisposition='FALSE POSITIVE'), NTP (others)
- **CANDIDATE Preservation**: All 4,717 CANDIDATE records preserved as PC labels
- **Data Quality Control**: Two-tier filtering approach
  - **DV Completeness**: Key Data Validation metrics availability and physical validity
  - **ExoMiner Filters**: MES > 8.0, T_eff > 3500K, ≥3 transits
- **Architecture**: Outer join ensures no CANDIDATE loss during processing
- **Scientific Rigor**: Follows NASA standards for astronomical data validation
- **Size**: Complete dataset with all planetary candidates included
- **Applications**: Machine learning training, algorithm development, comprehensive evaluation

## Visualization Features

The notebook includes comprehensive interactive visualizations:

- **Scientific Dashboard**: 6-panel analysis with data flow, quality metrics, and impact indicators
- **Interactive Charts**: Altair-powered exploration with hover details and statistical graphics using grammar of graphics
- **Quality Heatmaps**: Color-coded data completeness and integrity assessment
- **Class Balance Analysis**: Visual representation of training data distribution
- **ExoMiner 2022 Comparison**: Side-by-side statistical and visual comparison with original paper dataset
- **Benchmark Analysis**: Quantitative assessment showing +16.9% improvement in planet candidate preservation
- **Historical Comparison**: Comprehensive benchmarking against original ExoMiner study results

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

### Version 1.9.0 - Enhanced Labeling System and Filter Configuration

- **NEW Labeling Rules** - Updated to use koi_disposition instead of koi_pdisposition for improved accuracy:
  - koi_disposition='CONFIRMED' → PC (Planet Candidates)
  - koi_disposition='FALSE POSITIVE' → AFP (Astrophysical False Positives)
  - Remaining TCE records → NTP (Non-Transit-like Phenomena)
- **Disabled ExoMiner Quality Filters** - Removed automatic filtering for broader dataset inclusion:
  - MES > 8.0 filter: DISABLED
  - Teff > 3500K filter: DISABLED
  - Transit count >= 3 filter: DISABLED
- **Enhanced Data Completeness** - Preserves more astronomical data while maintaining scientific integrity
- **Updated Documentation** - All cells and markdown updated to reflect new labeling system
- **Improved Scientific Accuracy** - Uses confirmed planets as positive examples instead of candidates

### Version 1.8.1 - Variable Definition and CSV Output Fixes

- **Fixed Variable Scoping** - Resolved final_df undefined variable issues in summary reporting
- **Enhanced CSV Output** - Added automatic CSV file saving within merge_tce_koi_data() function
- **Improved Error Handling** - Better exception handling for missing datasets in pipeline execution
- **Code Quality Improvements** - Fixed variable reference consistency and enhanced robustness
- **Documentation Updates** - Improved variable flow documentation for better maintainability

### Version 1.8.0 - Enhanced Three-Class Labeling and Filter Optimization

- **Three-Class Labeling System** - Implemented comprehensive PC/AFP/NTP classification for improved machine learning applications
- **Enhanced Label Logic** - CANDIDATE → PC, FALSE POSITIVE → AFP, all others → NTP for complete astronomical classification
- **Optimized MES Filter** - Refined MES > 8.0 threshold for optimal balance between data quality and sample size
- **Improved Target ID Handling** - Ensured integer type consistency for target_id field across all processing stages
- **Enhanced Documentation** - Updated all markdown cells and documentation to reflect three-class system
- **Robust Error Handling** - Fixed NameError issues and improved variable scoping in final summary reporting

### Version 1.7.0 - Optimized Field Selection and Output Standardization

- **Streamlined field selection** - Output limited to essential 33 fields for enhanced performance and compatibility
- **Standardized output format** - Includes uid, target_id, and all required TCE/KOI analysis fields
- **Improved data consistency** - Automatic handling of duplicate columns and field mapping
- **Enhanced file efficiency** - Reduced output file size from 33MB to 5.9MB while maintaining full scientific value
- **Robust error handling** - Fixed duplicate column issues in final processing stage
- **Complete field coverage** - All core required, diagnostic, stellar, and planet parameter fields included

### Version 1.6.0 - Data Processing and Merge Logic Improvements

- **Enhanced merge strategy** - Changed from outer join to left join using TCE as base table
- **Flexible quality filtering** - Configurable MES and stellar temperature filters for different research needs
- **Improved data flow** - TCE records enriched with KOI labels without creating additional rows
- **Better labeling logic** - Clear distinction between PC (Planet Candidates) and FP (False Positives)
- **Enhanced documentation** - Clearer explanations of merge strategy and filtering approach

### Version 1.5.1 - Bug Fixes and Stability Improvements

- **Fixed variable scoping issues** - Resolved NameError in final summary cell
- **Enhanced error handling** - Better detection of processing stage completion
- **Improved privacy protection** - Removed sensitive path information from configuration output
- **Code modernization** - Updated imports with better type annotation support
- **Stability improvements** - More robust notebook execution flow

### Version 1.5.0 - Clean Interface and Code Quality

- **Clean interface design** - Removed all decorative elements for professional presentation
- **Improved code readability** - Streamlined notebook outputs with focus on data and results
- **Enhanced documentation quality** - Professional markdown formatting with proper structure
- **Maintained scientific accuracy** - All analytical capabilities and data processing remain unchanged
- **Professional presentation** - Clean, distraction-free interface suitable for academic and research use
- **ExoMiner 2022 paper comparison** with comprehensive statistical analysis and validation
- **Enhanced planet candidate preservation** - 4,717 PC vs 4,034 in original (+16.9% improvement)
- **Quantitative benchmark analysis** showing superior CANDIDATE preservation strategy
- **Visual comparison dashboard** demonstrating dataset composition improvements
- **Scientific validation framework** with side-by-side methodology comparison
- **Data Validation (DV) completeness check** ensuring robust astronomical data quality
- **Two-tier filtering approach** with DV metrics validation before ExoMiner filters
- **Complete CANDIDATE preservation** with unified preprocessing pipeline (all 4,717 preserved)
- **Outer join architecture** ensuring no CANDIDATE loss during processing
- **Enhanced documentation** with benchmark comparison and scientific impact assessment

**Ready for research applications, educational use, and machine learning development!**

---

*This project represents a significant advancement in automated exoplanet detection preprocessing, providing the astronomical community with modern, efficient tools for exploring the cosmos and discovering new worlds.*
