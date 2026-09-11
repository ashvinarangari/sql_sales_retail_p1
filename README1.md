# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
 **Database**: `sql_project_p1'

This project demonstrates my SQL skills and techniques commonly used by data analysts to explore, clean, and analyze retail sales data.
The project involves setting up a retail sales database, performing Exploratory Data Analysis (EDA), cleaning and transforming data, and answering important business questions using SQL queries.Through this project, I have applied SQL concepts such as data cleaning, filtering, aggregation, GROUP BY, JOINs, subqueries, CTEs, window functions, and analytical queries to extract meaningful insights from retail sales data.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. **Database Setup**

- **Database Creation**:
The project begins by creating a database named sql_project_p1.

- **Table Creation**:
 The table named retail_sales is created to store the retail sales data.

- **Table Structure**:

**The table contains the following columns**:
- Transaction ID
- Sale Date
- Sale Time
- Customer ID
- Gender
- Age
- Product Category
- Quantity Sold
- Price per Unit
- Cost of Goods Sold (COGS)
- Total Sale Amount

```sql
CREATE DATABASE sql_project_p1;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
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

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT * FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
select
category, sum(total_sale) as net_sale,
count(*) as total_orders from retail_sales
group by category;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
select
gender,category, count(*) as total_id from retail_sales
group by gender,category;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
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
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
select customer_id,
sum(total_sale) as sale from retail_sales
group by customer_id 
order by sale desc limit 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
select category,
count(distinct(customer_id))from retail_sales
group by category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
with hourly_sale
as (
select *,
case
when extract(hour from sale_time)<=12 then 'Morning'
when extract(hour from sale_time) between 12 and 17 then 'Afternoon'
else 'Evening'
end as shift from retail_sales)
select shift,
count(*) as total_order 
from hourly_sale
group by shift;

```
## Key Insights

- **Customer Demographics:** The dataset includes customers from various age groups, with sales distributed across different product categories, such as Clothing and Beauty.
- **High-Value Transactions:** Several transactions recorded a total sale amount greater than 1,000, indicating high-value or premium purchases.
- **Sales Trends:** Monthly sales analysis reveals variations in sales performance, helping identify peak sales periods and seasonal trends.
- **Customer Insights:** The analysis identifies the highest-spending customers and the most popular product categories, providing valuable insights into customer purchasing behavior.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project provides a practical introduction to SQL for data analysis, covering database setup, data cleaning, exploratory data analysis (EDA), and business-focused SQL queries.
Through this project, I developed hands-on experience in analyzing retail sales data and extracting meaningful insights related to sales trends, customer behavior, and product performance. The insights generated from the analysis can support data-driven business decisions and help organizations better understand their sales and customer patterns.

## SQL Skills & Concepts Used

* Database and table creation
* Data cleaning and NULL value handling
* SELECT, WHERE, ORDER BY
* GROUP BY and HAVING
* Aggregate functions: SUM(), COUNT(), AVG(), MIN(), MAX()
* DISTINCT
* CASE statements
* INNER JOIN and other JOIN operations
* Subqueries
* Common Table Expressions (CTEs)
* Window functions
* Date and time functions
* Data aggregation and filtering
* Business-oriented data analysis

## Tools & Technologies
* **SQL**
* **postgreySQL**
* **GitHub**
* **Excel** — for supporting data preparation and analysis.

## Project Files

* `sql_sales_retail_p1.sql` — SQL script containing database creation, table creation, data cleaning, exploratory analysis, and business queries.
* `retail_sales_analysis_utf.csv` — Retail sales dataset used for the analysis.
* `README1.md` — Project documentation and analysis summary.

## Author
**Ashvina Rangari**
Aspiring Data Analyst | SQL | Excel | Data Analysis

This project was created to demonstrate my practical SQL and data analysis skills through a real-world retail sales analysis project.

## Connect With Me

* **LinkedIn:** [Ashvina Rangari](https://www.linkedin.com/in/ashvina-rangari-b7822818b/)
* **GitHub:** [Ashvina Rangari](https://github.com/ashvinarangari)
* **Email:** [patilashvina24@gmail.com](mailto:patilashvina24@gmail.com)

Feel free to connect with me for opportunities, collaboration, or feedback on this project.







