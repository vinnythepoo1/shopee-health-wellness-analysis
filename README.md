<div align="center">
<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=28&pause=1000&color=E8A838&center=true&vCenter=true&width=700&lines=Shopee+Health+%26+Wellness+Sales+Analysis;2%2C622+Products+%C2%B7+30+Sections+%C2%B7+%E0%B8%BF349M+Revenue;Python+%2B+Power+BI+%2B+Machine+Learning" alt="Typing SVG" />
<br/>
![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![pandas](https://img.shields.io/badge/pandas-Data_Wrangling-150458?style=for-the-badge&logo=pandas&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
 
![Status](https://img.shields.io/badge/status-complete-2EA043?style=flat-square)
![Last Commit](https://img.shields.io/badge/maintained-2026-blue?style=flat-square)
 
</div>
---
 
## The Question
 
> **Skin care products dominate listings on Shopee Thailand, but does that mean they dominate revenue too? And what actually drives a product's sales volume when price barely correlates with it?**
 
This project is an end-to-end analysis of 68,499 raw Health & Wellness product listings from Shopee Thailand: deduplicated, cleaned, and pushed through a full **Python EDA → Power BI → Machine Learning** pipeline to answer that question with numbers, not assumptions.
 
---
 
## Table of Contents
 
- [The Question](#the-question)
- [Tech Stack](#tech-stack)
- [Data Cleaning](#data-cleaning-what-got-fixed)
- [EDA: Key Business Questions](#eda-key-business-questions)
- [Power BI Dashboards](#power-bi-dashboards)
- [Machine Learning](#machine-learning)
- [Key Takeaways](#key-business-takeaways)
- [Repo Structure](#repo-structure)
- [How to Reproduce](#how-to-reproduce)
---
 
## Tech Stack
 
| Layer | Tools |
|-------|-------|
| **Data Cleaning & EDA** | Python (pandas, NumPy) |
| **Machine Learning** | scikit-learn (LinearRegression, RandomForestRegressor) |
| **Dashboard** | Power BI Desktop (2-page, interactive) |
| **Version Control** | Git, GitHub |
 
---
 
## Data Cleaning: What Got Fixed
 
Raw Shopee data is messy. Here's what was actually found and fixed, not assumed away:
 
- **65,877 duplicate rows (96%)** removed. The raw file had near-identical listings repeated dozens of times per product.
- **Price column** stored as Thai-formatted currency strings (`"฿9,999.00"`), stripped and converted to float using regex `r'[^\d.]'`.
- **Total Sold and Total Reviews** stored as Thai-language text (`"9,999+ ชิ้น"`, `"(7216)"`), parsed with regex, converted to numeric, missing values filled with 0 where absence of a sale or review is the correct interpretation.
- **Deliberate decision to keep** rows that share a Product ID but differ in price. These reflect price updates or promotional pricing over time, not duplicate noise. Documented explicitly rather than silently dropped.
- After cleaning: **2,622 unique product listings**, zero nulls, all columns correctly typed.
---
 
## EDA: Key Business Questions
 
Every analysis here answers a real question, not just "here's a bar chart."
 
**1. Which section actually drives the most revenue?**
 
Skin Nourishment has the most listings (137) and the highest total revenue at **฿36.6M**, but not because it charges the most. It wins on volume: average 1,152 units sold per product at an average price of just ฿301. Protein products hold the #2 revenue spot with far fewer listings but a higher average price (฿952) and the strongest customer engagement of any section (Review Rate 4.34, more than 3x the next closest).
 
**2. Does price actually drive revenue?**
 
No. The correlation between Price and Revenue is **r = -0.05**, essentially zero. Revenue is driven almost entirely by sales volume. A ฿9 product can and does out-earn a ฿5,600 product. Pricing strategy alone is not a lever for revenue growth in this category.
 
**3. Is Bangkok the dominant market?**
 
Not by total revenue. Rural provinces collectively generate **฿184M vs Bangkok's ฿164M**, despite Bangkok having more listings. Pattani and Sakon Nakhon have the highest average units sold per shop in the entire dataset. The assumption that Bangkok equals the market does not hold.
 
**4. What price range dominates the category?**
 
**89% of all products are priced under ฿1,127.** Mid-Range (฿200 to ฿500) and Budget (under ฿200) each generate about ฿125M, essentially tied. This is a mass-market, volume-driven category, not a premium one.
 
---
 
## Power BI Dashboards
 
Two pages, every visual answers a decision, not decoration.
 
**Dashboard 1: Overview**
KPI cards (Total Products, Total Revenue, Average Price), revenue by section, price segment breakdown, and a geographic revenue map across Thailand, filterable by Section.
 
**Dashboard 2: Deep Dive**
Top 10 products and locations by revenue, Bangkok vs Rural comparison, price vs average units sold by section (scatter), revenue by price segment (treemap), and total reviews by section, filterable by Price Segment.
 
Static view: [`dashboard/dashboard_report.pdf`](dashboard/dashboard_report.pdf)
Interactive: [`dashboard/Project_1.pbix`](dashboard/Project_1.pbix), open in Power BI Desktop (free)
 
---
 
## Machine Learning
 
> A two-stage regression exercise, included because catching a problem with a model is a more honest skill signal than only showing clean results.
 
**The question:** can product features (price, section, location, review engagement) predict sales volume?
 
**Stage 1: Predicting Price**
 
R² = 0.13. Price is not predictable from sales volume, reviews, or category. It appears to be set by factors outside this dataset such as brand positioning, ingredient cost, and packaging. This is a real finding, not a failure.
 
**Stage 2: Predicting Total Sold**
 
Initial model: **R² = 0.978**, suspiciously high. Investigation revealed **data leakage**: `Total Reviews` is a near-direct byproduct of `Total Sold` (you can only review something you've already bought), so the model was using the answer to predict the answer.
 
After removing `Total Reviews`:
 
| Model | R² | Notes |
|-------|-----|-------|
| Linear Regression | 0.72 | Reasonable signal |
| Random Forest | **0.372** | Honest result after leakage fix |
 
**Feature Importance (post-fix):**
 
| Feature | Importance |
|---------|-----------|
| Review Rate | 44% |
| Price | 31% |
| Location Type | 4% |
| Section (all) | about 21% combined |
 
Review Rate has **1.5x the influence of Price** on sales volume. Engagement, getting buyers to leave reviews, is a more effective lever than discounting.
 
Full notebook: [`notebook/Lazada project.ipynb`](notebook/Lazada%20project.ipynb)
 
---
 
## Key Business Takeaways
 
```
1. Skin Nourishment wins on volume (฿36.6M), Protein wins on trust
   (Review Rate 4.34x average): two different paths to revenue.
 
2. Price has near-zero correlation with revenue (r = -0.05).
   Cutting prices is unlikely to move the needle.
 
3. Rural provinces out-earn Bangkok in total revenue (฿184M vs ฿164M),
   an underserved market hiding in plain sight.
 
4. 89% of the market is priced under ฿1,127.
   This is a mass-market, volume-driven category, not a premium one.
 
5. Review Rate (44%) drives sales more than Price (31%).
   The highest-leverage action for a seller is generating more reviews,
   not adjusting price.
```
 
---
 
## Repo Structure
 
```
shopee-health-wellness-analysis/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── health_wellness_cleaned.csv     # cleaned dataset (2,622 rows)
├── notebook/
│   └── Lazada project.ipynb            # cleaning + EDA + ML
└── dashboard/
    ├── Project_1.pbix                  # Power BI interactive file
    └── dashboard_report.pdf            # static export of both dashboards
```
 
---
 
## How to Reproduce
 
```bash
# 1. Clone
git clone https://github.com/vinnythepoo1/shopee-health-wellness-analysis.git
cd shopee-health-wellness-analysis
 
# 2. Install dependencies
pip install -r requirements.txt
 
# 3. Run the notebook
jupyter notebook "notebook/Lazada project.ipynb"
 
# 4. View dashboards
# Quick look: open dashboard/dashboard_report.pdf
# Interactive: open dashboard/Project_1.pbix in Power BI Desktop (free)
```
 
**Dataset:** Shopee Thailand, Health & Wellness category (August 2023)
 
---
 
<div align="center">
<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=400&size=14&pause=1500&color=8FA6B2&center=true&vCenter=true&width=500&lines=Thanks+for+reading+-+questions+welcome!" alt="footer" />
</div>
