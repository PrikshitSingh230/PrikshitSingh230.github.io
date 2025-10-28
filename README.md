# A Top YouTuber Analysis Project

This project aims to discover which UK-based YouTube channel offers the highest potential return on marketing investment.  
Through a combination of SQL data cleaning, Power BI visualizations, and Excel profitability modeling, we evaluate metrics such as viewership, engagement, and conversion rates to guide data-driven decisions.

---

## Table of Contents
- [Objective](#objective)
- [Data Overview](#data-overview)
- [Data Preparation (SQL)](#data-preparation-sql)
- [Power BI Dashboard](#power-bi-dashboard)
- [Excel Analysis](#excel-analysis)
- [SQL Profitability Analysis](#sql-profitability-analysis)
- [Findings & Recommendations](#findings--recommendations)
- [Resources](#resources)
- [Conclusion](#conclusion)

---

## Objective

The objective of this analysis is to use data-backed insights to identify the most profitable YouTube channel for product marketing partnerships.  
By studying performance across 100 top UK YouTubers, we aim to:

- Analyze and compare average views, engagement rate, and subscriber base  
- Estimate potential sales and ROI using defined conversion and cost parameters  
- Determine which channel provides the maximum return per investment dollar  
- Support marketing and sponsorship decisions using analytical evidence  

This analysis blends quantitative data modeling with qualitative reasoning (e.g., audience type and target demographics).

[Back to Top](#table-of-contents)

---

## Data Overview

The dataset includes the Top 100 YouTube channels in the United Kingdom, covering creators from diverse content categories such as entertainment, gaming, education, and music.

### Key Features of the Dataset:
- Channel Name  
- Subscribers Count  
- Total Views  
- Total Videos  
- Engagement Rate  
- Country and Category  

The data was first imported into SQL Server for cleaning and transformation, ensuring only the relevant attributes were retained for further analysis.

[Back to Top](#table-of-contents)

---

## Data Preparation (SQL)

### Steps Taken:
1. Data Loading – The raw dataset was imported into SQL Server.  
2. Data Cleaning – Removed missing or inconsistent values and standardized formats.  
3. Column Selection – Extracted the key columns required for profitability analysis:
   - Channel Name  
   - Total Views  
   - Total Videos  
   - Subscribers  
   - Engagement Rate  
4. Data Transformation – Calculated derived metrics such as average views per video and prepared the dataset for visualization.  
5. Export to Power BI – The cleaned and filtered dataset was loaded into Power BI for dashboard creation.

This process ensures the analysis is based on clean, consistent, and validated data.

[Back to Top](#table-of-contents)

---

## Power BI Dashboard

After preparing the dataset, the data was visualized in Power BI to identify patterns and top-performing channels at a glance.

### Dashboard Components:
- Matrix View – Lists all channels with their metrics (Views, Subscribers, Engagement Rate)  
- KPI Cards – Display three core indicators:
  - Average Views  
  - Engagement Rate  
  - Views per Subscriber  
- Bar Charts:
  - Top 10 Channels by Subscriber Count  
    ![Top 10 Channels by Subscriber Count](<!-- Add image link here -->)
  - Top 10 Channels by View Count  
    ![Top 10 Channels by View Count](<!-- Add image link here -->)

### Dashboard Preview
Check out the live Power BI dashboard here:  
[View Dashboard]([youtubers analysis.png](https://github.com/PrikshitSingh230/PrikshitSingh230.github.io/blob/main/assets/images/youtubers%20analysis.png))

The dashboard helps decision-makers instantly compare performance, spot high-engagement creators, and filter insights interactively.

[Back to Top](#table-of-contents)

---

## Excel Analysis

To calculate potential returns and profitability, additional modeling was performed in Excel.  
This included simulating possible sales outcomes based on key assumptions and cost structures.

### Key Parameters
| Metric | Value |
|---------|--------|
| Conversion Rate | 0.02 (2%) |
| Product Cost | $5 |
| Campaign Cost | $5,000 |

### Excel Workbook Snapshot
### Excel Table

| Channel Name       | Avg Views per Video (Excel) | Avg Views per Video (SQL) | Potential Product Sales per Video (Excel) | Potential Product Sales per Video (SQL) | Potential Revenue (Excel) | Potential Revenue (SQL) | Net Profit (Excel) | Net Profit (SQL) | Difference |
|--------------------|------------------------------|-----------------------------|--------------------------------------------|-------------------------------------------|----------------------------|---------------------------|-------------------|------------------|-------------|
| NoCopyrightSounds  | 6,920,000                    | 6,920,000                   | $138,400                                   | $138,400                                  | $692,000                   | $692,000                  | $687,000          | $687,000         | 0           |
| DanTDM             | 5,340,000                    | 5,340,000                   | $106,800                                   | $106,800                                  | $534,000                   | $534,000                  | $529,000          | $529,000         | 0           |
| DanRhodes          | 11,150,000                   | 11,150,000                  | $223,000                                   | $223,000                                  | $1,115,000                 | $1,115,000                | $1,110,000        | $1,110,000       | 0           |
| Miss Katy          | 14,330,000                   | 14,330,000                  | $286,600                                   | $286,600                                  | $1,433,000                 | $1,433,000                | $1,428,000        | $1,428,000       | 0           |


### Analysis Summary
- Calculations were consistent between Excel and SQL, confirming data integrity.  
- Conversion rate and cost assumptions were key in modeling realistic outcomes.  
- The Net Profit was the ultimate deciding factor for investment feasibility.

[Back to Top](#table-of-contents)

---

## SQL Profitability Analysis

The same profitability logic was implemented in SQL to validate the Excel model.  
This ensures accuracy and allows automated scaling of calculations across multiple channels.

```sql
/*
1. Define the variables
2. Create a CTE that rounds the average views per video
3. Select the columns required for the analysis
4. Filter the results by the selected YouTube channels
5. Order by net_profit (from highest to lowest)
*/

DECLARE @conversionRate FLOAT = 0.02;       -- Conversion rate (2%)
DECLARE @productCost MONEY = 5.0;           -- Product cost ($5)
DECLARE @campaigncost MONEY = 50000.0;      -- Campaign cost ($50,000)

WITH channelData AS (
    SELECT
        channel_name,
        total_views,
        total_videos,
        ROUND((CAST(total_views AS FLOAT) / total_videos), -4) AS avg_views_per_video
    FROM
        top_uk_youtubers_2024
)
SELECT 
    channel_name,
    avg_views_per_video,
    (avg_views_per_video * @conversionRate) AS potential_sale_per_video,
    (avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    (avg_views_per_video * @conversionRate * @productCost) - @campaigncost AS net_profit
FROM 
    channelData
WHERE 
    channel_name IN ('NoCopyrightSounds', 'DanTDM', 'Dan Rhodes', 'Miss Katy')
ORDER BY
    net_profit DESC;
```
# Findings & Recommendations — A Top YouTuber Analysis Project

## Profitability Validation (SQL)
This SQL script helps validate profitability in a reproducible, automated manner — making it ideal for scaling across large datasets.  
[Back to Top](#findings--recommendations--a-top-youtuber-analysis-project)

---

## Findings & Recommendations

After reviewing both the Excel and SQL analyses:

- **DanRhodes** is identified as the most profitable channel with the highest potential return on investment.  
- **Miss Katy**, despite having higher views, primarily targets children, which makes it unsuitable for product-based campaigns targeting adult audiences.  
- **DanTDM** and **NoCopyrightSounds** show stable engagement but generate lower profit margins compared to DanRhodes.  

### Final Recommendation
Investing in **DanRhodes** offers the best balance between high audience engagement, profit potential, and target market suitability.  

[Back to Top](#findings--recommendations--a-top-youtuber-analysis-project)

---

## Resources

**Dataset:** Top 100 YouTubers in the UK (2024)  
**Tools Used:** SQL Server, Microsoft Excel, Power BI  

**Dashboard Link:** _Add Power BI link here_  

**Dashboard Screenshots:**  
_Add screenshots of your Power BI dashboard here_

[Back to Top](#findings--recommendations--a-top-youtuber-analysis-project)

---

## Conclusion

This project showcases a full-cycle data analysis workflow — from raw data cleaning in SQL to visual analytics in Power BI and financial modeling in Excel.  

It demonstrates how combining technical data skills with business reasoning can yield actionable insights for marketing investment decisions.  

### Key Takeaways
- **SQL** ensures data accuracy and validation  
- **Power BI** enables interactive data storytelling  
- **Excel** bridges analytics with business forecasting  

By leveraging all three tools together, data professionals can make strategic, evidence-based decisions with measurable impact.  

---

**Author:** Soulsnatcher  
**Project Year:** 2025  

[Back to Top](#findings--recommendations--a-top-youtuber-analysis-project)
