## **Report: Dataset and Underlying Assumptions (Part 2\)**

### **2\. Dataset and Underlying Assumptions**

This section details the definition of the dataset, the establishment of the core business assumptions that guided the modeling approach, and the final steps of data preparation, including the mitigation of data leakage.

### **2.1 Underlying Assumptions**

The modeling process relies on two fundamental business assumptions:

1. **Defining a "Hit":** A commercial **"Hit"** is defined as a game whose Global\_Sales fall within the **top 10th percentile** of the entire distribution.

2. **Proxy for Early Success:** Since true pre-release data (budget, marketing) is unavailable, the model assumes that early commercial indicators—like the sales figures of the game's **platform** or **publisher**—serve as the best available proxy for the quality, marketing, and potential of an unreleased title.

**2.2 Data Preparation and Leakage Prevention**

The dataset was first cleaned and then pre-processed to ensure features were suitable for machine learning algorithms.

#### **Data Preprocessing Steps**

* **Encoding:** All categorical features (e.g., Genre, Platform) were converted into a numerical format using **One-Hot Encoding**.

* **Scaling:** Continuous numerical features (e.g., Critic\_Score) were normalized using **Standard Scaling**.

* **Target Definition:** A new binary column, $\\mathbf{y}$ (Is\_hit), was created to serve as the target variable for classification.

#### **Preventing Data Leakage**

The most critical step in this phase was preventing **data leakage**, where information from the future (or the target variable itself) accidentally influences the features, leading to inflated and unrealistic performance scores.

* **Mitigation:** The column **Global\_Sales** was dropped from the feature matrix ($\\mathbf{X}$). This removal is mandatory because Global\_Sales was used directly to define the Is\_hit target variable, and its inclusion would have resulted in the model predicting the answer from the answer key.

* **Final Feature Set:** The resulting feature matrix, $\\mathbf{X}$, contained the metadata (Genre, Platform, Year, etc.) that acts as the "clues" for the model.

### **2.3 Train/Test Split**

The final prepared dataset was split into training and testing subsets:

* **Ratio:** The data was split using a $\\mathbf{70\\%}$ training set and a $\\mathbf{30\\%}$ testing set.

* **Stratification:** The split used **stratification** (stratify=y) to ensure the severe **$90/10$ class imbalance** was preserved identically in both the training and testing sets. This is vital for the reliable evaluation of the model's ability to handle imbalance.  
