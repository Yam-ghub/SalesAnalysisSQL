# Retail Sales Analysis SQL & Excel Project

## Project Overview
**Project Title**: Retail Sales Analysis  
**Database**: `sql_project_p2`


This project is designed to showcase SQL skills and techniques commonly used by data analysts to explore, clean, and analyze retail sales data. It involves setting up a retail sales database using PostgreSQL and managing it through pgAdmin 4. The project includes performing exploratory data analysis (EDA) and answering key business questions through well-structured SQL queries. It’s ideal for those starting their data analysis journey and looking to build a strong foundation in SQL using industry-standard tools.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE p1_retail_db;

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
SELECT * FROM retail_sales
LIMIT 10;

SELECT 
	COUNT(*)
FROM retail_sales
LIMIT 10;

-- Data Cleaning
SELECT * FROM retail_sales
WHERE transactions_id IS NULL 
	OR sale_date IS NULL
	OR sale_time IS NULL
	OR customer_id IS NULL
	OR gender IS NULL
	OR category IS NULL
	OR price_per_unit IS NULL
	OR cogs IS NULL
	OR total_sale IS NULL;

--
DELETE FROM retail_sales
WHERE transactions_id IS NULL 
	OR sale_date IS NULL
	OR sale_time IS NULL
	OR customer_id IS NULL
	OR gender IS NULL
	OR category IS NULL
	OR price_per_unit IS NULL
	OR cogs IS NULL
	OR total_sale IS NULL;

-- Data Exploration
-- How many sales we have?
SELECT COUNT(*) as total_sale FROM retail_sales;

-- How many customers we have?
SELECT COUNT(DISTINCT customer_id) as total_sale FROM retail_sales;

-- What are the different categories?
SELECT DISTINCT category FROM retail_sales;
```
### 3. Data Analysis

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT category, quantity, transactions_id
FROM retail_sales
WHERE 
  category = 'Clothing'
  AND quantity >= 4
  AND to_char(sale_date, 'YYYY-MM') = '2022-11'
ORDER BY transactions_id
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
	category, 
	SUM(total_sale) AS total_sale,
	COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category; 
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT category, ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty'
GROUP BY category;
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT transactions_id, total_sale
FROM retail_sales
WHERE total_sale > 1000;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT COUNT(transactions_id) AS no_transactions, gender, category
FROM retail_sales 
GROUP BY gender, category
ORDER BY category;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT * FROM
(SELECT 
	EXTRACT(YEAR FROM sale_date) AS year,
	EXTRACT(MONTH FROM sale_date) AS month,
	AVG(total_sale) AS avg_sale,
	RANK () OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC)
FROM retail_sales
GROUP BY year, month
) as t1
WHERE rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT DISTINCT customer_id, SUM(total_sale) AS t_sale
FROM retail_sales 
GROUP BY customer_id
ORDER BY t_sale DESC
LIMIT 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
	category,
	COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH day_period
AS
(SELECT *,
	CASE 
		WHEN EXTRACT(HOUR FROM sale_time) <=12 THEN 'Morning'
		WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
		ELSE 'Evening'
	END as shift
FROM retail_sales
)
SELECT shift, COUNT(*) as shift_count
FROM day_period
GROUP BY 1
ORDER BY 2
```

## Findings

High-Value Sales: Multiple transactions had total sales over $1,000, indicating premium products or bulk purchases.

Clothing Category Surge: In November 2022, the Clothing category had several transactions with quantity ≥ 4, suggesting strong seasonal demand.

Top Revenue Categories: Some categories generated high total sales, while others had more frequent purchases, showing differences in price vs. volume.

Best-Selling Months: Monthly average sales revealed specific peak months each year, highlighting seasonal shopping trends.

Sales by Time of Day: Most transactions occurred during Morning and Afternoon shifts, suggesting optimal times for promotions and staffing.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Create a Repository in Github**
2.  **Clone the Repository from github to local**
3. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
4. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
5. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. 



