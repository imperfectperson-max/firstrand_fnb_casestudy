# Fraud Detection Analysis - Google Colab Guide

## Setup Instructions
1. Open Google Colab (colab.research.google.com)
2. Create a new notebook
3. Copy each cell below into separate code cells in your Colab notebook
4. Run cells in order (Shift + Enter)

---

## CELL 1: Install Required Libraries
```python
# Install necessary libraries
!pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn
```

---

## CELL 2: Import Libraries
```python
# Import all necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime
import warnings
warnings.filterwarnings('ignore')

# Set plotting style
plt.style.use('seaborn-v0_8-darkgrid')
sns.set_palette("husl")

print("✓ All libraries imported successfully!")
```

---

## CELL 3: Upload Your Dataset
```python
# Upload the CSV file
from google.colab import files
uploaded = files.upload()

# Get the filename
filename = list(uploaded.keys())[0]
print(f"File uploaded: {filename}")
```

---

## CELL 4: Load the Data
```python
# Load the dataset
df = pd.read_csv(filename)

# Display basic information
print("Dataset Shape:", df.shape)
print("\nFirst few rows:")
df.head()
```

---

## CELL 5: Initial Data Overview
```python
# Get detailed information about the dataset
print("=== DATASET INFORMATION ===\n")
df.info()

print("\n=== STATISTICAL SUMMARY ===\n")
df.describe()
```

---

## STEP 1: DATA PREPARATION

## CELL 6: Check Missing Values
```python
# Check for missing values
print("=== MISSING VALUES ===\n")
missing = df.isnull().sum()
missing_pct = (missing / len(df)) * 100

missing_df = pd.DataFrame({
    'Missing Count': missing,
    'Percentage': missing_pct
})

print(missing_df[missing_df['Missing Count'] > 0].sort_values('Missing Count', ascending=False))

# Visualize missing values
plt.figure(figsize=(12, 6))
missing_df[missing_df['Missing Count'] > 0].sort_values('Missing Count')['Percentage'].plot(kind='barh')
plt.xlabel('Percentage Missing')
plt.title('Missing Values by Column')
plt.tight_layout()
plt.show()
```

---

## CELL 7: Handle Missing Values
```python
# Fill missing values strategically
# Numeric columns - fill with median
numeric_cols = df.select_dtypes(include=['float64', 'int64']).columns
for col in numeric_cols:
    if df[col].isnull().sum() > 0:
        df[col].fillna(df[col].median(), inplace=True)

# Categorical columns - fill with mode or 'Unknown'
categorical_cols = df.select_dtypes(include=['object']).columns
for col in categorical_cols:
    if df[col].isnull().sum() > 0:
        df[col].fillna(df[col].mode()[0] if len(df[col].mode()) > 0 else 'Unknown', inplace=True)

print("✓ Missing values handled!")
print("\nRemaining missing values:", df.isnull().sum().sum())
```

---

## CELL 8: Convert Data Types
```python
# Convert timestamp columns to datetime
timestamp_cols = ['timestamp', 'prev_timestamp']

for col in timestamp_cols:
    if col in df.columns:
        df[col] = pd.to_datetime(df[col], errors='coerce')

print("✓ Data types converted!")
df.dtypes
```

---

## CELL 9: Check for Duplicates
```python
# Check for duplicate transaction IDs
duplicates = df.duplicated(subset=['transaction_id']).sum()
print(f"Duplicate transactions: {duplicates}")

if duplicates > 0:
    df = df.drop_duplicates(subset=['transaction_id'], keep='first')
    print(f"✓ Removed {duplicates} duplicates")
```

---

## STEP 2: EXPLORATORY DATA ANALYSIS

