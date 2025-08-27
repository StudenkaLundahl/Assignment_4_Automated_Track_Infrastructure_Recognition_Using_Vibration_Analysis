# Assignment_4_Automated_Track_Infrastructure_Recognition_Using_Vibration_Analysis
Develop an algorithm that detects railway infrastructure events (bridges, rail joints, turnouts) from synchronized GPS and vibration sensor data.

# Automated Track Infrastructure Recognition Using Vibration Analysis

A comprehensive machine learning pipeline for detecting railway infrastructure (bridges, rail joints, turnouts) from train-mounted vibration sensors and GPS data.

## 📋 Project Overview

This project implements a complete Grade 3-5 solution for automated railway infrastructure monitoring:

- **Grade 3 (Mapping)**: Infrastructure visualization and GPS data quality validation
- **Grade 4 (Labeling)**: Vibration segment labeling with categorical infrastructure types  
- **Grade 5 (Classification)**: Machine learning models for automated infrastructure detection

### Key Achievements

- **8.6x improvement** in RailJoint detection coverage (173 vs 20 reference points)
- **716 labeled segments** across 4 validated measurement sessions
- **70.2% classification accuracy** with Logistic Regression model
- **Real-time processing** capability for 600MB+ vibration files

## 🗂️ Repository Structure

```
Project_Folder/
│
├── Code/                           # Analysis notebooks and scripts
│   ├── Code_1_SL_v9.ipynb         # Grade 3: Infrastructure mapping
│   ├── Code_2_SL_v10.ipynb        # Grade 4: Vibration labeling  
│   └── Code_3_SL_v8.ipynb         # Grade 5: ML classification
│
├── Data_1/                        # Infrastructure reference data
│   ├── converted_coordinates_Resultat_Bridge.csv
│   ├── converted_coordinates_Resultat_RailJoint.csv
│   ├── converted_coordinates_Turnout.csv
│   └── Emaint_Bandel_331_Borlänge_Mora_Turnout.xlsx  # Primary source
│
├── Data_2/                        # Train measurement data (139 folders)
│   ├── 2024-12-10_10-00-00_(1)/  # ✓ Validated dataset
│   ├── 2024-12-10_12-00-00_(1)/  # ✓ Validated dataset  
│   ├── 2024-12-10_16-00-00_(1)/  # ✓ Validated dataset
│   ├── 2024-12-12_10-00-00_(1)/  # ✗ Excluded (sensor malfunction)
│   ├── 2024-12-12_12-00-00_(1)/  # ✓ Validated dataset
│   └── [135 other folders]/       # Failed validation (will not be added here)
│   
│   # Each valid folder contains:
│   ├── GPS.latitude.csv           # GPS coordinates (~0.4MB)
│   ├── GPS.longitude.csv          
│   ├── GPS.speed.csv              # Train speed data
│   ├── GPS.satellites.csv         # Signal quality
│   ├── CH1_ACCEL1Z1.csv          # Left rail vibration (~650MB)
│   └── CH2_ACCEL1Z2.csv          # Right rail vibration (~620MB)
│
├── Output_Files/                  # Generated results
│   ├── Code1_outputs/             # Infrastructure maps and validation
│   ├── Code2_outputs/             # Labeled segment datasets
│   └── Code3_outputs/             # ML models and analysis
│
└── Report/                        # Documentation
    └── Assignment_4_Report_Studenka_Lundahl.pdf
```

## 🚀 Getting Started

### Prerequisites

```bash
pip install pandas numpy scikit-learn tensorflow plotly dash pyproj
```

### Data Requirements

- **Data_1**: Infrastructure database (provided Excel file)
- **Data_2**: Train measurement folders with GPS and vibration CSV files

### Usage Instructions

#### Step 1: Infrastructure Mapping (Grade 3)
```bash
# Run Code_1_SL_v9.ipynb
# Generates: infrastructure_points.csv, valid_folders.txt
```

#### Step 2: Vibration Labeling (Grade 4)  
```bash
# Run Code_2_SL_v10.ipynb (requires Code 1 outputs)
# GUI will appear for folder selection
# Generates: SL_labeled_segments_*.csv files
```

