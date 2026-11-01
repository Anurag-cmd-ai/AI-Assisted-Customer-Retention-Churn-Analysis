# Insights & Outcomes

Findings from the Olist customer retention and churn analysis. Scope: **93,358 unique customers**, **110,197 delivered order items** and **R$15.4M** in revenue. Churn is defined as no purchase in the last 120 days. The full write-up is in `Project_Report.docx`.

---

## 1. Headline Findings

1. **Delivery performance is the strongest driver of churn.** A cluster of about 6,200 customers has a 99% late-delivery rate and a 2.26 average review score, against 4.0 to 4.3 for every other cluster.
2. **Most customers buy only once.** Revenue and order volume grew steadily over the two years, so the problem is retention, not falling demand.
3. **About R$6.8M of revenue is exposed** (customers with a churn probability of 50% or more), out of R$15.4M in total.
4. **Loyal customers hold the most money at risk** (R$3.02M), while **At-risk customers are the most exposed in relative terms** (46.0% of their revenue).
5. **3,107 high-value customers** (Champions and Loyal at 60%+ churn probability) need personal outreach now.

---

## 2. Exploratory Analysis

| Area | What the data shows |
| --- | --- |
| Revenue and orders | Both grew steadily with no major drops. Growth came mainly from more orders, not larger orders. |
| Categories | A small number of categories generate most of the revenue. Some categories have very high order counts but low revenue (cheap, frequently ordered items). |
| Delivery and satisfaction | As review scores rise, both delivery delay and the share of late orders fall. Late orders go with low reviews. |
| Geography | On-time delivery varies widely by state, and a small group of states accounts for most late deliveries. |
| Payments | Most orders use just one or two payment methods. |
| Reviews | Most reviews are 4 to 5 stars, but a meaningful share are 1 to 2 stars, and those are strongly associated with late deliveries. |
| Repeat purchase | Most customers place one order and never return. |
| Freight | Most orders have low shipping costs, but a smaller group pays much more. High freight may be a secondary reason customers leave. |

---

## 3. Customer Segmentation

### RFM segments

Customers are scored 1 to 5 on recency, frequency and monetary value (quintiles), summed, and mapped to a segment.

| Segment | Customers | Avg churn prob. | Total revenue | Revenue at risk | % at risk |
| --- | ---: | ---: | ---: | ---: | ---: |
| Loyal customers | 31,508 | 50.2% | R$6.77M | R$3.02M | 44.6% |
| At-risk customers | 38,382 | 50.8% | R$4.97M | R$2.29M | 46.0% |
| Champions | 8,033 | 48.3% | R$2.68M | R$1.10M | 41.2% |
| Lost customers | 15,435 | 50.8% | R$1.00M | R$0.40M | 40.2% |

### Behavioural clusters (K-Means, k = 4)

| Cluster | Recency (days) | Monetary | Late-order rate | Avg review | Customers |
| --- | ---: | ---: | ---: | ---: | ---: |
| 0 - Big spenders, slipping away | 241.5 | R$1,143 | 5% | 4.03 | 2,576 |
| 1 - Dormant low-spenders | 390.1 | R$133 | 0% | 4.27 | 36,042 |
| 2 - Healthy recent shoppers | 127.0 | R$138 | 0% | 4.31 | 48,527 |
| 3 - Logistics-failure group | 220.6 | R$154 | 99% | 2.26 | 6,213 |

- Cluster 3 is defined by delivery failure, not price or category, and it appears in **every** RFM segment, so logistics-driven risk cuts across value tiers.
- Cluster 0 pairs high spend with high recency, matching RFM's at-risk high-value group.
- Cluster 2 has an average recency of 127 days, just past the 120-day cutoff, so "healthy" refers to satisfaction and delivery, not recency.
- The silhouette score peaks at k = 3 (about 0.56) and drops at k = 4 (about 0.32). k = 4 was chosen because it gives four distinct, actionable groups.

---

## 4. Churn Model

Logistic regression on 8 behavioural features (75/25 stratified split, balanced class weights).

| Metric | Retained (0) | Churned (1) |
| --- | ---: | ---: |
| Precision | 0.29 | 0.77 |
| Recall | 0.64 | 0.43 |
| F1-score | 0.40 | 0.55 |
| Test customers | 6,203 | 16,986 |

**Accuracy 0.49, ROC-AUC about 0.56.** The model is only marginally better than random guessing, so `churn_probability` is a directional signal for ranking outreach, not a precise forecast.

- Higher late-order rate, order value and installment count push toward churn.
- Higher review score, monetary value and category count push toward retention.
- Freight cost has a counter-intuitive direction, likely due to confounding.

---

## 5. Outcomes: Retention Action Matrix

Each customer is assigned one action from their RFM segment, churn probability and late-order rate.

| # | Tier | Criteria | Action |
| --- | --- | --- | --- |
| 1 | Priority VIP Recovery | Champions / Loyal, churn prob. 70%+ | Direct outreach, apology credits, priority handling |
| 2 | Proactive Retention | Champions / Loyal, churn prob. 40-70% | Loyalty rewards, retention offers |
| 3 | Upsell & Advocacy | Champions / Loyal, churn prob. under 40% | Early access, cross-sell, referrals |
| 4 | Logistics Service Recovery | At-risk with any late deliveries | Shipping vouchers, tracking updates |
| 5 | Win-Back Discount | At-risk with no late deliveries | Targeted win-back discount |
| 6 | Automated Reactivation | Lost customers | Low-cost automated drip email |

### Recommendations

1. **Fix delivery reliability first.** Target the states and routes with the worst on-time rates.
2. **Protect high-value revenue.** Give Loyal customers proactive communication, loyalty benefits and personalized offers.
3. **Launch a VIP recovery program** for the 3,107 high-risk Champions and Loyal customers, with personal outreach instead of broad campaigns.
4. **Match the action to the cause:** service recovery for late-delivery customers, win-back discounts for other At-risk customers, automated emails for Lost customers.
5. **Act before customers lapse.** Reach out as customers approach the 120-day mark, because prevention is cheaper than win-back.
6. **Improve post-purchase communication** with delivery tracking and proactive delay updates.

---

## 6. Limitations

- **Churn label:** no purchase in 120 days flags about 73% of customers because most Olist customers order only once. It identifies lapsed customers, not confirmed attrition.
- **Model strength:** with ROC-AUC near 0.56, individual churn probabilities are weak. The 40% / 70% action tiers and the 50% revenue-at-risk cut-off are prioritisation rules, not forecasts.
- **Cluster stability:** K-Means depends on initialisation, so re-running the notebook in a different environment can give different cluster sizes. The cluster figures above come from the original run.
- **Next steps:** use a time-based holdout, add richer features (category, region, delivery time), try a non-linear model such as gradient boosting, and A/B test the retention actions to measure real lift.