## CELL 10: Fraud Distribution
```python
# Analyze fraud distribution
fraud_counts = df['is_fraud'].value_counts()
fraud_pct = df['is_fraud'].value_counts(normalize=True) * 100

print("=== FRAUD DISTRIBUTION ===\n")
print(f"Legitimate: {fraud_counts[0]:,} ({fraud_pct[0]:.2f}%)")
print(f"Fraudulent: {fraud_counts[1]:,} ({fraud_pct[1]:.2f}%)")

# Visualize
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Count plot
fraud_counts.plot(kind='bar', ax=axes[0], color=['green', 'red'])
axes[0].set_title('Transaction Count by Fraud Status')
axes[0].set_xlabel('Is Fraud (0=No, 1=Yes)')
axes[0].set_ylabel('Count')
axes[0].set_xticklabels(['Legitimate', 'Fraudulent'], rotation=0)

# Pie chart
axes[1].pie(fraud_counts, labels=['Legitimate', 'Fraudulent'], 
            autopct='%1.1f%%', colors=['green', 'red'], startangle=90)
axes[1].set_title('Fraud Distribution')

plt.tight_layout()
plt.show()
```

---

## CELL 11: Transaction Amount Analysis
```python
# Analyze transaction amounts
fig, axes = plt.subplots(2, 2, figsize=(15, 10))

# Distribution by fraud status
df[df['is_fraud']==0]['amount'].hist(bins=50, ax=axes[0,0], color='green', alpha=0.7)
axes[0,0].set_title('Legitimate Transaction Amounts')
axes[0,0].set_xlabel('Amount')

df[df['is_fraud']==1]['amount'].hist(bins=50, ax=axes[0,1], color='red', alpha=0.7)
axes[0,1].set_title('Fraudulent Transaction Amounts')
axes[0,1].set_xlabel('Amount')

# Box plot comparison
df.boxplot(column='amount', by='is_fraud', ax=axes[1,0])
axes[1,0].set_title('Amount Distribution by Fraud Status')
axes[1,0].set_xlabel('Is Fraud')
axes[1,0].set_ylabel('Amount')

# Statistics
fraud_stats = df.groupby('is_fraud')['amount'].describe()
axes[1,1].axis('off')
axes[1,1].table(cellText=fraud_stats.values, 
                rowLabels=fraud_stats.index,
                colLabels=fraud_stats.columns,
                loc='center')
axes[1,1].set_title('Amount Statistics by Fraud Status')

plt.tight_layout()
plt.show()
```

---

## CELL 12: Transaction Channel Analysis
```python
# Analyze transaction channels
channel_fraud = pd.crosstab(df['transaction_channel'], df['is_fraud'], normalize='index') * 100

print("=== FRAUD RATE BY CHANNEL ===\n")
print(channel_fraud)

# Visualize
fig, axes = plt.subplots(1, 2, figsize=(15, 6))

# Count by channel
df['transaction_channel'].value_counts().plot(kind='bar', ax=axes[0])
axes[0].set_title('Transaction Count by Channel')
axes[0].set_xlabel('Channel')
axes[0].set_ylabel('Count')
axes[0].tick_params(axis='x', rotation=45)

# Fraud rate by channel
channel_fraud[1].plot(kind='bar', ax=axes[1], color='red')
axes[1].set_title('Fraud Rate by Channel (%)')
axes[1].set_xlabel('Channel')
axes[1].set_ylabel('Fraud Rate (%)')
axes[1].tick_params(axis='x', rotation=45)

plt.tight_layout()
plt.show()
```

---

## CELL 13: Country Analysis
```python
# Top countries by transaction volume
top_countries = df['country'].value_counts().head(10)

# Fraud rate by country (top 10)
country_fraud = df.groupby('country')['is_fraud'].agg(['sum', 'count', 'mean'])
country_fraud['fraud_rate'] = country_fraud['mean'] * 100
country_fraud = country_fraud.sort_values('count', ascending=False).head(10)

fig, axes = plt.subplots(1, 2, figsize=(15, 6))

# Transaction volume
top_countries.plot(kind='barh', ax=axes[0])
axes[0].set_title('Top 10 Countries by Transaction Volume')
axes[0].set_xlabel('Number of Transactions')

# Fraud rate
country_fraud['fraud_rate'].plot(kind='barh', ax=axes[1], color='red')
axes[1].set_title('Fraud Rate by Country (Top 10 by Volume)')
axes[1].set_xlabel('Fraud Rate (%)')

plt.tight_layout()
plt.show()
```

