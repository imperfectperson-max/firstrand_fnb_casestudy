# Data Directory

This directory is for storing input data files for the fraud detection analysis.

## 📁 Directory Purpose

Store your transaction data CSV files here for analysis. The notebook expects data in this format.

## 📊 Expected Data Format

### Required Columns

Your CSV file should contain these columns:

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `transaction_id` | int | Unique transaction identifier | 12345 |
| `user_id` | string | User identifier | USR_001 |
| `amount` | float | Transaction amount | 500.50 |
| `transaction_channel` | string | Channel used | online, mobile, atm, branch |
| `merchant` | string | Merchant name | Amazon, Walmart |
| `device_id` | string | Device identifier | DEV_12345 |
| `card_type` | string | Card type | credit, debit |
| `country` | string | Country code | US, UK, ZA |
| `timestamp` | datetime | Transaction timestamp | 2026-01-27 10:30:00 |
| `prev_timestamp` | datetime | Previous transaction time | 2026-01-26 15:20:00 |
| `lat` | float | Latitude | 40.7128 |
| `lon` | float | Longitude | -74.0060 |
| `prev_lat` | float | Previous latitude | 40.7580 |
| `prev_lon` | float | Previous longitude | -73.9855 |
| `time_since_last_tx` | float | Seconds since last transaction | 3600 |
| `distance_from_last_tx` | float | Distance in km | 5.2 |
| `velocity_last_10min` | float | Transactions in last 10 min | 2 |
| `km_per_min` | float | Travel speed | 0.5 |
| `device_change` | int | Device changed (0/1) | 1 |
| `high_risk_country` | int | High risk country (0/1) | 0 |
| `is_night` | int | Night time transaction (0/1) | 1 |
| `merchant_risk_score` | float | Merchant risk score | 0.65 |
| `multiple_risk_factors` | int | Multiple risks present (0/1) | 1 |
| `hour` | int | Hour of day (0-23) | 23 |
| `is_fraud` | int | **Target variable** (0=legitimate, 1=fraud) | 0 |

### Sample Data Structure

```csv
transaction_id,user_id,amount,transaction_channel,is_fraud
1,USR_001,125.50,online,0
2,USR_002,2500.00,atm,1
3,USR_001,45.00,mobile,0
```

## 📥 Adding Your Data

1. **Place CSV file** in this directory:
   ```bash
   cp /path/to/your/transactions.csv data/
   ```

2. **Update notebook** to point to your file:
   ```python
   df = pd.read_csv('data/your_transactions.csv')
   ```

3. **Verify format**:
   ```python
   print(df.columns.tolist())
   print(df.shape)
   print(df.head())
   ```

## 🔒 Data Privacy

**IMPORTANT**: This directory is included in `.gitignore` to prevent accidentally committing sensitive data.

### Best Practices

- ✅ Use synthetic or anonymized data for development
- ✅ Remove PII (Personally Identifiable Information)
- ✅ Encrypt data at rest if contains sensitive information
- ✅ Use secure file transfers (SFTP, encrypted channels)
- ❌ Never commit real customer data to version control
- ❌ Don't share raw data publicly

## 📝 Data Preparation Tips

### Cleaning Your Data

Before analysis, ensure:

1. **Correct Data Types**:
   ```python
   df['timestamp'] = pd.to_datetime(df['timestamp'])
   df['amount'] = df['amount'].astype(float)
   ```

2. **Handle Missing Values**:
   ```python
   print(df.isnull().sum())
   ```

3. **Check for Duplicates**:
   ```python
   print(df.duplicated().sum())
   ```

4. **Validate Ranges**:
   ```python
   print(df['amount'].describe())
   print(df['hour'].min(), df['hour'].max())  # Should be 0-23
   ```

## 🧪 Sample Data

For testing purposes, you can generate synthetic data:

```python
import pandas as pd
import numpy as np
from datetime import datetime, timedelta

np.random.seed(42)
n_samples = 1000

# Generate synthetic transaction data
data = {
    'transaction_id': range(1, n_samples + 1),
    'user_id': [f'USR_{i%100:03d}' for i in range(n_samples)],
    'amount': np.random.lognormal(5, 1, n_samples),
    'transaction_channel': np.random.choice(['online', 'mobile', 'atm', 'branch'], n_samples),
    'is_fraud': np.random.choice([0, 1], n_samples, p=[0.9, 0.1])
}

df = pd.DataFrame(data)
df.to_csv('data/sample_transactions.csv', index=False)
print(f"Created sample data: {df.shape}")
```

## 📊 Data Statistics

After loading your data, run these checks:

```python
import pandas as pd

df = pd.read_csv('data/your_file.csv')

print("=" * 50)
print("DATA STATISTICS")
print("=" * 50)
print(f"Total records: {len(df):,}")
print(f"Features: {len(df.columns)}")
print(f"Fraud rate: {df['is_fraud'].mean()*100:.2f}%")
print(f"Date range: {df['timestamp'].min()} to {df['timestamp'].max()}")
print(f"Amount range: ${df['amount'].min():.2f} - ${df['amount'].max():.2f}")
```

## 🗂️ File Naming Convention

Use descriptive names for your data files:

- `transactions_2026_01.csv` - Monthly data
- `fraud_analysis_train.csv` - Training dataset
- `fraud_analysis_test.csv` - Test dataset
- `historical_fraud_data.csv` - Historical data

## ⚠️ Common Issues

### Issue: File Not Found
```
FileNotFoundError: data/transactions.csv
```
**Solution**: Ensure file is in the `data/` directory and name matches exactly.

### Issue: Encoding Errors
```
UnicodeDecodeError
```
**Solution**: Specify encoding when reading:
```python
df = pd.read_csv('data/file.csv', encoding='utf-8')
```

### Issue: Date Parsing Errors
```
ValueError: Unknown datetime string format
```
**Solution**: Specify date format:
```python
df['timestamp'] = pd.to_datetime(df['timestamp'], format='%Y-%m-%d %H:%M:%S')
```

---

**Note**: This directory is git-ignored by default. Only documentation files are tracked.
