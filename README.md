# 🛒 Customer Segmentation — E-Commerce Retail
### RFM Analysis + K-Means Clustering for Customer Intelligence

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0+-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-1.4+-150458?style=flat&logo=pandas&logoColor=white)
![Unsupervised ML](https://img.shields.io/badge/ML-Unsupervised-8E44AD?style=flat)
![Status](https://img.shields.io/badge/Status-Completed-2ECC71?style=flat)

---

## 📌 Project Overview

Not all customers are equal. This project applies **RFM Analysis** (Recency, Frequency, Monetary) and **K-Means Clustering** to segment customers of a UK-based online retail store into 4 distinct groups — each with a tailored marketing strategy.

Using ~1 million transaction records from the [Online Retail II](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) dataset, we identified **Champions, Loyal Customers, At Risk, and Lost/Inactive** customer segments, validated the clustering statistically, and translated results into actionable business recommendations.

---

## 🗂️ Project Structure

```
customer-segmentation/
│
├── data/
│   └── online_retail_II.xlsx          # Raw dataset (from Kaggle)
│
├── notebooks/
│   └── customer_segmentation.ipynb    # Full analysis notebook
│
├── reports/
│   ├── Customer_Segmentation_Report.pdf   # Business PDF report
│   └── Customer_Segmentation_Summary.pptx # Stakeholder presentation (11 slides)
│
├── README.md
└── requirements.txt
```

---

## 📊 Dataset

| Property | Value |
|---|---|
| Source | [Kaggle — Online Retail II (UCI)](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) |
| Raw Rows | ~1,000,000 transactions |
| UK Customers | ~4,300 (after cleaning) |
| Date Range | December 2009 – December 2011 |
| Columns | Invoice, StockCode, Description, Quantity, InvoiceDate, Price, Customer ID, Country |

---

## 🔧 Tech Stack

| Category | Tools |
|---|---|
| Language | Python 3.8+ |
| Data Manipulation | pandas, numpy |
| Visualization | matplotlib, seaborn |
| Preprocessing | scikit-learn (StandardScaler) |
| Clustering | scikit-learn (KMeans) |
| Validation | Elbow Method, Silhouette Score |
| Dimensionality Reduction | PCA (2D visualisation) |

---

## 💡 The RFM Framework

RFM is a proven, industry-standard customer analytics framework:

| Dimension | Definition | Good Signal |
|---|---|---|
| **Recency (R)** | Days since last purchase | Low = recently active |
| **Frequency (F)** | Total number of orders | High = loyal buyer |
| **Monetary (M)** | Total amount spent (£) | High = high value |

By computing R, F, M per customer and clustering them, we create a data-driven segmentation without needing any labelled data.

---

## 🔍 Analysis Workflow

### 1. Data Cleaning
- Removed cancelled invoices (prefix `C`)
- Dropped rows with missing `Customer ID`
- Removed invalid quantities (≤0) and prices (≤0)
- Filtered non-product stock codes (POST, D, M, DOT, etc.)
- Focused on UK customers only (85%+ of all transactions)
- Created `TotalPrice = Quantity × Price`

### 2. Exploratory Data Analysis
- Monthly revenue trend — clear Q4 seasonal spike each year
- Orders peak Tuesday–Thursday, 10AM–2PM
- Revenue is highly right-skewed — strong Pareto effect (top 20% customers → majority of revenue)
- Top 10 products by revenue identified

### 3. RFM Feature Engineering
- Reference date set to 1 day after last transaction
- Computed Recency, Frequency, Monetary per customer
- Applied **log transformation** (log1p) to reduce right skew in all 3 features
- Applied **StandardScaler** — ensures all 3 features contribute equally to K-Means distance

### 4. Finding Optimal K
| Method | Result |
|---|---|
| Elbow Method | Elbow visible at K=3–4 |
| Silhouette Score | Peak at K=4 |
| Business Logic | 4 segments = actionable marketing plan |

**Chosen K = 4**

### 5. K-Means Clustering
- Algorithm: `k-means++` initialisation
- 10 random initialisations (`n_init=10`)
- PCA used to project clusters into 2D for visual validation

### 6. Cluster Profiling

| Segment | Recency | Frequency | Monetary | Revenue Share |
|---|---|---|---|---|
| 🏆 Champions | Very Low (recent) | Very High | Highest | Largest |
| 💛 Loyal Customers | Low–Moderate | High | Moderate–High | 2nd largest |
| ⚠️ At Risk | High (fading) | Moderate | Moderate | Declining |
| 😴 Lost / Inactive | Very High | Low | Lowest | Smallest |

---

## 💼 Business Recommendations

| Segment | Strategy | Channel | KPI |
|---|---|---|---|
| 🏆 Champions | VIP rewards, early product access, referrals | Email, App push | Churn rate, NPS |
| 💛 Loyal Customers | Upsell, subscription offers, personalised recs | Email, Landing page | Upgrade rate |
| ⚠️ At Risk | Win-back: 15-20% time-limited discount | Email, SMS, Retargeting | Re-activation in 30d |
| 😴 Lost / Inactive | Final offer (30%+); if no response → stop | Email (last attempt) | <5% → remove |

**Budget Allocation Principle:**
- Champions: 30–35% of marketing budget
- Loyal Customers: 35–40%
- At Risk: 20–25%
- Lost / Inactive: 5–10%

---

## 🔑 Key Findings

1. **Pareto Effect Confirmed** — A small % of customers (Champions) generate a disproportionate share of revenue. Protecting them is the single highest-ROI action.
2. **Log Transformation is Essential** — Raw RFM features were too skewed for K-Means. Transformation significantly improved cluster quality and separation.
3. **K=4 is Statistically & Practically Optimal** — Validated by both Elbow and Silhouette methods, and maps to 4 business-interpretable segments.
4. **At Risk = Hidden Opportunity** — These customers were once valuable; a targeted win-back campaign can recover significant revenue.
5. **Seasonality Matters** — Q4 dominates revenue. Campaign timing must align with the October–November gifting peak.

---

## 🚀 How to Run

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl jupyter
```

### Steps
1. Download `online_retail_II.xlsx` from [Kaggle](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci)
2. Place it in the `data/` folder (or same folder as notebook)
3. Open Jupyter Notebook:
```bash
jupyter notebook notebooks/customer_segmentation.ipynb
```
4. Run all cells (Kernel → Restart & Run All)

### requirements.txt
```
pandas>=1.4.0
numpy>=1.21.0
matplotlib>=3.5.0
seaborn>=0.11.0
scikit-learn>=1.0.0
openpyxl>=3.0.0
jupyter>=1.0.0
```

---

## 📁 Deliverables

| File | Description |
|---|---|
| `customer_segmentation.ipynb` | Full pipeline: EDA → RFM → K-Means → Profiling → Recommendations |
| `Customer_Segmentation_Report.pdf` | 8-page business report |
| `Customer_Segmentation_Summary.pptx` | 11-slide stakeholder presentation |
| `README.md` | This file |

---

## 🗺️ Portfolio Roadmap

| # | Project | Status | Key Skills |
|---|---|---|---|
| 1 | RSVP Movies SQL Case Study | ✅ Done | SQL, Business Queries |
| 2 | Netflix EDA | ✅ Done | Python, pandas, Visualization |
| 3 | Lead Scoring | ✅ Done | Logistic Regression, Feature Engineering |
| 4 | Credit Risk Analysis | ✅ Done | XGBoost, ROC-AUC, Class Imbalance |
| **5** | **Customer Segmentation** | ✅ **Done** | **RFM, K-Means, Unsupervised ML** |
| 6 | E-commerce Capstone | 🔄 Next | Full-stack DA/DS |

---

## 👤 Author

**[Your Name]**
- LinkedIn: [your-linkedin-url]
- GitHub: [your-github-url]
- Email: [your-email]

---

*Project 5 of 5 — DA/BA Portfolio | March 2026*