---

## CELL 14: Time-Based Analysis
```python
# Extract time features
if 'timestamp' in df.columns:
    df['hour'] = df['timestamp'].dt.hour
    df['day_of_week'] = df['timestamp'].dt.dayofweek
    df['month'] = df['timestamp'].dt.month
    
    # Fraud by hour
    hourly_fraud = df.groupby('hour')['is_fraud'].mean() * 100
    
    fig, axes = plt.subplots(1, 2, figsize=(15, 6))
    
    # Transactions by hour
    df['hour'].hist(bins=24, ax=axes[0], edgecolor='black')
    axes[0].set_title('Transaction Volume by Hour')
    axes[0].set_xlabel('Hour of Day')
    axes[0].set_ylabel('Count')
    
    # Fraud rate by hour
    hourly_fraud.plot(kind='line', ax=axes[1], marker='o', color='red')
    axes[1].set_title('Fraud Rate by Hour')
    axes[1].set_xlabel('Hour of Day')
    axes[1].set_ylabel('Fraud Rate (%)')
    axes[1].grid(True)
    
    plt.tight_layout()
    plt.show()
```

---

## CELL 15: Device Change Analysis
```python
# Analyze device changes
if 'device_change' in df.columns:
    device_fraud = pd.crosstab(df['device_change'], df['is_fraud'], normalize='index') * 100
    
    print("=== FRAUD RATE BY DEVICE CHANGE ===\n")
    print(device_fraud)
    
    # Visualize
    fig, axes = plt.subplots(1, 2, figsize=(14, 5))
    
    # Count
    df['device_change'].value_counts().plot(kind='bar', ax=axes[0])
    axes[0].set_title('Transactions by Device Change Status')
    axes[0].set_xticklabels(['No Change', 'Device Changed'], rotation=0)
    
    # Fraud rate
    device_fraud[1].plot(kind='bar', ax=axes[1], color='red')
    axes[1].set_title('Fraud Rate by Device Change')
    axes[1].set_ylabel('Fraud Rate (%)')
    axes[1].set_xticklabels(['No Change', 'Device Changed'], rotation=0)
    
    plt.tight_layout()
    plt.show()
```

---

## CELL 16: Distance and Velocity Analysis
```python
# Analyze distance and velocity patterns
if 'distance_from_last_tx' in df.columns and 'km_per_min' in df.columns:
    fig, axes = plt.subplots(2, 2, figsize=(15, 10))
    
    # Distance distribution
    df[df['is_fraud']==0]['distance_from_last_tx'].hist(bins=50, ax=axes[0,0], 
                                                         color='green', alpha=0.7)
    axes[0,0].set_title('Distance from Last TX - Legitimate')
    axes[0,0].set_xlabel('Distance (km)')
    
    df[df['is_fraud']==1]['distance_from_last_tx'].hist(bins=50, ax=axes[0,1], 
                                                         color='red', alpha=0.7)
    axes[0,1].set_title('Distance from Last TX - Fraudulent')
    axes[0,1].set_xlabel('Distance (km)')
    
    # Velocity distribution
    df[df['is_fraud']==0]['km_per_min'].hist(bins=50, ax=axes[1,0], 
                                              color='green', alpha=0.7)
    axes[1,0].set_title('Velocity - Legitimate')
    axes[1,0].set_xlabel('km/min')
    
    df[df['is_fraud']==1]['km_per_min'].hist(bins=50, ax=axes[1,1], 
                                              color='red', alpha=0.7)
    axes[1,1].set_title('Velocity - Fraudulent')
    axes[1,1].set_xlabel('km/min')
    
    plt.tight_layout()
    plt.show()
```

---

