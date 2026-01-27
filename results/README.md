# Analysis Results Directory

This directory contains all analysis results, metrics, and performance comparisons from the fraud detection study.

## 📊 Files Overview

### Excel Master File
- **`FNB_Fraud_Detection_Results_20260127_165934.xlsx`**
  - Comprehensive Excel workbook with all results
  - Multiple sheets for different analyses
  - Ready for presentation and reporting

### CSV Result Files

| File | Description | Use Case |
|------|-------------|----------|
| `model_comparison.csv` | Performance metrics for all 6 models | Model selection, reporting |
| `threshold_analysis.csv` | Threshold optimization results | Business cost analysis |
| `performance_by_class.csv` | Class-specific metrics (fraud vs. legitimate) | Detailed performance review |
| `dataset_summary.csv` | Dataset statistics and feature summaries | Data understanding |
| `business_impact_analysis.csv` | Cost-benefit calculations | Business case justification |

## 📈 Key Results Summary

### Best Model Performance
- **Model**: Gradient Boosting
- **F1-Score**: 0.6533
- **Precision**: 0.9800 (98%)
- **Recall**: 0.4900 (49%)
- **ROC-AUC**: 0.9468

### Business Impact
- **Optimal Threshold**: 0.10
- **Expected Savings**: R1,930 per 100 transactions
- **False Positive Rate**: 2% when flagged as fraud
- **Fraud Detection Rate**: 49% of all fraud cases caught

## 🔍 Using the Results

### Loading CSV Files

```python
import pandas as pd

# Load model comparison
model_comp = pd.read_csv('results/model_comparison.csv')
print(model_comp)

# Load threshold analysis
threshold_analysis = pd.read_csv('results/threshold_analysis.csv')

# Find optimal threshold
optimal = threshold_analysis.loc[threshold_analysis['total_cost'].idxmin()]
print(f"Optimal threshold: {optimal['threshold']}")
```

### Reading Excel File

```python
import pandas as pd

# Load Excel file with all sheets
excel_file = pd.ExcelFile('results/FNB_Fraud_Detection_Results_20260127_165934.xlsx')

# List all sheets
print("Available sheets:", excel_file.sheet_names)

# Read specific sheet
model_results = pd.read_excel(excel_file, sheet_name='Model_Comparison')
```

## 📊 Data Dictionary

### model_comparison.csv

| Column | Description | Range |
|--------|-------------|-------|
| Model | Model name | String |
| Accuracy | Overall accuracy | 0.0 - 1.0 |
| Precision | Positive predictive value | 0.0 - 1.0 |
| Recall | Sensitivity/True Positive Rate | 0.0 - 1.0 |
| F1-Score | Harmonic mean of precision/recall | 0.0 - 1.0 |
| ROC-AUC | Area under ROC curve | 0.0 - 1.0 |

### threshold_analysis.csv

| Column | Description |
|--------|-------------|
| threshold | Classification threshold tested |
| precision | Precision at this threshold |
| recall | Recall at this threshold |
| f1_score | F1-score at this threshold |
| true_positives | Number of correctly identified frauds |
| false_positives | Number of false alarms |
| total_cost | Total business cost (lower is better) |

### performance_by_class.csv

| Column | Description |
|--------|-------------|
| class | 0 (Legitimate) or 1 (Fraud) |
| precision | Class-specific precision |
| recall | Class-specific recall |
| f1_score | Class-specific F1-score |
| support | Number of samples in class |

## 📉 Interpreting Results

### Model Comparison

- **Accuracy**: Overall correctness (can be misleading with imbalanced data)
- **Precision**: Of all flagged frauds, how many are truly fraud
- **Recall**: Of all actual frauds, how many did we catch
- **F1-Score**: Balance between precision and recall
- **ROC-AUC**: Overall model discrimination ability

### Threshold Selection

- **Lower threshold (0.1)**: Catches more fraud but more false alarms
- **Higher threshold (0.5)**: Fewer false alarms but misses more fraud
- **Optimal (0.1)**: Best business value based on cost analysis

### Business Impact

```
Cost Model:
- Missed fraud: R100 loss
- False alarm: R10 investigation cost  
- Caught fraud: R90 benefit

At threshold 0.1:
- 67 frauds caught × R90 = R6,030 benefit
- 80 false alarms × R10 = R800 cost
- 33 frauds missed × R100 = R3,300 loss
- Net value = R6,030 - R800 - R3,300 = R1,930
```

## 📁 File Formats

All CSV files use:
- **Encoding**: UTF-8
- **Delimiter**: Comma (,)
- **Line Ending**: Unix (LF)
- **Decimal**: Period (.)

## 🔄 Regenerating Results

To regenerate these results:

1. Run the complete Jupyter Notebook
2. Execute cells in Step 7: Export Results
3. New files will be created with updated timestamps
4. Previous results will be preserved

## 📊 Visualization

These results are complemented by visualizations in the `../visualizations/` directory:

- Summary dashboards showing all key metrics
- ROC curves for model comparison
- Confusion matrices for detailed analysis
- Feature importance charts

## 📝 Notes

- **Date Format**: Results generated on 2026-01-27 16:59:34
- **Version**: These results correspond to model version 1.0
- **Reproducibility**: All results use random_state=42
- **Updates**: Results should be regenerated monthly with new data

---

**Last Updated**: January 27, 2026  
**Results Version**: 1.0  
**Generated By**: FNB Fraud Detection Analysis Pipeline
