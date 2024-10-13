AtliQ Mart’s Diwali & Sankranti Promotions!
This repository contains the SQL scripts used to analyze the performance of promotional campaigns run by AtliQ Mart during Diwali 2023 and Sankranti 2024. The project addresses various business requests related to identifying high-value discounted products, store distribution, campaign effectiveness, and product performance in terms of incremental sales and revenue.

Introduction
Promotional campaigns play a crucial role in the retail industry, driving sales and attracting customers during festive seasons. This project aims to analyze the performance of promotional campaigns conducted by AtliQ Mart during Diwali 2023 and Sankranti 2024. By leveraging data analytics, we seek to gain insights into the effectiveness of these campaigns and provide recommendations for optimizing future marketing strategies.

Data Sources
The analysis is based on data obtained from AtliQ Mart's internal databases. The main datasets used include fact_events, dim_products, dim_stores, and sales_summary. These datasets contain information about product sales, store locations, promotional events, and campaign revenues.

Project Overview:
Analyzed data from AtliQ Mart's internal databases.
Performed SQL queries to fulfill five business requests.
Insights are intended to inform future promotional strategies and resource allocation.
Business Requests
1. High-Value Products in 'BOGOF' Promotion
Objective: Identify high-value products featured in the 'BOGOF' (Buy One Get One Free) promotion.

SELECT
    DISTINCT p.product_name,
    f.base_price
FROM
    fact_events f
JOIN
    dim_products p ON f.product_code = p.product_code
WHERE
    f.promo_type = 'BOGOF' AND f.base_price > 500;
2. Store Presence Overview
Objective: Provide an overview of the number of stores in each city.

SELECT
    City,
    COUNT(store_id) as Total_Stores
FROM
    dim_stores
GROUP BY
    City
ORDER BY
    Total_Stores DESC;
3. Promotional Campaign Revenue Analysis
Objective: Display total revenue generated before and after each promotional campaign.

SELECT
	campaign_name,
	CONCAT(ROUND(SUM(revenue_before) / 1000000, 2), 'M') AS Revenue_Before,
    CONCAT(ROUND(SUM(revenue) / 1000000, 2), 'M') AS Revenue_After
From
	sales_summary
GROUP BY
	campaign_name;
4. Incremental Sold Quantity Analysis during Diwali Campaign
Objective: Calculate Incremental Sold Quantity (ISU%) for each category during the Diwali campaign.

SELECT
    category,
    `ISU%`,
    RANK() OVER (ORDER BY `ISU%` DESC) AS Ranking
FROM
    (
        SELECT
            category,
            SUM(ISU) as ISU,
            ROUND(SUM(ISU) / SUM(quantity_before_promo) * 100,2) as `ISU%`
        FROM
            sales_summary
        WHERE
            CAMPAIGN_NAME = "Diwali"
        GROUP BY 
            category
    ) AS subquery;