## CELL 17: Correlation Analysis
```python
# Correlation heatmap for numerical features
numeric_features = df.select_dtypes(include=['float64', 'int64']).columns
correlation = df[numeric_features].corr()

plt.figure(figsize=(14, 10))
sns.heatmap(correlation, annot=True, fmt='.2f', cmap='coolwarm', center=0)
plt.title('Feature Correlation Heatmap')
plt.tight_layout()
plt.show()

# Correlation with fraud
print("\n=== CORRELATION WITH FRAUD ===\n")
fraud_corr = df[numeric_features].corr()['is_fraud'].sort_values(ascending=False)
print(fraud_corr)
```

---

## STEP 3: FEATURE ENGINEERING

## CELL 18: Create Time-Based Features
```python
# Create additional time-based features
if 'timestamp' in df.columns and 'prev_timestamp' in df.columns:
    # Time difference in hours
    df['hours_since_last_tx'] = df['time_since_last_tx'] / 3600
    
    # Is weekend
    df['is_weekend'] = df['day_of_week'].isin([5, 6]).astype(int)
    
    # Time of day category
    df['time_category'] = pd.cut(df['hour'], 
                                  bins=[0, 6, 12, 18, 24],
                                  labels=['Night', 'Morning', 'Afternoon', 'Evening'],
                                  include_lowest=True)

print("✓ Time-based features created!")
df[['hours_since_last_tx', 'is_weekend', 'time_category']].head()
```

---

## CELL 19: Create Risk Score Features
```python
# Create composite risk features
# High-risk transaction flag
df['high_risk_transaction'] = (
    (df['amount'] > df['amount'].quantile(0.95)) |
    (df['device_change'] == 1) |
    (df['is_night'] == 1) |
    (df['high_risk_country'] == 1)
).astype(int)

# Velocity risk
if 'velocity_last_10min' in df.columns:
    df['high_velocity'] = (df['velocity_last_10min'] > 3).astype(int)

# Distance anomaly
if 'distance_from_last_tx' in df.columns:
    df['unusual_distance'] = (df['distance_from_last_tx'] > df['distance_from_last_tx'].quantile(0.90)).astype(int)

print("✓ Risk score features created!")
df[['high_risk_transaction', 'high_velocity', 'unusual_distance']].head()
```

---

## CELL 20: Create User Behavior Features
```python
# Aggregate user behavior
user_stats = df.groupby('user_id').agg({
    'amount': ['mean', 'std', 'max'],
    'transaction_id': 'count',
    'is_fraud': 'sum'
}).reset_index()

user_stats.columns = ['user_id', 'user_avg_amount', 'user_std_amount', 
                      'user_max_amount', 'user_tx_count', 'user_fraud_count']

# Merge back to main dataframe
df = df.merge(user_stats, on='user_id', how='left')

# Amount deviation from user average
df['amount_deviation'] = abs(df['amount'] - df['user_avg_amount'])

print("✓ User behavior features created!")
df[['user_avg_amount', 'user_tx_count', 'amount_deviation']].head()
```

---

## CELL 21: Encode Categorical Variables
```python
from sklearn.preprocessing import LabelEncoder

# Identify categorical columns to encode
categorical_to_encode = ['transaction_channel', 'card_type', 'country', 'merchant']

# Create label encoders
encoders = {}
for col in categorical_to_encode:
    if col in df.columns:
        le = LabelEncoder()
        df[f'{col}_encoded'] = le.fit_transform(df[col].astype(str))
        encoders[col] = le

print("✓ Categorical variables encoded!")
print("\nEncoded columns:", [f'{col}_encoded' for col in categorical_to_encode if col in df.columns])
```

---

## STEP 4: BUILD MACHINE LEARNING MODEL

