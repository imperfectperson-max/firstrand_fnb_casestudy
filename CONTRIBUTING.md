# Contributing to FNB Fraud Detection Analysis

Thank you for your interest in contributing to this fraud detection project! This document provides guidelines for contributing.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Coding Standards](#coding-standards)
- [Pull Request Process](#pull-request-process)

## 🤝 Code of Conduct

This project adheres to professional standards of conduct. Please:
- Be respectful and inclusive
- Focus on constructive feedback
- Maintain confidentiality of sensitive data
- Follow data privacy and security best practices

## 🎯 How Can I Contribute?

### Reporting Bugs

If you find a bug, please open an issue with:
- Clear description of the problem
- Steps to reproduce
- Expected vs. actual behavior
- Python version and library versions
- Sample data (if applicable and non-sensitive)

### Suggesting Enhancements

Enhancement suggestions are welcome! Include:
- Clear description of the feature
- Use case and business value
- Potential implementation approach
- Any relevant research or examples

### Code Contributions

We welcome contributions in:
- **Model improvements**: New algorithms, hyperparameter tuning
- **Feature engineering**: Novel features, transformations
- **Performance optimization**: Speed, memory efficiency
- **Documentation**: Clarifications, examples, tutorials
- **Testing**: Unit tests, integration tests
- **Visualization**: New charts, dashboards

## 🛠️ Development Setup

### 1. Fork and Clone

```bash
git clone https://github.com/YOUR-USERNAME/firstrand_fnb_casestudy.git
cd firstrand_fnb_casestudy
```

### 2. Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
pip install -r requirements-dev.txt  # If available
```

### 4. Create Feature Branch

```bash
git checkout -b feature/your-feature-name
```

## 📝 Coding Standards

### Python Style

Follow PEP 8 guidelines:
- 4 spaces for indentation
- Max line length: 100 characters
- Descriptive variable names
- Docstrings for functions and classes

### Example

```python
def calculate_fraud_probability(features, model, threshold=0.1):
    """
    Calculate fraud probability for a transaction.
    
    Args:
        features (pd.DataFrame): Transaction features
        model: Trained ML model
        threshold (float): Classification threshold
        
    Returns:
        tuple: (prediction, probability)
    """
    probability = model.predict_proba(features)[:, 1]
    prediction = (probability >= threshold).astype(int)
    return prediction, probability
```

### Documentation

- Add docstrings to all functions
- Update README.md for significant changes
- Include inline comments for complex logic
- Create examples for new features

### Testing

- Write unit tests for new functions
- Ensure existing tests pass
- Test with various data scenarios
- Validate edge cases

## 🔄 Pull Request Process

### Before Submitting

1. **Update Documentation**: Ensure README and docstrings are current
2. **Run Tests**: All tests should pass
3. **Check Style**: Follow PEP 8 guidelines
4. **Update Changelog**: Document your changes

### Submitting PR

1. **Descriptive Title**: Clear, concise summary
2. **Detailed Description**:
   - What changes were made
   - Why they were needed
   - How to test them
3. **Link Issues**: Reference related issues
4. **Screenshots**: Include if UI/visualization changes

### PR Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Performance improvement
- [ ] Documentation update

## Testing
How has this been tested?

## Checklist
- [ ] Code follows style guidelines
- [ ] Documentation updated
- [ ] Tests added/updated
- [ ] All tests pass
```

## 🧪 Testing Guidelines

### Unit Tests

```python
import unittest
import pandas as pd
from fraud_detection import calculate_fraud_probability

class TestFraudDetection(unittest.TestCase):
    def test_probability_calculation(self):
        # Test implementation
        pass
```

### Integration Tests

Test complete workflows:
- Data loading → Preprocessing → Prediction
- Model training → Evaluation → Export

## 📊 Model Contribution Guidelines

### Adding New Models

1. **Benchmark**: Compare against existing models
2. **Document**: Explain approach and hyperparameters
3. **Justify**: Show improvement in key metrics
4. **Code**: Follow existing model structure

### Feature Engineering

1. **Rationale**: Explain why feature is useful
2. **Implementation**: Clear, efficient code
3. **Validation**: Show correlation with fraud
4. **Documentation**: Update feature list

## 🔒 Security Considerations

- **Never commit**: API keys, passwords, sensitive data
- **Sanitize**: Remove PII from examples
- **Review**: Check for SQL injection, XSS vulnerabilities
- **Data**: Use synthetic data for tests

## 📞 Questions?

- Open an issue for discussion
- Tag as "question"
- Provide context and specific questions

## 🙏 Recognition

Contributors will be acknowledged in:
- README.md contributors section
- Release notes
- Project documentation

Thank you for contributing to fraud detection research! 🚀
