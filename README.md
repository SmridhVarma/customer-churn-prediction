### Business Problem and Evaluation Strategy

The project is grounded in the marketing principle that acquiring new customers is **5 to 25 times more expensive** than retaining existing ones. To reflect this, the team designed a custom loss function () where failing to identify a churner (**False Negative**) is penalized at **$100**, while incorrectly flagging a loyal customer (**False Positive**) costs only **$10** in retention incentives.

By using this 10:1 cost ratio, the models were optimized to be more aggressive in predicting churn, ultimately identifying an optimal classification threshold (e.g., **11%** for the blackbox model) that is much lower than the standard **50%** cutoff.

---

### Data and Pre-Processing

The study utilized the **Telco Customer Churn dataset** from Kaggle, containing **7,043 customer records** with 21 features.

* **Target:** Binary churn variable (Yes/No) with a **26.5% churn rate**.

* **Cleaning:** Addressed datatype inconsistencies and removed a negligible number of empty strings in 'TotalCharge'.

* **Transformation:** Applied **one-hot encoding** for categorical variables and **StandardScaler** for continuous variables to ensure stable model optimization.



---

### Model Performance Comparison

The team evaluated several "Glass-box" (interpretable) and "Blackbox" (complex) models using an **80/20 train/validation split**.

| Model Category | Champion Model | Performance Metrics | Key Takeaway |
| --- | --- | --- | --- |
| **Glass-box** | <br>**Scaled Logistic Regression** | AUC: 0.836, Min Loss: $7,090 | Scaling was vital for stable optimization and fair L2 regularization.|
| **Blackbox** | <br>**Optimized XGBoost** | AUC: 0.841, Min Loss: $7,090 | Captured non-linear relationships better than linear models.|

Models trained with **Surrogate Loss (Log Loss)** consistently matched or outperformed those trained with **L2 Loss (Mean Squared Error)** for this binary classification task.

---

### Key Churn Drivers and Interpretability

To explain the complex XGBoost model, the team applied **SHAP (SHapley Additive exPlanations)** to extract feature importance.

* **Top Retention Drivers:** Having a **Two-Year Contract** and **high tenure** were the strongest indicators that a customer would stay.


* **Top Churn Risks:** Customers with **Fiber Optic internet** and those using **Electronic Check** payments were at a significantly higher risk of leaving.


* **Low Impact:** Factors like gender (**Is_Male**) or having device protection had minimal influence on the final predictions.



---

### Final Conclusions

* **Financial Savings:** Optimizing for minimum business cost saved **$2,940** (approx. **29%**) compared to a standard balanced F1-score approach.


* **Sensitivity:** The final optimized model achieved a **95% recall**, successfully catching the vast majority of potential churners.

Further Reading and Reasoning Behind this design of loss function: https://hbr.org/2014/10/the-value-of-keeping-the-right-customers

Dataset and Problem Scenario Source: https://www.kaggle.com/datasets/blastchar/telco-customer-churn