## CELL 22: Prepare Features and Target
```python
from sklearn.model_selection import train_test_split

# Define feature columns (exclude IDs, timestamps, and target)
exclude_cols = ['transaction_id', 'user_id', 'device_id', 'merchant', 
                'timestamp', 'prev_timestamp', 'is_fraud', 'country', 
                'transaction_channel', 'card_type', 'time_category',
                'lat', 'lon', 'prev_lat', 'prev_lon']

# Get all numeric columns
feature_cols = [col for col in df.columns if col not in exclude_cols and df[col].dtype in ['int64', 'float64']]

print(f"Number of features: {len(feature_cols)}")
print("\nFeatures to be used:")
for i, col in enumerate(feature_cols, 1):
    print(f"{i}. {col}")

# Prepare X and y
X = df[feature_cols].fillna(0)
y = df['is_fraud']

print(f"\n✓ Feature matrix shape: {X.shape}")
print(f"✓ Target shape: {y.shape}")
```

---

## CELL 23: Split Data
```python
# Split into train and test sets
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42, stratify=y
)

print("=== DATA SPLIT ===")
print(f"Training set: {X_train.shape[0]:,} samples")
print(f"Test set: {X_test.shape[0]:,} samples")
print(f"\nFraud distribution in training set:")
print(y_train.value_counts())
print(f"\nFraud distribution in test set:")
print(y_test.value_counts())
```

---

## CELL 24: Handle Class Imbalance (Optional)
```python
from imblearn.over_sampling import SMOTE

# Apply SMOTE to balance classes
smote = SMOTE(random_state=42)
X_train_balanced, y_train_balanced = smote.fit_resample(X_train, y_train)

print("=== AFTER SMOTE ===")
print(f"Training set: {X_train_balanced.shape[0]:,} samples")
print(f"\nClass distribution:")
print(y_train_balanced.value_counts())
```

---

## CELL 25: Train Logistic Regression Model
```python
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler

# Scale features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train_balanced)
X_test_scaled = scaler.transform(X_test)

# Train Logistic Regression
lr_model = LogisticRegression(random_state=42, max_iter=1000)
lr_model.fit(X_train_scaled, y_train_balanced)

print("✓ Logistic Regression model trained!")
```

---

## CELL 26: Train Decision Tree Model
```python
from sklearn.tree import DecisionTreeClassifier

# Train Decision Tree
dt_model = DecisionTreeClassifier(random_state=42, max_depth=10, min_samples_split=20)
dt_model.fit(X_train_balanced, y_train_balanced)

print("✓ Decision Tree model trained!")
```

---

## CELL 27: Train Random Forest Model
```python
from sklearn.ensemble import RandomForestClassifier

# Train Random Forest
rf_model = RandomForestClassifier(n_estimators=100, random_state=42, max_depth=10)
rf_model.fit(X_train_balanced, y_train_balanced)

print("✓ Random Forest model trained!")
```

---

## STEP 5: MODEL EVALUATION

## CELL 28: Evaluate All Models
```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, roc_auc_score, confusion_matrix, classification_report

# Make predictions
models = {
    'Logistic Regression': (lr_model, X_test_scaled),
    'Decision Tree': (dt_model, X_test),
    'Random Forest': (rf_model, X_test)
}

results = []

for model_name, (model, X_test_data) in models.items():
    y_pred = model.predict(X_test_data)
    y_pred_proba = model.predict_proba(X_test_data)[:, 1]
    
    results.append({
        'Model': model_name,
        'Accuracy': accuracy_score(y_test, y_pred),
        'Precision': precision_score(y_test, y_pred),
        'Recall': recall_score(y_test, y_pred),
        'F1-Score': f1_score(y_test, y_pred),
        'ROC-AUC': roc_auc_score(y_test, y_pred_proba)
    })

results_df = pd.DataFrame(results)
print("=== MODEL PERFORMANCE COMPARISON ===\n")
print(results_df.to_string(index=False))
```

---

