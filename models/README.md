# Trained Models Directory

This directory contains all trained machine learning models for fraud detection.

## 📦 Available Models

### Primary Models

| Model File | Algorithm | Description | Performance (F1) |
|------------|-----------|-------------|------------------|
| `final_model.pkl` | Gradient Boosting | **Best performing model** - Selected for production | 0.6533 |
| `fraud_detection_pipeline.pkl` | Complete Pipeline | Full preprocessing + model pipeline | - |

### Individual Classifiers

| Model File | F1-Score | Precision | Recall | ROC-AUC |
|------------|----------|-----------|--------|---------|
| `gradient_boosting.pkl` | 0.6533 | 0.9800 | 0.49 | 0.9468 |
| `ensemble_voting.pkl` | 0.6231 | 0.6263 | 0.62 | 0.9349 |
| `xgboost.pkl` | 0.6291 | 0.5929 | 0.67 | 0.9403 |
| `random_forest.pkl` | 0.4842 | 0.5111 | 0.46 | 0.9243 |
| `logistic_regression.pkl` | 0.3060 | 0.1951 | 0.71 | 0.8363 |
| `decision_tree.pkl` | 0.2616 | 0.1637 | 0.65 | 0.8342 |

## 🚀 Usage

### Loading a Model

```python
import joblib

# Load the best model
model = joblib.load('models/final_model.pkl')

# Or load the complete pipeline
pipeline = joblib.load('models/fraud_detection_pipeline.pkl')
```

### Making Predictions

```python
import pandas as pd

# Prepare your data
new_data = pd.DataFrame([{
    'amount': 500.0,
    'hour': 23,
    'device_change': 1,
    # ... other features
}])

# Predict
prediction = model.predict(new_data)
probability = model.predict_proba(new_data)[:, 1]

print(f"Prediction: {prediction[0]}")  # 0 = Legitimate, 1 = Fraud
print(f"Fraud Probability: {probability[0]:.2%}")
```

## 📊 Model Selection Criteria

The **Gradient Boosting** model was selected as the final model based on:

1. **Highest Precision (98%)**: Minimizes false positives
2. **Best F1-Score (0.65)**: Optimal balance
3. **Superior ROC-AUC (0.95)**: Excellent discrimination
4. **Business Value**: 98 out of 100 flagged transactions are truly fraudulent

## 🔧 Model Specifications

### Training Configuration

- **Training Samples**: 7,000 (after 70/30 split)
- **Class Balancing**: SMOTE applied
- **Features**: 23 selected features
- **Cross-Validation**: 5-fold stratified CV
- **Random State**: 42 (for reproducibility)

### Optimal Threshold

- **Default**: 0.5
- **Optimized**: 0.1 (based on business cost analysis)
- **Use Case**: Production deployment should use 0.1 threshold

## 📝 Model Versioning

- **Version**: 1.0
- **Training Date**: 2026-01-27
- **Python Version**: 3.8+
- **Scikit-learn**: 1.0+
- **XGBoost**: 1.5+

## ⚠️ Important Notes

1. **Feature Requirements**: Models expect exactly 23 features in the correct order
2. **Preprocessing**: Use `fraud_detection_pipeline.pkl` for automatic preprocessing
3. **Scaling**: Some models require StandardScaler (included in pipeline)
4. **Dependencies**: Ensure compatible library versions

## 🔄 Retraining

To retrain models with new data:

1. Follow the Jupyter Notebook steps 1-5
2. Use the same feature engineering process
3. Apply SMOTE for class balancing
4. Save new models with version numbers
5. Compare performance with existing models

## 📈 Performance Monitoring

Monitor these metrics in production:

- **Precision**: Should stay above 0.90
- **Recall**: Target at least 0.45
- **F1-Score**: Maintain above 0.60
- **ROC-AUC**: Keep above 0.93

If metrics degrade, consider retraining with recent data.

---

**Last Updated**: January 27, 2026  
**Model Version**: 1.0
