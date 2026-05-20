# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner 

This is a Retail Sales Analysis SQL project using a dataset called SQL - Retail Sales Analysis_utf. The project walks through the full data analysis pipeline: database creation, table setup, data cleaning, exploration, and business problem solving with SQL queries.

The dataset captures transactional retail data with fields like transactions_id, sale_date, sale_time, customer_id, gender, age, category, quantiy, price_per_unit, cogs, and total_sale.

## Objectives

The main goals of this project are:

Data Preparation: Create a structured database and clean the raw sales data by handling NULL values
Data Exploration: Understand key metrics like total sales, unique customers, and product categories
Business Insights: Answer 10 specific business questions to derive actionable insights about sales performance, customer behavior, and trends
Skill Demonstration: Showcase proficiency in SQL including DDL, DML, aggregations, window functions, CTEs, and date/time functions.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `monikadas_sql`.
- **Table Creation**: A table named `retail_sales1` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE monikadas_sql;

Create table retail_sales1
           (
             transactions_id INT PRIMARY KEY,
             sale_date DATE,
             sale_time TIME,
             customer_id INT,
             gender VARCHAR(10),
             age INT,
             category VARCHAR(20),
             quantiy INT,
             price_per_unit FLOAT,
             cogs FLOAT,
             total_sale FLOAT
            );
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.


SELECT * FROM retail_sales1
SELECT * FROM retail_sales1
WHERE transactions_id IS NULL

SELECT * FROM retail_sales1
WHERE sale_date IS NULL

SELECT * FROM retail_sales1
WHERE sale_time IS NULL

SELECT * FROM retail_sales1
WHERE 
    transactions_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantiy IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;

	DELETE FROM retail_sales1
WHERE 
    transactions_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantiy IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;

    
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05

 SELECT *
FROM retail_sales1
WHERE sale_date = '2022-11-05';

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022

SELECT 
  *
FROM retail_sales1
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantiy >= 4

-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.

SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales1
GROUP BY 1

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales1
WHERE category = 'Beauty'

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.

SELECT * FROM retail_sales1
WHERE total_sale > 1000

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.

SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales1
GROUP 
    BY 
    category,
    gender
ORDER BY 1

-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year

SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales1
GROUP BY 1, 2
) as t1
WHERE rank = 1
    
-- ORDER BY 1, 3 DESC

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 

SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales1
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.


SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales1
GROUP BY category



-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)

WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales1
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift


```

## Findings

Based on the 10 business queries written, here are the types of insights this project extracts:

Date-specific Performance: Query Q1 filters all sales for 2022-11-05 to analyze single-day performance
Category + Time Filtering: Q2 finds high-quantity Clothing sales in Nov 2022 with quantiy >= 4
Category Revenue: Q3 calculates SUM(total_sale) and order count per category to identify top revenue drivers
Customer Demographics: Q4 finds AVG(age) of Beauty category customers for targeted marketing
High-Value Transactions: Q5 flags all transactions where total_sale > 1000 for VIP analysis
Gender-Category Trends: Q6 counts transactions by gender in each category to spot buying patterns
Seasonal Trends: Q7 uses EXTRACT() + RANK() window function to find the best selling month each year
Top Customers: Q8 ranks top 5 customers by SUM(total_sale) for loyalty programs
Category Reach: Q9 counts DISTINCT customer_id per category to measure customer base breadth
Time-of-Day Analysis: Q10 uses a CASE statement + CTE to group sales into Morning <12, Afternoon 12-17, Evening >17 shifts and count orders

## Conclusion

This project demonstrates end-to-end SQL data analysis for retail. Starting from raw data, it establishes a clean, queryable dataset and uses it to answer practical business questions.

Technical wins: You’ve covered data cleaning, aggregations, joins-free analysis, date/time extraction, window functions with RANK(), and CTEs for shift analysis.

Business value: The queries directly support decisions around inventory planning, customer segmentation, marketing timing, and identifying high-value customers. The structure makes it easy to scale — just swap in new data and re-run the analysis.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Zero Analyst

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

### Stay Updated and Join the Community

For more content on SQL, data analysis, and other data-related topics, make sure to follow me on social media and join our community:

- **YouTube**: [Subscribe to my channel for tutorials and insights](https://www.youtube.com/@zero_analyst)
- **Instagram**: [Follow me for daily tips and updates](https://www.instagram.com/zero_analyst/)
- **LinkedIn**: [Connect with me professionally](https://www.linkedin.com/in/najirr)
- **Discord**: [Join our community to learn and grow together](https://discord.gg/36h5f2Z5PK)

Thank you for your support, and I look forward to connecting with you!
