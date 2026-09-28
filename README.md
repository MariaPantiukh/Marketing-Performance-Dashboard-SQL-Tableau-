# Marketing Performance Dashboard (SQL & Tableau)

## 📌 Project Overview
This end-to-end data analytics project simulates a real-world task for a Marketing Data Analyst. The goal is to extract, clean, and combine multi-source marketing data (Facebook Ads and Google Ads) using **PostgreSQL**, export the unified analytical dataset, and build an interactive **Marketing Performance Dashboard** in **Tableau Public**.

The dashboard helps stakeholders evaluate campaign performance, tracking key marketing metrics (CTR, CPC, CPL, ROMI, Conversions) and analyzing the correlation between marketing expenditure and lead generation.

---

## 🛠️ Tech Stack & Tools
* **Database Management:** PostgreSQL, DBeaver
* **SQL Techniques:** Data aggregation (`SUM`, `COALESCE`), String Decoding/Manipulation, CTEs / Subqueries, `UNION ALL`, Table Joins
* **Visualization:** Tableau Public (LOD Expressions, Parameters, Calculated Fields, Dual Axis Charts, Dashboard Actions)
* **Documentation:** Markdown

---

## 💾 Data Architecture & SQL Pipeline

The source data is stored across four main tables in PostgreSQL:
1. `facebook_ads_basic_daily` (Fact table for FB Ads)
2. `facebook_campaign` (Dimension lookup for FB Campaign names)
3. `facebook_adset` (Dimension lookup for FB Ad Set/Audience names)
4. `google_ads_basic_daily` (Fact table for Google Ads with built-in names)

### Key SQL Processing Steps:
* **Lookup Joins:** Joined FB Fact table with FB Campaign and Adset dimension tables using `campaign_id` and `adset_id` to retrieve clear business names (`campaign_name`, `adset_name`).
* **Source Tagging:** Added a constant field `source` ('Facebook' vs 'Google') to identify channels.
* **Combining Sources:** Used `UNION ALL` to unify Facebook and Google performance data into a single analytical stream.
* **URL Decoding:** Decoded encoded URL parameters to clean up `utm_campaign` values.
* **Handling Nulls:** Handled missing/null metric values using the `COALESCE` function.
* **Aggregations:** Calculated foundational totals for Spend, Clicks, Impressions, Reach, Leads, and Value.


---

## 📊 Tableau Dashboard Features & Metrics

### 1. Calculated Fields & LOD Expressions
* **CTR:** `SUM([Clicks]) / SUM([Impressions])`
* **CPC:** `SUM([Spend]) / SUM([Clicks])`
* **CPM:** `(SUM([Spend]) / SUM([Impressions])) * 1000`
* **CPL:** `SUM([Spend]) / SUM([Leads])`
* **ROMI:** `SUM([Value]) / SUM([Spend])`
* **Clicks to Leads Conv.:** `SUM([Leads]) / SUM([Clicks])`
* **Reach to Leads Conv.:** `SUM([Leads]) / SUM([Reach])`
* **Monthly Leads (Fixed LOD):** `{ FIXED [Month]: SUM([Leads]) }`
* **Monthly Spend (Fixed LOD):** `{ FIXED [Month]: SUM([Spend]) }`
* **Budget-Lead Correlation (`corr_leads_spend`):** Correlation coefficient measuring the relationship between monthly spend and generated leads.

### 2. Interactive Features
* **Dynamic Metric Switching:** Implemented a Parameter (`Select Metric`) paired with a `Selected Metric` calculated field to allow users to toggle between CTR, CPC, CPL, ROMI, and Conversions dynamically across visuals.
* **Spend vs. Leads Analysis:** Dual-axis chart showcasing monthly expenditure alongside leads generated, complete with a floating correlation metric tile.
* **Scatter Plot (Campaign Efficiency):** Evaluates campaign ROI/efficiency vs. Spend, with color-coding by platform (`source`) and node size reflecting total lead volume.
* **Filter Action:** Dynamic filtering that zooms into specific campaign performance across trends upon selecting a data point in the Scatter Plot.
  
<img width="1839" height="1349" alt="Dashboard 2" src="https://github.com/user-attachments/assets/23ac1e90-2255-4a74-9609-729151aabf59" />

---

## 🔗 Live Interactive Dashboard

📁 **View the Interactive Dashboard on Tableau Public:** 👉 [(https://public.tableau.com/shared/PG958KKY7?:display_count=n&:origin=viz_share_link)]

---
## Executive Summary & Campaign Performance Insights

### 1. Key Performance Indicators (KPIs)
* **Overall ROMI (122%):** Indicates positive overall marketing profitability across the analyzed period.
* **Financial Return:** Advertising investments fully covered the spend and yielded a 122% total return on ad spend (+22% net profit margin).

---

### 2. Analytical Findings
* **Channel Performance (Google Ads vs. Facebook Ads):**
  * **Google Ads** demonstrated superior cost-efficiency, maintaining a consistently lower Cost Per Lead (CPL) and Cost Per Click (CPC).
  * **Facebook Ads** proved to be more expensive overall, with the **Lookalike Expansion** campaign recording the highest CPL among all initiatives.
* **Campaign Highs & Lows:**
  * **Discounts Campaign:** Generated the highest Click-Through Rate (CTR) despite receiving the lowest budget allocation across both Google and Facebook.
  * **Year-over-Year (YoY) Trend:** In 2022, overall ad spend increased compared to 2021, while total lead volume decreased, indicating a drop in acquisition efficiency.
* **Summary:**
  Despite elevated CPL metrics in specific channels (notably Facebook Ads and *Lookalike Expansion*), overall business outcome remains profitable due to high-performing drivers like the *Discounts* campaign.

---

### 3. Strategic Recommendations
1. **Reallocate Ad Spend:** Shift budget away from high-cost, low-yield campaigns (*Lookalike Expansion*) toward high-converting segments (*Discounts* and core Google Ads setups).
2. **Optimize Acquisition Costs:** Refine audience targeting to reduce overall CPL and scale high-performing ad sets.
3. **Target ROI Growth:** Implementing budget re-optimization aims to elevate overall **ROMI from 122% to 150%+**.
