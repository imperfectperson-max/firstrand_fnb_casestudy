# Reports Directory

This directory contains configuration files and exported scripts from the fraud detection analysis.

## 📄 Files Overview

| File | Type | Description |
|------|------|-------------|
| `config_20260127_165934.json` | Configuration | Analysis parameters and settings |
| `fraud_detection_script_20260127_165934.py` | Python Script | Exported Python code from notebook |

## ⚙️ Configuration File

### config_20260127_165934.json

This JSON file contains all configuration parameters used in the analysis:

#### Contents
```json
{
  "analysis_date": "2026-01-27 16:59:34",
  "dataset": {
    "total_records": 10000,
    "features": 25,
    "fraud_rate": 0.10
  },
  "preprocessing": {
    "train_test_split": 0.3,
    "random_state": 42,
    "scaling_method": "StandardScaler",
    "balancing_method": "SMOTE"
  },
  "models": {
    "trained": [
      "Logistic Regression",
      "Decision Tree", 
      "Random Forest",
      "XGBoost",
      "Gradient Boosting",
      "Ensemble Voting"
    ],
    "final_model": "Gradient Boosting",
    "optimal_threshold": 0.1
  },
  "evaluation": {
    "metrics": ["accuracy", "precision", "recall", "f1_score", "roc_auc"],
    "cv_folds": 5
  }
}
```

#### Usage

```python
import json

# Load configuration
with open('reports/config_20260127_165934.json', 'r') as f:
    config = json.load(f)

# Access parameters
print(f"Random state: {config['preprocessing']['random_state']}")
print(f"Final model: {config['models']['final_model']}")
print(f"Optimal threshold: {config['models']['optimal_threshold']}")
```

## 🐍 Python Script

### fraud_detection_script_20260127_165934.py

This is a standalone Python script exported from the Jupyter Notebook.

#### Purpose
- Reproducible analysis outside of Jupyter
- Automated pipeline execution
- Integration into production systems
- Scheduled retraining jobs

#### Structure

The script contains:
1. **Imports**: All required libraries
2. **Configuration**: Parameters and settings
3. **Data Loading**: CSV file loading and validation
4. **Preprocessing**: Data cleaning and transformation
5. **Feature Engineering**: Creating derived features
6. **Model Training**: Training all models
7. **Evaluation**: Performance metrics calculation
8. **Export**: Saving models and results

#### Running the Script

```bash
# Basic execution
python fraud_detection_script_20260127_165934.py

# With custom data file
python fraud_detection_script_20260127_165934.py --data /path/to/data.csv

# With different configuration
python fraud_detection_script_20260127_165934.py --config custom_config.json
```

#### Requirements

Ensure you have installed all dependencies:
```bash
pip install pandas numpy scikit-learn xgboost matplotlib seaborn imbalanced-learn
```

## 🔧 Configuration Parameters

### Data Splitting

| Parameter | Value | Description |
|-----------|-------|-------------|
| `test_size` | 0.3 | 30% of data for testing |
| `random_state` | 42 | Seed for reproducibility |
| `stratify` | True | Maintain class distribution |

### SMOTE Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| `sampling_strategy` | 'auto' | Balance minority class |
| `random_state` | 42 | Seed for reproducibility |
| `k_neighbors` | 5 | Neighbors for synthetic samples |

### Model Hyperparameters

#### Gradient Boosting (Final Model)
```python
{
    'n_estimators': 100,
    'learning_rate': 0.1,
    'max_depth': 5,
    'random_state': 42
}
```

#### Random Forest
```python
{
    'n_estimators': 100,
    'max_depth': 10,
    'random_state': 42
}
```

#### XGBoost
```python
{
    'n_estimators': 100,
    'learning_rate': 0.1,
    'max_depth': 5,
    'random_state': 42
}
```

## 📊 Using the Script for Production

### 1. Scheduled Retraining

```bash
# Add to crontab for monthly retraining
0 2 1 * * /usr/bin/python3 /path/to/fraud_detection_script.py --data /path/to/latest_data.csv
```

### 2. CI/CD Integration

```yaml
# Example GitHub Actions workflow
name: Retrain Model
on:
  schedule:
    - cron: '0 2 1 * *'  # Monthly

jobs:
  retrain:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Run training script
        run: python reports/fraud_detection_script.py
```

### 3. Batch Processing

```python
import subprocess
from datetime import datetime

# Run script and capture output
result = subprocess.run(
    ['python', 'fraud_detection_script_20260127_165934.py'],
    capture_output=True,
    text=True
)

# Log results
with open(f'training_log_{datetime.now():%Y%m%d}.txt', 'w') as f:
    f.write(result.stdout)
```

## 📈 Performance Tracking

### Creating a Training Log

```python
import json
from datetime import datetime

training_log = {
    'timestamp': datetime.now().isoformat(),
    'data_records': 10000,
    'model': 'Gradient Boosting',
    'metrics': {
        'accuracy': 0.9740,
        'precision': 0.9800,
        'recall': 0.4900,
        'f1_score': 0.6533,
        'roc_auc': 0.9468
    }
}

with open('training_logs.json', 'a') as f:
    f.write(json.dumps(training_log) + '\n')
```

## 🔄 Version Control

### Config File Versioning

Keep track of configuration changes:

```bash
# Tag configurations with dates
cp config.json config_20260127_165934.json

# Git track
git add reports/config_*.json
git commit -m "Add configuration for analysis 2026-01-27"
```

## 📝 Customizing Configuration

To create a custom configuration:

```python
import json

custom_config = {
    "preprocessing": {
        "train_test_split": 0.2,  # 80-20 split instead
        "random_state": 123,       # Different seed
        "scaling_method": "MinMaxScaler"  # Different scaler
    },
    "models": {
        "optimal_threshold": 0.15  # Different threshold
    }
}

with open('reports/custom_config.json', 'w') as f:
    json.dump(custom_config, f, indent=2)
```

## 🎯 Best Practices

1. **Version Configuration**: Always timestamp configuration files
2. **Document Changes**: Comment why parameters were changed
3. **Track Performance**: Log metrics for each configuration
4. **Test Thoroughly**: Validate new configurations on test data
5. **Backup Models**: Keep previous versions before retraining

## 📚 Additional Documentation

For more details on specific components:
- **Preprocessing**: See Notebook Steps 1-3
- **Model Training**: See Notebook Step 4
- **Evaluation**: See Notebook Step 5
- **Feature Engineering**: See Notebook Step 3

---

**Last Updated**: January 27, 2026  
**Script Version**: 1.0  
**Configuration Version**: 1.0
