# AI-Assisted Customer Retention & Churn Analytics

Built an end-to-end customer churn analytics solution using Brazil's Olist e-commerce dataset (93K+ customers), combining customer segmentation, churn-risk prediction, and targeted retention strategies to identify at-risk customers, protect R$3M+ in revenue, and improve retention ROI.

---
<img width="2880" height="1620" alt="dashboard_1_overview" src="https://github.com/user-attachments/assets/ab6e1af2-4cdc-45c2-9000-cc311f992c8a" />


## Objective

- Identify **who** is likely to stop buying (churn-risk prediction).
- Understand **why** customers leave (delivery performance, satisfaction, spend behaviour).
- Segment customers by value and behaviour (RFM scoring and K-Means clustering).
- Quantify the **revenue at risk** and map each customer to a targeted retention action.

---

## Tech Stack

| Area | Tools |
| --- | --- |
| Language | Python 3.9+ |
| Environment | Jupyter / Google Colab |
| Data processing | Pandas, NumPy |
| Machine learning | Scikit-learn (StandardScaler, KMeans, Silhouette Score, LogisticRegression, train/test split) |
| Visualization | Matplotlib |
| BI hand-off | Final CSV export, ready to load into Power BI or Tableau |

---

## Project Structure

```
├── Data/
│   ├── olist_orders_dataset.csv                  raw Olist tables (Kaggle)
├── notebook/
│   ├── 01_data_cleaning.ipynb                    merge, clean, feature engineering
│   └── 02_eda_and_segmentation.ipynb             EDA, RFM segmentation, K-Means clustering
├── model/
│   └── 03_churn_model.ipynb                      churn model, retention strategy, export
├── images/
│   ├── dashboard_1_retention_overview.png
│   ├── dashboard_2_behavioural_clusters.png
│   └── dashboard_3_segment_revenue_risk.png
│   └── dashboard_4_behavioural_clusters.png
│   └── dashboard_5_churn_model_actions.png
├── Insights_and_outcomes.md                                   detailed insights and outcomes
└── README.md
```

Run the notebooks in order: `data_cleaning` > `eda_and_segmentation` > `churn_model`. Each one reads from and writes to the `Data/` folder, so set `drive_path` at the top of each notebook to your `Data/` location. Dataset: [Olist Brazilian E-Commerce on Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

---

## Insights & Outcomes

- **Delivery performance is the strongest churn signal.** A cluster of about 6,200 customers has a 99% late-delivery rate and a 2.26 average review score, against 4.0 to 4.3 elsewhere.
- **Most customers buy only once.** Revenue and orders kept growing, so the problem is retention, not falling demand.
- **About R$6.8M of R$15.4M revenue is at risk**, with Loyal customers holding the most (R$3.02M) and At-risk customers the highest share (46.0%).
- **3,107 high-value customers** (Champions and Loyal at 60%+ churn probability) are flagged for personal outreach.
- **Every customer gets a retention action**, from VIP recovery to automated win-back emails.

> **Caveat:** the churn model is weak (ROC-AUC 0.56), so use the scores to prioritize outreach, not to forecast.

Full breakdown of segments, clusters, model results, the action matrix and recommendations: **[INSIGHTS_OUTCOMES.md](INSIGHTS_OUTCOMES.md)**.

---

## Dashboard
<img width="2880" height="1620" alt="dashboard_1_overview" src="https://github.com/user-attachments/assets/ab6e1af2-4cdc-45c2-9000-cc311f992c8a" />
<img width="2880" height="1620" alt="dashboard_2_delivery_satisfaction" src="https://github.com/user-attachments/assets/af4e6dd1-fb88-49f8-b280-47bf115cb422" />
<img width="2880" height="1620" alt="dashboard_3_segments_revenue_risk" src="https://github.com/user-attachments/assets/80d9bd4a-be2e-49ed-a59b-73b2e7d5eb05" />
<img width="2880" height="1620" alt="dashboard_4_behavioural_clusters" src="https://github.com/user-attachments/assets/0bf36e26-b738-44ab-af1f-165f5aada873" />
<img width="2880" height="1620" alt="dashboard_5_model_actions" src="https://github.com/user-attachments/assets/2be75ca2-de22-4927-bc45-d47103c7d7fc" />

