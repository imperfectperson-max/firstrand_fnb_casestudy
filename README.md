# FNB Fraud Detection Analysis

A comprehensive machine learning project for detecting fraudulent transactions using advanced analytics and multiple classification algorithms.

![Status](https://img.shields.io/badge/status-complete-success)
![Python](https://img.shields.io/badge/python-3.8+-blue)
![License](https://img.shields.io/badge/license-MIT-green)

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Dataset Information](#dataset-information)
- [Key Findings](#key-findings)
- [Model Performance](#model-performance)
- [Installation & Setup](#installation--setup)
- [Usage Guide](#usage-guide)
- [Project Structure](#project-structure)
- [Methodology](#methodology)
- [Results & Insights](#results--insights)
- [Recommendations](#recommendations)
- [Contributing](#contributing)

## 🎯 Project Overview

This project develops and evaluates multiple machine learning models to detect fraudulent transactions for FNB (First National Bank). The analysis encompasses comprehensive data exploration, feature engineering, model training, and business-oriented threshold optimization.

### Objectives

- Identify fraudulent transactions with high accuracy and precision
- Minimize false positives to reduce investigation costs
- Provide actionable insights for fraud prevention
- Deliver production-ready models with optimal business value

### Business Impact

- **Fraud Detection Rate**: 49.0% of actual fraud cases caught
- **Precision**: 98.0% accuracy when flagging fraudulent transactions
- **Cost Optimization**: Optimal threshold yields estimated savings of R1,930 per 100 transactions
- **Risk Reduction**: 98% confidence in fraud predictions minimizes customer disruption

## 📊 Dataset Information

### Overview

- **Total Transactions**: 10,000
- **Features**: 25 columns
- **Fraud Rate**: ~10% (imbalanced dataset)
- **Time Period**: Historical transaction data
- **Geographic Coverage**: Multiple countries

### Features Breakdown

| Category | Features | Description |
|----------|----------|-------------|
| **Transaction Details** | amount, transaction_channel, card_type | Core transaction attributes |
| **Temporal** | timestamp, prev_timestamp, time_since_last_tx, hour | Time-based patterns |
| **Geographic** | country, lat, lon, prev_lat, prev_lon, distance_from_last_tx | Location data |
| **Behavioral** | velocity_last_10min, km_per_min, device_change | User behavior indicators |
| **Risk Factors** | high_risk_country, is_night, merchant_risk_score | Pre-computed risk flags |
| **Identifiers** | transaction_id, user_id, device_id, merchant | Unique identifiers |

### Data Quality

- **Missing Values**: 74 records in merchant field (0.74%)
- **Duplicates**: None detected
- **Data Types**: Mixed (numeric, categorical, datetime)
- **Preprocessing**: Missing values filled, timestamps converted, features scaled

## 🔍 Key Findings

### Top 5 Fraud Indicators

1. **Hour (Time Patterns)** - Importance: 0.7588 ± 0.3166
   - Certain hours show significantly higher fraud rates
   - Night-time transactions carry elevated risk

2. **Amount (Transaction Value)** - Importance: 0.4984 ± 0.4305
   - Large deviations from user's normal spending patterns
   - Unusually high or low amounts flag suspicious activity

3. **Velocity (Last 10 Minutes)** - Importance: 0.4840 ± 0.3000
   - Multiple transactions in short time windows
   - Geographically impossible transaction sequences

4. **Device Change** - Importance: 0.4776 ± 0.4330
   - New or different device usage
   - Strong indicator when combined with other risk factors

5. **Multiple Risk Factors** - Importance: 0.4260 ± 0.3724
   - Compound risk score from multiple indicators
   - Presence of 2+ risk factors significantly increases fraud likelihood

### Feature Category Analysis

- **Most Important Category**: Time Patterns (avg importance: 0.269)
- **Most Features Required**: 11 features for 80% predictive power
- **Most Stable Feature**: merchant_risk_score
- **Most Variable Feature**: device_change

## 🏆 Model Performance

### Comprehensive Model Comparison

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|-------|----------|-----------|--------|----------|---------|
| **Gradient Boosting** ⭐ | **0.9740** | **0.9800** | 0.49 | **0.6533** | **0.9468** |
| Ensemble Voting | 0.9625 | 0.6263 | **0.62** | 0.6231 | 0.9349 |
| XGBoost | 0.9605 | 0.5929 | **0.67** | 0.6291 | 0.9403 |
| Random Forest | 0.9510 | 0.5111 | 0.46 | 0.4842 | 0.9243 |
| Logistic Regression | 0.8390 | 0.1951 | **0.71** | 0.3060 | 0.8363 |
| Decision Tree | 0.8165 | 0.1637 | 0.65 | 0.2616 | 0.8342 |

⭐ **Selected Final Model**: Gradient Boosting

### Why Gradient Boosting?

- **Highest Precision (98%)**: Minimizes false positives, reducing investigation costs
- **Best F1-Score (0.65)**: Optimal balance between precision and recall
- **Superior ROC-AUC (0.95)**: Excellent overall discriminative ability
- **Business Value**: For every 100 flagged transactions, 98 are actually fraudulent

### Threshold Optimization

- **Optimal Threshold**: 0.10 (adjusted for business costs)
- **Cost Assumptions**:
  - Missed fraud: R100 loss per incident
  - False alarm: R10 investigation cost
  - Fraud caught: R90 benefit per detection
- **Expected Savings**: R1,930 per 100 transactions at optimal threshold

## 🚀 Installation & Setup

### Prerequisites

- Python 3.8 or higher
- pip package manager
- Jupyter Notebook (optional, for notebook exploration)

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/imperfectperson-max/firstrand_fnb_casestudy.git
   cd firstrand_fnb_casestudy
   ```

2. **Create Virtual Environment** (Recommended)
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

### Required Libraries

```
pandas>=1.3.0
numpy>=1.21.0
scikit-learn>=1.0.0
xgboost>=1.5.0
matplotlib>=3.4.0
seaborn>=0.11.0
imbalanced-learn>=0.9.0
joblib>=1.1.0
```

## 💻 Usage Guide

### Quick Start - Using Pre-trained Models

```python
import joblib
import pandas as pd

# Load the final trained model
model = joblib.load('models/final_model.pkl')

# Load the complete pipeline (includes preprocessing)
pipeline = joblib.load('models/fraud_detection_pipeline.pkl')

# Predict on new transaction
new_transaction = pd.DataFrame([{
    'amount': 500.0,
    'transaction_channel': 'online',
    'device_change': 1,
    'hour': 23,
    # ... other features
}])

# Get prediction and probability
prediction = model.predict(new_transaction)
probability = model.predict_proba(new_transaction)[:, 1]

print(f"Fraud Prediction: {'FRAUD' if prediction[0] == 1 else 'LEGITIMATE'}")
print(f"Fraud Probability: {probability[0]:.2%}")
```

### Running the Full Analysis

1. **Explore the Jupyter Notebook**
   ```bash
   jupyter notebook FNB_Fraud_Detection_Analysis.ipynb
   ```

2. **View Existing Results**
   - Model comparisons: `results/model_comparison.csv`
   - Feature analysis: `results/dataset_summary.csv`
   - Business impact: `results/business_impact_analysis.csv`
   - Visualizations: `visualizations/summary_dashboard_*.png`

3. **Access Comprehensive Excel Report**
   ```bash
   # Open in Excel or LibreOffice
   results/FNB_Fraud_Detection_Results_20260127_165934.xlsx
   ```

### Google Colab Setup

For step-by-step instructions on running this analysis in Google Colab, refer to `README.txt` which contains:
- Cell-by-cell setup instructions
- Library installation commands
- Data upload procedures
- Complete analysis workflow

## 📁 Project Structure

```
firstrand_fnb_casestudy/
│
├── README.md                                    # This file - comprehensive documentation
├── README.txt                                   # Google Colab step-by-step guide
├── FNB_Fraud_Detection_Analysis.ipynb          # Main Jupyter notebook (116 cells)
├── requirements.txt                             # Python dependencies
├── .gitignore                                   # Git ignore patterns
│
├── models/                                      # Trained ML models (8 files)
│   ├── final_model.pkl                         # Best performing model (Gradient Boosting)
│   ├── fraud_detection_pipeline.pkl            # Complete preprocessing + model pipeline
│   ├── gradient_boosting.pkl                   # Gradient Boosting classifier
│   ├── xgboost.pkl                             # XGBoost classifier
│   ├── random_forest.pkl                       # Random Forest classifier
│   ├── decision_tree.pkl                       # Decision Tree classifier
│   ├── logistic_regression.pkl                 # Logistic Regression classifier
│   └── ensemble_voting.pkl                     # Ensemble Voting classifier
│
├── results/                                     # Analysis results and metrics
│   ├── FNB_Fraud_Detection_Results_20260127_165934.xlsx  # Master Excel file
│   ├── model_comparison.csv                    # Performance metrics comparison
│   ├── threshold_analysis.csv                  # Threshold optimization results
│   ├── performance_by_class.csv                # Class-specific performance
│   ├── dataset_summary.csv                     # Dataset statistics
│   └── business_impact_analysis.csv            # Cost-benefit analysis
│
├── visualizations/                              # Generated plots and charts
│   ├── summary_dashboard_20260127_165934.png   # Comprehensive dashboard (PNG)
│   └── summary_dashboard_20260127_165934.svg   # Comprehensive dashboard (SVG)
│
└── reports/                                     # Configuration and scripts
    ├── fraud_detection_script_20260127_165934.py  # Exported Python script
    └── config_20260127_165934.json             # Analysis configuration
```

## 🔬 Methodology

### Step 1: Data Preparation

- **Missing Value Handling**: Median imputation for numeric, mode for categorical
- **Data Type Conversion**: Timestamps parsed, categorical encoding applied
- **Duplicate Removal**: Transaction ID-based deduplication
- **Quality Checks**: 10,000 transactions validated, 25 features confirmed

### Step 2: Exploratory Data Analysis (EDA)

- **Fraud Distribution**: Analyzed class imbalance (~10% fraud rate)
- **Transaction Patterns**: Amount, channel, country, and time analysis
- **Geographic Analysis**: Distance and velocity calculations
- **Behavioral Patterns**: Device changes, merchant risk, time-based patterns
- **Correlation Study**: Feature relationships and multicollinearity checks

### Step 3: Feature Engineering

- **Time-Based Features**: Hour extraction, day of week, is_weekend, time categories
- **Risk Indicators**: High-risk transaction flags, velocity metrics, distance anomalies
- **User Behavior**: Aggregated user statistics, spending patterns, deviation metrics
- **Encoding**: Label encoding for categorical variables (channels, countries, card types)

### Step 4: Model Development

- **Train-Test Split**: 70/30 stratified split maintaining fraud distribution
- **Class Balancing**: SMOTE (Synthetic Minority Over-sampling Technique) applied
- **Feature Scaling**: StandardScaler for distance-based algorithms
- **Models Trained**:
  - Logistic Regression (baseline)
  - Decision Tree
  - Random Forest
  - XGBoost
  - Gradient Boosting ⭐
  - Ensemble Voting

### Step 5: Model Evaluation

- **Metrics Tracked**: Accuracy, Precision, Recall, F1-Score, ROC-AUC
- **Cross-Validation**: Performed to ensure model stability
- **Confusion Matrix**: Detailed analysis of TP, TN, FP, FN
- **ROC Curves**: Comparative analysis across all models
- **Feature Importance**: Ranked features by predictive power

### Step 6: Business Optimization

- **Threshold Tuning**: Adjusted classification threshold from 0.5 to 0.1
- **Cost-Benefit Analysis**: Incorporated business costs into decision-making
- **Risk Tolerance**: Balanced false positives vs. false negatives
- **Production Readiness**: Selected optimal model and threshold for deployment

## 💡 Results & Insights

### Transaction Channel Analysis

- **Highest Risk Channels**: Online and mobile transactions show elevated fraud rates
- **Safest Channel**: In-branch transactions have lowest fraud incidence
- **Recommendation**: Enhanced verification for digital channels

### Geographic Patterns

- **High-Risk Countries**: Identified specific countries with fraud rates above 15%
- **Distance Anomalies**: Transactions >100km from previous location are suspicious
- **Impossible Travel**: Velocity checks reveal geographically impossible sequences

### Temporal Insights

- **Peak Fraud Hours**: Night-time (23:00 - 05:00) shows 2.5x higher fraud rate
- **Weekend Effect**: Marginally higher fraud on weekends
- **Holiday Patterns**: Increased fraud during holiday seasons

### User Behavior

- **New Devices**: 47.8% higher fraud rate when device changes
- **Spending Anomalies**: Transactions >3 standard deviations from user average
- **Velocity Alerts**: >3 transactions in 10 minutes triggers high-risk flag

## 📈 Recommendations

### For Deployment

1. **Deploy Gradient Boosting Model** with 0.1 threshold for optimal business value
2. **Real-Time Scoring**: Implement sub-second prediction API for transaction screening
3. **Feedback Loop**: Capture confirmed fraud/legitimate labels to retrain model monthly
4. **Hybrid Approach**: Combine ML predictions with rule-based checks for high-risk transactions

### For Risk Management

1. **Enhanced Monitoring** for transactions with:
   - Night-time hours (23:00 - 05:00)
   - Device changes
   - High velocity (>3 in 10 minutes)
   - Unusual geographic patterns

2. **Stepped Verification**:
   - Low risk (prob < 0.1): Approve automatically
   - Medium risk (0.1 - 0.5): SMS verification
   - High risk (> 0.5): Block and investigate

3. **Customer Experience**:
   - Alert customers to suspicious patterns proactively
   - Provide easy fraud reporting mechanisms
   - Minimize false positive friction for legitimate users

### For Future Improvements

1. **Data Collection**:
   - Capture more behavioral features (typing patterns, mouse movements)
   - Enrich with external data (IP reputation, device fingerprinting)
   - Collect merchant category and reputation data

2. **Model Enhancement**:
   - Experiment with deep learning (LSTM for sequence modeling)
   - Implement ensemble methods with diverse algorithms
   - Use AutoML for hyperparameter optimization

3. **Monitoring & Maintenance**:
   - Track model drift monthly
   - A/B test new models before full deployment
   - Maintain model performance dashboard

## 📝 Contributing

This project was developed as a case study for FNB fraud detection. For questions, improvements, or collaboration opportunities:

1. **Issues**: Open an issue for bugs or feature requests
2. **Pull Requests**: Submit PRs for improvements
3. **Contact**: Reach out to the development team

## 📄 License

This project is available under the MIT License. See LICENSE file for details.

## 🙏 Acknowledgments

- **FNB (First National Bank)** for the business case and domain expertise
- **Scikit-learn & XGBoost** communities for excellent ML libraries
- **Data Science Community** for fraud detection research and best practices

---

**Last Updated**: January 27, 2026  
**Version**: 1.0  
**Status**: Production Ready ✅

For detailed technical implementation, refer to the Jupyter Notebook.  
For Google Colab setup, see README.txt.
