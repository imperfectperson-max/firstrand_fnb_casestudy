# Visualizations Directory

This directory contains all visual outputs from the fraud detection analysis, including charts, graphs, and dashboards.

## 🎨 Available Visualizations

### Summary Dashboard

| File | Format | Description | Use Case |
|------|--------|-------------|----------|
| `summary_dashboard_20260127_165934.png` | PNG | High-resolution dashboard image | Presentations, reports, web |
| `summary_dashboard_20260127_165934.svg` | SVG | Vector graphics dashboard | Print materials, scaling |

## 📊 Dashboard Contents

The summary dashboard includes comprehensive visualizations covering:

### 1. Model Performance Comparison
- Bar charts comparing all 6 models
- Metrics: Accuracy, Precision, Recall, F1-Score, ROC-AUC
- Visual identification of best-performing model

### 2. ROC Curves
- Individual ROC curves for each model
- AUC values displayed
- Comparison against random classifier baseline

### 3. Confusion Matrix
- Detailed breakdown for the final model
- True Positives, True Negatives, False Positives, False Negatives
- Visual heat map representation

### 4. Feature Importance
- Top 15 most important features
- Horizontal bar chart showing relative importance
- Feature categories color-coded

### 5. Threshold Analysis
- Precision-Recall curve
- Optimal threshold marker
- Business cost visualization

### 6. Class Distribution
- Fraud vs. Legitimate transaction counts
- Percentage breakdown
- Visual representation of class imbalance

## 🖼️ Viewing Visualizations

### PNG Files
```bash
# Linux/Mac
open summary_dashboard_20260127_165934.png

# Windows
start summary_dashboard_20260127_165934.png

# Or use any image viewer
```

### SVG Files
```bash
# Open in web browser
firefox summary_dashboard_20260127_165934.svg

# Or use Inkscape, Adobe Illustrator for editing
```

### In Python
```python
from PIL import Image
import matplotlib.pyplot as plt

# Load and display PNG
img = Image.open('visualizations/summary_dashboard_20260127_165934.png')
plt.figure(figsize=(20, 12))
plt.imshow(img)
plt.axis('off')
plt.show()
```

## 📐 Image Specifications

### PNG Dashboard
- **Resolution**: 1920 x 1080 pixels (Full HD)
- **DPI**: 300 (print quality)
- **Color Space**: RGB
- **File Size**: ~2-3 MB
- **Best For**: Digital presentations, web, reports

### SVG Dashboard
- **Format**: Scalable Vector Graphics
- **Scalability**: Infinite without quality loss
- **File Size**: ~1-2 MB
- **Best For**: Print materials, posters, high-quality publications

## 🎯 Visualization Insights

### Key Takeaways from Dashboard

1. **Model Performance**
   - Gradient Boosting clearly outperforms other models
   - Ensemble methods (RF, XGB, GB) significantly better than simple models
   - ROC-AUC >0.93 for top 3 models

2. **Feature Importance**
   - Time-based features dominate (hour is #1)
   - Transaction amount is critical
   - Behavioral patterns (velocity, device) very important

3. **Confusion Matrix**
   - High True Negative rate (good)
   - Moderate True Positive rate (49%)
   - Very low False Positive rate (excellent)

4. **Threshold Impact**
   - Lowering threshold increases recall
   - Trade-off between precision and recall
   - Business costs favor lower threshold

## 📊 Creating Custom Visualizations

If you need to generate custom visualizations:

```python
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

# Set style for consistent look
plt.style.use('seaborn-v0_8-darkgrid')
sns.set_palette("husl")

# Load results
results = pd.read_csv('../results/model_comparison.csv')

# Create custom plot
plt.figure(figsize=(12, 6))
plt.bar(results['Model'], results['F1-Score'])
plt.xlabel('Model')
plt.ylabel('F1-Score')
plt.title('Model F1-Score Comparison')
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('custom_f1_comparison.png', dpi=300)
plt.show()
```

## 🎨 Color Scheme

The visualizations use a consistent color scheme:

- **Legitimate Transactions**: Green (#28a745)
- **Fraudulent Transactions**: Red (#dc3545)
- **High Performance**: Blue (#007bff)
- **Medium Performance**: Orange (#fd7e14)
- **Low Performance**: Gray (#6c757d)

## 📏 Figure Sizes

Standard figure sizes used:
- **Single Plot**: 10 x 6 inches
- **Comparison Plot**: 12 x 6 inches
- **Dashboard**: 20 x 12 inches
- **Detailed Analysis**: 15 x 10 inches

## 🔄 Regenerating Visualizations

To regenerate visualizations with updated data:

1. Run the Jupyter Notebook through Step 7
2. Visualizations are automatically generated and saved
3. New files will have updated timestamps
4. Previous versions are preserved

## 📱 Using in Presentations

### PowerPoint/Keynote
1. Insert → Pictures → Select PNG file
2. Resize as needed (maintains quality)
3. Use for slides 4-8 of presentation

### Web/HTML
```html
<img src="visualizations/summary_dashboard_20260127_165934.png" 
     alt="Fraud Detection Dashboard" 
     width="100%" />
```

### LaTeX/Academic Papers
```latex
\begin{figure}[h]
  \centering
  \includegraphics[width=\textwidth]{visualizations/summary_dashboard_20260127_165934.png}
  \caption{Comprehensive Fraud Detection Analysis Dashboard}
  \label{fig:dashboard}
\end{figure}
```

## 📝 Notes

- **Resolution**: All PNG files are created at 300 DPI for print quality
- **Fonts**: Default matplotlib fonts are used for compatibility
- **Accessibility**: Consider adding alt text when using in documents
- **Updates**: Regenerate after model retraining or data updates

## 🔍 Additional Visualizations in Notebook

The Jupyter Notebook contains many more detailed visualizations:
- Transaction amount distributions
- Channel-wise fraud rates
- Geographic heatmaps
- Temporal patterns (hourly, daily, monthly)
- Device change analysis
- Velocity distribution plots
- Correlation heatmaps
- And more...

To access these, run the complete notebook and view inline plots.

---

**Last Updated**: January 27, 2026  
**Dashboard Version**: 1.0  
**Generated By**: FNB Fraud Detection Analysis Pipeline
