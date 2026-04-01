# 📦 Supply Chain Analytics: Predicting Late Delivery Risks

![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.2+-orange.svg)
![XGBoost](https://img.shields.io/badge/XGBoost-Latest-green.svg)
![Status](https://img.shields.io/badge/Status-Completed-success.svg)

## 📌 Business Overview
In global logistics, late deliveries lead to customer dissatisfaction, negative reviews, and increased support costs. The goal of this project is to build a robust Machine Learning pipeline capable of predicting **Late Delivery Risks** *before* an order is physically shipped. 

By identifying high-risk orders at the moment of checkout, the business can proactively intervene (e.g., upgrading shipping tiers, notifying the warehouse, or managing customer expectations).

## 🔬 Key Methodologies & Techniques
This project prioritizes mathematical rigor and strict prevention of Data Leakage to ensure the model performs reliably in a real-world production environment.

### 1. Advanced Exploratory Data Analysis (EDA)
* **Statistical Outlier Handling:** Applied Z-Scores for normally distributed features (Profit) and IQR for right-skewed features (Prices, Sales). 
* **Strategic Retention:** Consciously chose to retain logistical outliers (extreme delays/losses) as they represent critical edge cases required for tree-based models to learn real-world failures.

### 2. Feature Selection & Engineering
* **Chi-Square Tests of Independence:** Filtered out noisy categorical demographics (e.g., Customer Segment) and prevented Data Leakage by dropping post-event statuses (`Delivery Status`, `Order Status`).
* **Numerical Evaluation:** Combined Variance Thresholds, Pearson Correlation, and Mutual Information (`mutual_info_classif`) to drop uninformative financial metrics and leaky features (`Days for shipping (real)`).
* **Temporal Engineering:** Extracted `Order_Month` and `Order_Day_of_Week` to capture seasonal rushes and warehouse weekend bottlenecks.

### 3. Production-Ready Pipelines
Built a seamless `scikit-learn` Pipeline utilizing a `ColumnTransformer` to handle preprocessing dynamically during Cross-Validation, ensuring zero data leakage:
* `MinMaxScaler` for temporal/numerical features.
* `OrdinalEncoder` for intrinsic hierarchies (`Shipping Mode`).
* `OneHotEncoder` for nominal geographies and payment types.

## 📊 Model Performance & Insights

We evaluated multiple algorithms using `GridSearchCV` to optimize hyperparameters. The project revealed a fascinating tradeoff between overall accuracy and business precision.

### 🏆 Model 1: Random Forest (The Well-Rounded Baseline)
* **Accuracy:** 75%
* **Precision (Late Deliveries):** 80%
* **Key Insight:** The Random Forest identified that delays are fundamentally a scheduling issue. The top predictive features were `Shipping Mode`, `Order_Day_of_Week`, and `Scheduled Days`. Geography played a minimal role.

### 🚀 Model 2: XGBoost (The High-Precision Specialist)
* **Accuracy:** 72%
* **Precision (Late Deliveries):** **82%**
* **Key Insight:** While slightly less accurate overall, XGBoost proved highly conservative and precise. It uncovered hidden bottlenecks that the Random Forest missed:
  * **Payment Delays:** Orders paid via `TRANSFER` showed high delay risks (likely due to bank clearance times).
  * **Geographic Bottlenecks:** Specific international routes (South America, Oceania, Eastern Europe) were mathematically flagged as systemic failure points.

### 📈 Final Verdict
* **Random Forest** is recommended for general deployment on the storefront to flag general delays.
* **XGBoost** is recommended for internal logistics teams to investigate specific geographical and payment-related operational bottlenecks.

---

## 💻 Installation & Usage

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/YourUsername/Supply-Chain-Delay-Prediction.git](https://github.com/YourUsername/Supply-Chain-Delay-Prediction.git)
   cd Supply-Chain-Delay-Prediction