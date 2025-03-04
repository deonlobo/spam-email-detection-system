# Spam Email Detection System

This project focuses on building a machine learning classifier to detect spam emails. The dataset used for this project is the [SMSSpamCollection](https://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection), which contains labeled SMS messages as 'spam' or 'ham' (not spam).

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Model Selection](#model-selection)
- [Evaluation](#evaluation)
- [Contributing](#contributing)

## Introduction

Spam detection is an essential aspect of email security. This project aims to create a reliable spam email detection system using machine learning techniques. The project involves reading and cleaning text data, feature extraction using TF-IDF, and model selection using different classifiers.

## Installation

To get started with the project, follow these steps:

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/spam-email-detection-system.git
   ```

2. Navigate to the project directory:

   ```bash
   cd spam-email-detection-system
   ```

3. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Usage

1. Ensure you have the `SMSSpamCollection.tsv` file in the project directory.
2. Run the Jupyter notebook to execute the code and train the models.
3. The notebook includes all the steps for data preprocessing, model training, and evaluation.

## Project Structure

- `spam-email-detection-system.ipynb`: Jupyter notebook containing the code for the project.
- `SMSSpamCollection.tsv`: Dataset file.
- `requirements.txt`: List of Python packages required to run the project.

## Model Selection

The project uses two main classifiers for spam detection:

1. **Random Forest Classifier**:

   - Fast training and prediction time.
   - High accuracy and precision.

2. **Gradient Boosting Classifier**:
   - Slower training time but good accuracy and precision.
   - Suitable for more complex data.

## Evaluation

The models are evaluated based on the following metrics:

- Precision
- Recall
- Accuracy

Example evaluation results:

```plaintext
Random Forest Classifier:
Fit time: 1.782 / Predict time: 0.213 ---- Precision: 1.0 / Recall: 0.81 / Accuracy: 0.975

Gradient Boosting Classifier:
Fit time: 186.61 / Predict time: 0.135 ---- Precision: 0.889 / Recall: 0.816 / Accuracy: 0.962
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
