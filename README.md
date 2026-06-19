# 🧴 Shopee Thailand Health & Wellness — Sales Analysis

End-to-end data analysis project on Health & Wellness products sold on Shopee Thailand, covering data cleaning, exploratory data analysis, Power BI dashboards, and machine learning.

## 📌 Project Overview

This project analyzes ~68,000 raw product listings (cleaned down to ~2,600 unique items) from the Health & Wellness category on Shopee Thailand. The goal is to understand what drives revenue and sales in this category, and to practice a full data analyst workflow: cleaning → EDA → visualization → machine learning → business storytelling.

**Tools used:** Python (pandas, NumPy, scikit-learn), Jupyter Notebook, Power BI

## 📂 Dataset

- **Source:** Shopee Thailand, Health & Wellness category
- **Raw size:** 68,499 rows
- **After cleaning:** 2,622 unique product listings
- **Columns:** Product ID, Section (category), Name, Price, Total Sold, Total Reviews, Shop Location

## 🧹 Data Cleaning

| Step | Action |
|------|--------|
| Duplicates | Removed ~96% duplicate rows (68,499 → 2,622) |
| Price | Stripped currency symbol/commas, converted to float |
| Total Sold / Total Reviews | Parsed Thai-language text (e.g. `"9,999+ ชิ้น"`), converted to numeric, filled missing values with 0 |
| Shop Location | Dropped rows with no location AND no sales/review activity; filled remaining missing locations as `"Unknown"` |

A deliberate decision was made to **keep** rows that share a Product ID but differ slightly in price — these likely reflect price updates or promotions over time rather than duplicate noise.

## 📊 Exploratory Data Analysis — Key Insights

- **Skin Nourishment** has the most listings (137) and the highest total revenue (฿36.6M), driven by high sales volume at a relatively low average price (฿301).
- **Protein** products have the highest customer engagement, with a Review Rate of 4.34 — more than 3x the next closest category — suggesting strong buyer trust despite a higher price point (avg ฿952).
- **89% of all products** are priced under ฿1,127, confirming this is a mass-market, price-accessible category.
- **Price is only weakly correlated with sales** (Price vs. Total Sold: r = -0.16) — cheaper does not reliably mean more sales in this category.
- **Rural provinces collectively out-earn Bangkok** (฿184M vs ฿164M total revenue), despite Bangkok having the most listings — provinces like Pattani and Sakon Nakhon show especially high average units sold per shop.

## 📈 Power BI Dashboards

**Dashboard 1 — Overview:** KPI cards (Total Products, Total Revenue, Avg Price), revenue by section, price segment breakdown, and a map of revenue distribution across Thailand, filterable by Section.

**Dashboard 2 — Deep Dive:** Top 10 products and locations by revenue, Bangkok vs. Rural comparison, revenue by price segment (treemap), price vs. sales scatter plot by section, and total reviews by section, filterable by Price Segment.

See `dashboard/dashboard_report.pdf` for a static view, or open `dashboard/Project_1.pbix` in Power BI Desktop for the full interactive experience.

## 🤖 Machine Learning

Two regression problems were explored using Linear Regression and Random Forest:

| Target | Best R² | Notes |
|--------|---------|-------|
| Predicting **Price** | 0.13 | Price is poorly explained by sales/review volume — likely driven by factors not in this dataset (brand, ingredients, packaging) |
| Predicting **Total Sold** (initial) | 0.978 | Suspiciously high — investigated further |
| Predicting **Total Sold** (after removing Total Reviews) | 0.372 | Reveals the true, more modest predictive signal |

**Finding — Data Leakage:** The initial 0.978 R² for predicting Total Sold was caused by data leakage: `Total Reviews` is a near-direct byproduct of `Total Sold` (you can only review what you've bought), so it told the model the answer rather than predicting it independently. After removing it, the more honest model — using Price, Review Rate, Section, and Location — achieved R² = 0.372, with **Review Rate (44%)** and **Price (31%)** emerging as the two most influential legitimate features.

This process — noticing an unrealistically high score, diagnosing the cause, and re-running a corrected model — was treated as a core part of the analysis rather than a footnote.

## 💡 Business Recommendations

1. **Prioritize review generation over price-cutting.** Review Rate has ~1.5x the influence of Price on sales volume — incentivizing post-purchase reviews (e.g. small discounts for verified reviews) may be more effective than promotions.
2. **Don't assume Bangkok is the primary market.** Rural provinces generate more total revenue collectively and show higher per-shop sales in several cases — there may be underserved demand worth targeting outside the capital.
3. **Re-evaluate pricing strategy for "Protein" and similar high-trust categories.** High Review Rate alongside a higher average price suggests buyers in this segment are less price-sensitive and more trust-driven — premium positioning may be sustainable here.

## 📁 Repository Structure

```
shopee-health-wellness-analysis/
├── README.md
├── requirements.txt
├── data/
│   └── health_wellness_cleaned.csv     # cleaned dataset (output of the notebook)
├── notebook/
│   └── projet.ipynb                    # full cleaning + EDA + ML workflow
└── dashboard/
    ├── Project_1.pbix                  # Power BI file (Dashboard 1 & 2, interactive)
    └── dashboard_report.pdf            # exported view of both dashboards
```

## 🛠️ How to Reproduce

```bash
pip install -r requirements.txt
jupyter notebook notebook/projet.ipynb
```

To view the dashboards: open `dashboard/dashboard_report.pdf` for a quick look, or open `dashboard/Project_1.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free) for the full interactive version with slicers and filters.

---
*This project was built as part of a Data Analyst portfolio for a cooperative internship application, with a deliberate focus on EDA-first thinking and transparent, honest reporting of model limitations (including the data leakage finding) over inflated metrics.*
