# **DA5401 Assignment 8: Ensemble Learning for Complex Regression Modeling on Bike Share Data**  
- **AKSHAY KUMAR G**  
- **ME22B044**

---

## **Objective**  
To apply and compare three major **ensemble learning techniques — Bagging, Boosting, and Stacking —** for solving a complex regression task on the **Bike Sharing Demand Dataset**.  
The goal is to accurately predict the **total count of rented bikes (cnt)** by modeling nonlinear relationships influenced by weather, season, and time features.

---

## **Problem Statement**

Accurately forecasting bike rental demand is critical for urban transport management.  
The dataset contains over **17,000 hourly samples**, with variables such as temperature, humidity, windspeed, and temporal features.

We evaluate how ensemble learning techniques can:
- Reduce **variance** (Bagging),
- Reduce **bias** (Boosting), and
- Optimize performance via **model stacking (meta-learning)**.

---

## **Tasks & Solutions**

### **Part A: Data Preprocessing and Baseline Model**

1. **Data Preparation**
   - Loaded the dataset `hour.csv`.  
   - Dropped irrelevant columns: `instant`, `dteday`, `casual`, and `registered`.  
   - Performed **One-Hot Encoding** for categorical variables like `season`, `mnth`, `hr`, `weekday`, and `weathersit`.  
   - Split data into **80% training** and **20% testing** sets.

2. **Baseline Models**
   - Trained:
     - **Linear Regression**
     - **Decision Tree Regressor** (`max_depth=6`)
   - Evaluated using **Root Mean Squared Error (RMSE)**.

| Model | RMSE |
|--------|------|
| Decision Tree | 118.00 |
| **Linear Regression (Baseline)** | **100.45** |

The **Linear Regression** model performed better and was chosen as the **baseline**.

---

### **Part B: Ensemble Techniques for Bias and Variance Reduction**

#### **1. Bagging (Variance Reduction)**  
- Implemented **Bagging Regressor** using a **Decision Tree Regressor (max_depth=6)** as the base estimator with **100 trees**.  
- Bagging reduces **variance** by averaging multiple independent high-variance models trained on different data subsets.

| Model | RMSE |
|--------|------|
| **Bagging (DT Base)** | **112.34** |

Bagging improved upon the single Decision Tree (118 → 112), showing effective variance reduction.  
Although RMSE is slightly higher than Linear Regression, it should be compared with the **Decision Tree baseline**, not Linear Regression, since bagging targets tree variance.

---

#### **2. Boosting (Bias Reduction)**  
- Implemented **Gradient Boosting Regressor** with:
  - 300 estimators,
  - learning_rate = 0.2,
  - max_depth = 6.
- Boosting sequentially learns from previous model errors, reducing **bias**.

| Model | RMSE |
|--------|------|
| **Gradient Boosting** | **48.15** |

Boosting drastically improved performance, demonstrating strong bias correction and better generalization.

---

### **Part C: Stacking for Optimal Performance**

#### **Principle of Stacking**
**Stacking** is an advanced ensemble technique that combines predictions from multiple diverse base learners (Level-0 models) using a **Meta-Learner (Level-1 model)** to achieve optimal predictive performance.

- **Level-0 (Base Learners):**  
  KNN Regressor, Bagging Regressor, Gradient Boosting Regressor.  
  Each captures different data patterns — local, variance-reduced, and bias-reduced respectively.

- **Level-1 (Meta-Learner):**  
  Ridge Regression — learns optimal weights for each base learner by minimizing prediction error (RMSE) on validation data.

The meta-learner receives the base models’ predictions as inputs and **learns how to best combine them** by assigning higher weights to models that perform better on specific regions of the data.

| Model | RMSE |
|--------|------|
| **Stacking Regressor** | **47.77** |

Stacking achieved the **lowest RMSE**, confirming that combining diverse learners balances bias and variance effectively.

---

### **Part D: Final Analysis**

#### **Comparative Performance**
<img width="695" height="471" alt="image" src="https://github.com/user-attachments/assets/8238765b-df18-4770-bf98-d40df9895007" />



| Model | RMSE | Remarks |
|--------|------:|---------|
| Linear Regression (Baseline) | 100.45 | High bias, low variance |
| Bagging (DT Base) | 112.34 | Reduced variance vs single tree |
| Gradient Boosting | 48.15 | Strong bias reduction |
| **Stacking Regressor** | **47.77** | **Best overall model** |

---

#### **Discussion**

- **Bagging**: Stabilized Decision Trees by averaging predictions — reduced variance but not bias.  
- **Boosting**: Sequentially minimized bias, achieving strong accuracy improvements.  
- **Stacking**: Combined diverse learners’ strengths (KNN, Bagging, Boosting) through a meta-learner, yielding optimal performance.

---

## **Conclusion**

- The **Stacking Regressor** outperformed all other models, achieving the **lowest RMSE (47.77)**.  
- **Bagging** reduced variance effectively compared to a single tree.  
- **Gradient Boosting** provided strong bias reduction.  
- **Stacking** leveraged model diversity, combining the strengths of each ensemble for superior generalization.

---

**Final Recommendation:**  
Use **Stacking Regressor** for production deployment — it offers the most accurate, stable, and generalizable predictions for bike rental forecasting.  
