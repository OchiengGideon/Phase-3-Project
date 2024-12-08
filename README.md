# Predictive Analytics for Waterpoint Operational Status in Tanzania
![Map of Tanzania](Images/Tanzania.jpg)
Tanzania relies heavily on waterpoints to provide clean and accessible water to its population. However, many waterpoints are either non-functional or in need of repair, leading to inefficiencies and hardships for communities that depend on them. This project seeks to address this issue by building a machine learning model to predict the operational status of waterpoints, enabling better resource allocation and proactive maintenance.
## Table of Contents
1. [Overview](#overview)
2. [Business Understanding](#business-understanding)
   - [Objectives](#objectives)
   - [Stakeholders](#stakeholders)
   - [Success Criteria](#success-criteria)
   - [Key Questions](#key-questions)
   - [Constraints](#constraints)
   - [Potential Impact](#potential-impact)
3. [Data Understanding](#data-understanding)
   - [Dataset](#dataset)
   - [Data Details](#data-details)
4. [Workflow](#workflow)
   - [Business Understanding Step](#business-understanding-step)
   - [Data Understanding Step](#data-understanding-step)
   - [Data Preparation](#data-preparation)
   - [Modeling](#modeling)
   - [Evaluation](#evaluation)
5. [Rationale](#rationale)
6. [Results](#results)
7. [Limitations](#limitations)
8. [Recommendations](#recommendations)
9. [Tableau Dashboard](#tableau-dashboard)

---

## Overview
This project aims to predict the operational status of waterpoints in Tanzania using machine learning techniques. It addresses critical challenges in water resource management by identifying whether a waterpoint is functional, needs repair, or is non-functional, enabling better planning and allocation of resources.

---

## Business Understanding
### Objectives
The primary goal of this project is to predict the operational status of waterpoint in Tanzania. By accurately predicting whether it is fuctional, needs repair or is non-fuctional, it can help the Tanzania Ministry of water and other stakeholders in optimizing operations ansd ensure a reliable supply of clean water to communities.
### Stakeholders
- **Tanzania Ministry of Water:** Responsible for infrastructure and resource allocation.
- **Local Communities:** Rely on waterpoints for daily needs.
- **Maintenance Teams:** Tasked with repairs and operational upkeep.

### Success Criteria
- **Accuracy:** The model should have a high accuracy in predicting the status of waterpoints.
- **Actionable Insight:** The predictions should lead to actionable insight that can improve maintainance schedules and resouce allocation
- **Scalability:** The solution should be scalable to handle data from other regions or countries

### Key Questions
- Which factors most influence the operational status of waterpoints?
- How can we prioritize waterpoints for maintenance based on the model’s predictions?
- What patterns or trends can be identified from the data that could inform future waterpoint installations?

### Constraints
- **Data Quality**: The accuracy of the model depends on the quality and completeness of the
data.
- **Resource Limitations:** Limited resources for maintenance and repairs may affect the implementation of the model’s recommendations.
- **Geographical Challenges:** Remote or hard-to-reach areas may pose challenges for data
collection and maintenance.

### Potential Impact
- **Improved Water Access:** Ensuring that more waterpoints are functional can significantly
improve access to clean water for communities.
- **Cost Savings:** Predictive maintenance can reduce costs by preventing major breakdowns
and optimizing resource allocation.
- **Enhanced Decision-Making:** Data-driven insights can help policymakers and stakeholders
make informed decisions about water infrastructure investments.

---

## Data Understanding
### Dataset
The project uses the following files:
1. **`training_set_values.csv`:** Feature data describing waterpoints.
2. **`training_set_labels.csv`:** Operational status labels.
3. **`test_set_values.csv`:** Feature data for predictions.

### Data Details
- **Features:** Include geographical, operational, and structural attributes of waterpoints.
- **Target Classes:** 
  - Functional
  - Non-Functional

---

## Workflow
### Business Understanding Step
Tanzania relies heavily on waterpoints to provide clean and accessible water to its population. However, many waterpoints are either non-functional or in need of repair, leading to inefficiencies and hardships for communities that depend on them. This project seeks to address this issue by building a machine learning model to predict the operational status of waterpoints, enabling better resource allocation and proactive maintenance.

### Data Understanding Step

#### **Dataset Overview**
- **Number of Records:** 59,400  
- **Number of Features:** 41  
- **Target Variable:** `status_group`  
  - Categories: *Functional*, *Needs Repair*, *Non-Functional*  
  - Imbalance observed, with a majority of water points being *Functional*.  

---

#### **Key Observations**
1. **Missing Values:**
   - Columns like `funder`, `installer`, `public_meeting`, and `scheme_name` had missing values.  
   - Imputation was performed:
     - Replaced missing categorical values with `'Unknown'`.
     - Replaced missing numerical values in `construction_year`, `longitude`, and `population` with median or domain-specific values.

2. **Outliers:**
   - Outliers were identified in `longitude` and `latitude`.  
   - These were removed using the Interquartile Range (IQR) method to ensure accurate modeling.

3. **High Cardinality Columns:**
   - Columns such as `id` (59,400 unique values), `wpt_name`, and `subvillage` have extremely high cardinality, suggesting diverse geographic and structural data points.

4. **Geographical Features:**
   - Features like `longitude`, `latitude`, and `region` indicate a wide geographical spread.
   - Average coordinates align with Tanzania's location, but some entries showed errors (e.g., GPS height below sea level).

5. **Target Distribution:**
   - `status_group` has three categories:
     - *Functional*: 32,259 entries
     - *Needs Repair*: Moderate proportion
     - *Non-Functional*: Remaining entries  

---

#### **Summary Statistics**
#### Numerical Features:
- **Amount of Tanzanian Shilling (`amount_tsh`)**
  - Mean: 318; Max: 350,000 (high variability in budget).  
- **Population**
  - Mean: 180; Max: 30,500 (indicates some waterpoints serve large communities).  
- **Construction Year**
  - Median: 1986; Missing values replaced with the median.  

#### Categorical Features:
- **Funder and Installer**
  - Most common: *Government of Tanzania* and *DWE*.  
- **Region**
  - Iringa has the highest number of entries.
- **Water Quality**
  - Majority have "soft" water, sourced from springs.
- **Waterpoint Type**
  - Communal standpipes are most common, indicating a focus on shared water access.

---

### Data Preparation
1. **Duplicates:**
   - No duplicate records were found.  

2. **Imputation:**
   - Missing values and zeros were imputed based on domain knowledge:
     - Replaced missing categorical values with `'Unknown'`.
     - Filled zeros in numerical columns (e.g., `construction_year`) with the median or appropriate default.

3. **Outliers:**
   - Outliers in `longitude` and `latitude` were removed to refine the dataset.  

---

### 4. Modeling
### **Modeling Summary**

#### **6.1 Splitting the Data**
- The dataset was split into **80% training** and **20% testing**.
- Numerical features were scaled using `StandardScaler`.
- Categorical features were encoded using one-hot encoding with `pd.get_dummies` to avoid multicollinearity.
- The training and testing sets were aligned to ensure consistent feature representation.

---

### **6.2 Logistic Regression**
- A baseline Logistic Regression model was trained using the encoded training data.
- **Model Evaluation**:
  - **Accuracy**: 0.77
  - **Precision**: 0.78
  - **Recall**: 0.77
  - **F1-Score**: 0.77
  - **ROC-AUC**: 0.84
- **Confusion Matrix**: Showed the distribution of true positives, false positives, true negatives, and false negatives.
- **Hyperparameter Tuning**:
  - A grid search was conducted for tuning hyperparameters such as `C`, `solver`, and `penalty`.
  - **Best Parameters**: `{C: 100, max_iter: 2000, penalty: 'l1', solver: 'saga'}`
  - The final tuned Logistic Regression model showed improved performance.

---

### **6.3 Decision Tree Classifier**
- A vanilla Decision Tree Classifier was trained with default parameters:
  - **ROC-AUC**: 0.80 (Baseline)
  - **Confusion Matrix**:
    - **True Negatives (TN)**: 3243
    - **False Positives (FP)**: 1183
    - **False Negatives (FN)**: 835
    - **True Positives (TP)**: 6257
- **Hyperparameter Tuning**:
  - Conducted using `GridSearchCV` with parameters:
    - `max_depth`: [None, 5, 10, 20]
    - `min_samples_split`: [0.01, 2, 5, 10]
    - `min_samples_leaf`: [0.01, 1, 2, 4]
  - **Best Parameters**: `{criterion: 'gini', max_depth: 20, min_samples_split: 2, min_samples_leaf: 1}`
- **Final Decision Tree Performance**:
  - **Cross-validated Accuracy**: 0.79
  - **ROC-AUC**: 0.82
  - **Confusion Matrix**:
    - TN: 3243, FP: 1183, FN: 835, TP: 6257

---

### **Overall Performance Metrics (Final Decision Tree)**
- **Accuracy**: 0.79
- **Precision**: 0.79 (weighted)
- **Recall**: 0.79 (weighted)
- **F1 Score**: 0.79 (weighted)
- **AUC**: 0.82

The confusion matrix highlights the model's ability to distinguish between classes, with relatively strong performance in predicting true positives and true negatives. 

---

### **Conclusion**
1. The **Logistic Regression model** performed well and was improved through hyperparameter tuning. It exhibited strong predictive performance and interpretability.
2. The **Decision Tree model** provided comparable results, with slightly better flexibility after tuning but with potential overfitting risks due to its nature.
3. The **ROC curve analysis** shows that both models perform better than a random classifier, with Logistic Regression slightly leading in stability and Decision Tree offering simplicity and interpretability.
4. **Recommendation**: Logistic Regression may be preferred for balanced datasets, while Decision Trees are useful for scenarios requiring interpretability and when hyperparameter tuning is applied to mitigate overfitting.

---

## Results
### Classification Metrics
- **Accuracy:** Evaluates the overall correctness of predictions.
- **Precision and Recall:** Chosen to assess performance on "Needs Repair" and "Non-Functional" classes, which have significant real-world implications.
- **F1-score:** Balances precision and recall, providing a comprehensive metric for imbalanced data.
- **ROC-AUC:** Measures model performance across thresholds, ensuring robustness.

### Business Implications
- High precision in identifying "Needs Repair" points ensures resources are not wasted.
- High recall minimizes the risk of overlooking critical waterpoints, reducing downtime.

---

## Limitations
### Model Challenges
- Predictions may be affected by noisy data in production.

---

## Recommendations
### For Stakeholders
- **Prioritize Repairs:** Use predictions to focus on waterpoints identified as "Needs Repair" to maximize impact.
- **Regular Data Updates:** Continuously update the dataset with new and accurate information to improve model performance.
- **Integrate with GIS Tools:** Combine predictions with geographical information for better planning and visualization.

## tableau-dashboard
- Click here to access the dashboard: [Tableau dashboard](https://public.tableau.com/views/WaterPointoperationalstatusinTanzania/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
![Dashboard image](Images/Tableau.png)

