# 🎮 Video Game Hit Predictor

## 🎯 Project Goal

The primary objective of this project was to develop a highly predictive **classification model** capable of forecasting whether a video game will be a **global "Hit"** (top 10% of sales) or a **"Flop"** (bottom 90% of sales) based on pre-release and early-stage features.

Given the extreme **class imbalance** in the dataset ($\sim 90\%$ Flop, $\sim 10\%$ Hit), the model's success was measured by its ability to accurately identify the rare "Hit" class.

***

## ⚙️ Environment and Installation

The code was developed and tested within an isolated Python environment to ensure dependency control and reliable execution.

| Component | Version |
| :--- | :--- |
| **Python** | 3.10.0 |
| **Key Libraries** | `pandas`, `numpy`, `scikit-learn`, `xgboost`, `matplotlib`, `seaborn` |

### Setup Instructions

1.  **Create Virtual Environment:**
    ```bash
    python -m venv .venv
    ```
2.  **Activate Environment (Windows PowerShell):**
    ```bash
    .\.venv\Scripts\activate
    ```
3.  **Install Dependencies:**
    ```bash
    pip install scikit-learn pandas numpy matplotlib seaborn xgboost ipykernel tabulate
    ```
4.  **Launch Notebook:** Use your IDE (e.g., VS Code) to select the newly configured **Python 3.10.0 (.venv)** kernel.

***

## 🛠️ Methodology and Approach

### 1. Data Preparation & Leakage Prevention
* Features were processed using **One-Hot Encoding** (Categoricals) and **Standard Scaling** (Continuous).
* **CRITICAL:** The feature used to define the target variable, `Global_Sales`, was **dropped** before training and evaluation to ensure no data leakage.
* Data was split into $70\%$ for training and $30\%$ for final evaluation.

### 2. Imbalance Handling
To combat the $90/10$ class imbalance, a **class weighting strategy** was implemented across all models to ensure the learning algorithms penalized misclassification of the minority "Hit" class more heavily.
* **XGBoost:** Used the calculated `scale_pos_weight` ratio (Flops/Hits).
* **Logistic Regression & Random Forest:** Used the built-in `class_weight='balanced'` parameter.

### 3. Models Evaluated
1.  **Dummy Classifier (Baseline)**
2.  **Logistic Regression**
3.  **Random Forest Classifier**
4.  **XGBoost Classifier** (Selected Best Performer)

***

## 📈 Evaluation Results

Model performance was primarily evaluated using the **F1-Score** for the "Hit" class (1), which provides the best single measure of balance between **Precision** and **Recall**.

### Model Comparison Table
| Model | **F1\_Score (Hit)** | ROC\_AUC | Recall (Hit) | Precision (Hit) | Accuracy |
| :--- | ---: | ---: | ---: | ---: | ---: |
| **XGBoost** | **0.367** | **0.793** | 0.560 | 0.273 | 0.808 |
| Random Forest | 0.318 | 0.770 | **0.719** | 0.204 | 0.692 |
| Logistic Regression | 0.266 | 0.705 | 0.711 | 0.164 | 0.609 |
| Dummy Classifier | 0.000 | 0.500 | 0.000 | 0.000 | 0.900 |

### Performance Trade-Off (XGBoost)
The **Confusion Matrix** demonstrates the core trade-off of our strategy:
* We successfully achieved **$56\%$ Recall (True Positives)**, meaning we capture over half of all actual successful games.
* This success came at the cost of higher **False Positives** (false alarms), which is accepted as missing a hit (False Negative) is the greater business risk.



***

## 🧠 Model Interpretation: Feature Drivers

The **Feature Importance analysis** confirmed what factors are most predictive, highlighting the niche nature of success in the dataset.

### Top Features
The model relied most heavily on **`Platform_SAT`** and **`Platform_PSP`**, followed by **`Genre_Strategy`** and **`Genre_Adventure`**.

### Key Findings
* **Niche Predictability:** The model prioritizes these older platforms and niche genres because they represent highly **reliable, low-noise signals**. Success in these segments is highly predictive of reaching the "Hit" sales threshold.
* **Context Over Scores:** The model proves that **market context (Platform and Genre)** is a stronger predictor of a Hit than continuous scores alone, validating the importance of strategic positioning.



***

## ✅ Conclusion and Assessment Summary

### Conclusion
* We successfully built a robust historical classification model (**XGBoost**) tailored to overcome data imbalance challenges, delivering strong discriminative performance (**F1: 0.367, ROC-AUC: 0.793**).
* The methodology validated the importance of **class weighting** and the use of **F1-Score** for assessing performance on imbalanced business objectives.

### Final Word
The model is a robust **historical success predictor**. Its utility for forecasting modern, unreleased titles is fundamentally constrained by the dataset's age (data ends in 2016) and lack of crucial external modern features (e.g., social media data, current console generation information).