## CELL 29: Visualize Model Performance
```python
# Plot model comparison
metrics = ['Accuracy', 'Precision', 'Recall', 'F1-Score', 'ROC-AUC']

fig, ax = plt.subplots(figsize=(12, 6))
x = np.arange(len(metrics))
width = 0.25

for i, model_name in enumerate(results_df['Model']):
    values = results_df[results_df['Model'] == model_name][metrics].values[0]
    ax.bar(x + i*width, values, width, label=model_name)

ax.set_xlabel('Metrics')
ax.set_ylabel('Score')
ax.set_title('Model Performance Comparison')
ax.set_xticks(x + width)
ax.set_xticklabels(metrics)
ax.legend()
ax.grid(axis='y', alpha=0.3)

plt.tight_layout()
plt.show()
```

---

## CELL 30: Confusion Matrix for Best Model
```python
from sklearn.metrics import ConfusionMatrixDisplay

# Use Random Forest (typically best performer)
y_pred_rf = rf_model.predict(X_test)

# Plot confusion matrix
fig, ax = plt.subplots(figsize=(8, 6))
cm = confusion_matrix(y_test, y_pred_rf)
disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=['Legitimate', 'Fraud'])
disp.plot(ax=ax, cmap='Blues')
plt.title('Confusion Matrix - Random Forest')
plt.tight_layout()
plt.show()

# Print classification report
print("\n=== CLASSIFICATION REPORT - RANDOM FOREST ===\n")
print(classification_report(y_test, y_pred_rf, target_names=['Legitimate', 'Fraud']))
```

---

## CELL 31: Feature Importance
```python
# Get feature importance from Random Forest
feature_importance = pd.DataFrame({
    'Feature': feature_cols,
    'Importance': rf_model.feature_importances_
}).sort_values('Importance', ascending=False).head(15)

print("=== TOP 15 MOST IMPORTANT FEATURES ===\n")
print(feature_importance.to_string(index=False))

# Visualize
plt.figure(figsize=(10, 8))
plt.barh(range(len(feature_importance)), feature_importance['Importance'])
plt.yticks(range(len(feature_importance)), feature_importance['Feature'])
plt.xlabel('Importance')
plt.title('Top 15 Most Important Features')
plt.gca().invert_yaxis()
plt.tight_layout()
plt.show()
```

---

## CELL 32: ROC Curve
```python
from sklearn.metrics import roc_curve

# Calculate ROC curves for all models
plt.figure(figsize=(10, 8))

for model_name, (model, X_test_data) in models.items():
    y_pred_proba = model.predict_proba(X_test_data)[:, 1]
    fpr, tpr, _ = roc_curve(y_test, y_pred_proba)
    auc = roc_auc_score(y_test, y_pred_proba)
    plt.plot(fpr, tpr, label=f'{model_name} (AUC = {auc:.3f})')

plt.plot([0, 1], [0, 1], 'k--', label='Random Classifier')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curves - Model Comparison')
plt.legend()
plt.grid(alpha=0.3)
plt.tight_layout()
plt.show()
```

---

## CELL 33: Save the Best Model
```python
import pickle

# Save the Random Forest model and scaler
with open('fraud_detection_model.pkl', 'wb') as f:
    pickle.dump(rf_model, f)

with open('feature_scaler.pkl', 'wb') as f:
    pickle.dump(scaler, f)

# Save feature names
with open('feature_names.pkl', 'wb') as f:
    pickle.dump(feature_cols, f)

print("✓ Model saved successfully!")

# Download files (optional)
# from google.colab import files
# files.download('fraud_detection_model.pkl')
```

---