#### Step 3: ML Classification (Grade 5)
```bash
# Run Code_3_SL_v8.ipynb (requires Code 2 outputs)  
# Auto-detects and combines all labeled segment files
# Generates: trained models and performance analysis
```

## 📊 Data Quality Pipeline

### Rigorous 8-Criteria Validation System

Out of 139 measurement folders, only 4 passed validation:

1. **File Completeness**: All 6 required CSV files present
2. **GPS Signal Quality**: Minimum 4 satellites for positioning
3. **Route Validation**: Coordinates within Mora-Borlänge corridor  
4. **Movement Detection**: Eliminate stationary recordings
5. **Distance Requirements**: Minimum 6km track coverage
6. **Temporal Consistency**: Complete 30-minute recordings
7. **Infrastructure Coverage**: Meaningful infrastructure encounters  
8. **Sensor Integrity**: Manual inspection for hardware malfunctions

**Result**: 3.6% folder acceptance rate prioritizing quality over quantity

## 🧠 Machine Learning Pipeline

### Feature Extraction
- **60 features per segment**: Time domain, frequency domain, cross-channel correlation
- **10-second segments**: 5,000 samples at 500Hz vibration data
- **GPS-Vibration sync**: 99.2% of segments achieve <1s alignment accuracy

### Model Performance

| Model | Accuracy | F1-Score | Cross-Validation |
|-------|----------|----------|------------------|
| **Logistic Regression** | **70.2%** | **0.675** | 0.685±0.024 |
| Gradient Boosting | 69.3% | 0.673 | 0.665±0.025 |
| Random Forest | 68.8% | 0.663 | 0.691±0.022 |
| SVM | 70.7% | 0.655 | 0.647±0.015 |
| Dense Neural Network | 67.0% | 0.615 | - |
| 1D CNN | 66.0% | 0.629 | - |

### Infrastructure Detection Results

- **Normal Track**: 468 segments (65.4%) - Strong baseline classification
- **RailJoint**: 190 segments (26.5%) - Primary infrastructure type  
- **Turnout**: 34 segments (4.7%) - Limited but adequate samples
- **Bridge**: 24 segments (3.4%) - Challenging minority class

## 🔧 Technical Implementation

### Memory Management
- Efficient processing of 600MB+ vibration files
- Memory-safe loading with ~14MB RAM usage
- Real-time segment processing capability

### Coordinate System Handling  
- SWEREF99 TM to WGS84 conversion using pyproj
- Millimeter-precision coordinate accuracy
- Adaptive distance thresholds: Bridge (150m), Turnout (90m), RailJoint (60m)

### Interactive Analysis
- HTML-based interactive dashboards 
- Real-time vibration plot generation
- GPS-map click functionality for segment exploration

## 📈 Results and Performance

### Key Findings

**Strengths:**
- Multi-dataset approach provides robust generalization
- Clear vibration signatures for different infrastructure types
- Production-ready pipeline with quality control measures

**Limitations:**  
- Bridge detection challenges due to limited training samples
- GPS accuracy dependencies (±3-5m uncertainty)
- Single railway corridor limits broader applicability

### Deployment Readiness

**Recommended Model**: Logistic Regression
- Best F1-score performance (0.675)
- Computational efficiency for real-time deployment
- Stable cross-validation results
- Suitable for continuous railway monitoring

## 🔄 Continuous Improvement

### Future Enhancements
1. **Targeted data collection** for minority classes (bridges, turnouts)
2. **Multi-route validation** across different railway corridors  
3. **Seasonal variation** data collection
4. **Ensemble methods** combining top-performing models
5. **Higher precision GPS** for improved labeling accuracy

## 📚 Documentation

- **Complete Technical Report**: Assignment_4_Report_Studenka_Lundahl.pdf
- **Interactive Results**: HTML visualization files in Output_Files/
- **Model Artifacts**: Trained models and feature extractors ready for deployment

## 👤 Author

**Studenka Lundahl**  
Student at Luleå University of Technology

---

*This project demonstrates a complete pipeline from raw sensor data to deployable machine learning models for automated railway infrastructure monitoring.*