## CELL 34: Generate Summary Insights
```python
# Create summary insights for presentation
print("=" * 60)
print("FRAUD DETECTION ANALYSIS - KEY INSIGHTS")
print("=" * 60)

print("\n1. DATASET OVERVIEW")
print(f"   • Total transactions: {len(df):,}")
print(f"   • Fraudulent transactions: {df['is_fraud'].sum():,} ({df['is_fraud'].mean()*100:.2f}%)")
print(f"   • Legitimate transactions: {(df['is_fraud']==0).sum():,} ({(1-df['is_fraud'].mean())*100:.2f}%)")

print("\n2. HIGH-RISK INDICATORS")
high_risk_channel = channel_fraud[1].idxmax()
print(f"   • Highest risk channel: {high_risk_channel} ({channel_fraud[1].max():.2f}% fraud rate)")

if 'device_change' in df.columns:
    device_change_fraud_rate = df[df['device_change']==1]['is_fraud'].mean() * 100
    print(f"   • Device change fraud rate: {device_change_fraud_rate:.2f}%")

if 'is_night' in df.columns:
    night_fraud_rate = df[df['is_night']==1]['is_fraud'].mean() * 100
    print(f"   • Night-time fraud rate: {night_fraud_rate:.2f}%")

print("\n3. MODEL PERFORMANCE")
best_model = results_df.loc[results_df['F1-Score'].idxmax()]
print(f"   • Best performing model: {best_model['Model']}")
print(f"   • Accuracy: {best_model['Accuracy']:.3f}")
print(f"   • Precision: {best_model['Precision']:.3f}")
print(f"   • Recall: {best_model['Recall']:.3f}")
print(f"   • F1-Score: {best_model['F1-Score']:.3f}")
print(f"   • ROC-AUC: {best_model['ROC-AUC']:.3f}")

print("\n4. TOP PREDICTIVE FEATURES")
for i, row in feature_importance.head(5).iterrows():
    print(f"   • {row['Feature']}: {row['Importance']:.4f}")

print("\n5. RECOMMENDATIONS")
print("   • Focus monitoring on high-risk channels and countries")
print("   • Flag transactions with device changes for additional verification")
print("   • Implement real-time velocity checks")
print("   • Review night-time transactions more carefully")
print("   • Set amount thresholds based on user behavior patterns")

print("\n" + "=" * 60)
```

---

## BONUS: Create a Prediction Function
```python
def predict_fraud(transaction_features):
    """
    Predict if a transaction is fraudulent
    
    Parameters:
    transaction_features: dict or DataFrame with transaction details
    
    Returns:
    prediction: 0 (legitimate) or 1 (fraud)
    probability: fraud probability
    """
    # Convert to DataFrame if dict
    if isinstance(transaction_features, dict):
        transaction_features = pd.DataFrame([transaction_features])
    
    # Ensure all required features are present
    for col in feature_cols:
        if col not in transaction_features.columns:
            transaction_features[col] = 0
    
    # Select and order features
    X_new = transaction_features[feature_cols]
    
    # Make prediction
    prediction = rf_model.predict(X_new)[0]
    probability = rf_model.predict_proba(X_new)[0][1]
    
    return prediction, probability

# Test the function
print("✓ Prediction function created!")
print("\nExample usage:")
print("prediction, prob = predict_fraud(your_transaction_data)")
```

---

## END OF NOTEBOOK

# Next Steps for Your Presentation (8-10 slides):

## Suggested Slide Structure:

1. **Title Slide**
   - Project title, your names, date

2. **Problem Statement**
   - Why fraud detection matters
   - Business impact

3. **Dataset Overview**
   - Size, features, fraud rate
   - Data quality issues found

4. **Exploratory Analysis - Part 1**
   - Fraud distribution
   - Amount patterns

5. **Exploratory Analysis - Part 2**
   - Channel analysis
   - Geographic patterns

6. **Feature Engineering**
   - New features created
   - Risk indicators

7. **Model Performance**
   - Comparison table
   - Best model results

8. **Key Insights & Recommendations**
   - Top risk factors
   - Actionable recommendations

9. **Feature Importance**
   - What matters most

10. **Conclusion & Next Steps**
    - Summary
    - Future improvements

---

## Voice-Over Notes Tips:

- **Slide 1-2**: Introduce team members (1 person)
- **Slide 3-4**: Data findings (1-2 people)
- **Slide 5-6**: Analysis insights (1-2 people)
- **Slide 7-8**: Model results (1 person)
- **Slide 9-10**: Conclusions and recommendations (1 person)

Make sure each team member contributes at least one voice-over!
```
