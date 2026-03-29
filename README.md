# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: 'Retail_Sales'

This project helps showcase basic SQL skills used by data analysts to work with retail sales data. It focuses on setting up a sales database, exploring and cleaning data, and answering business questions using SQL queries.
It’s a beginner‑friendly project, ideal for anyone starting in data analysis and looking to build a strong foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named 'Retail_Sales'.
- **Table Creation**: A table named 'Retail_Sales_Project' is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

---sql
CREATE DATABASE Retail_Sales;

Create Table Retails_Sales_data_dummy
(transactions_id INTEGER,
sale_date DATE,
sale_time TIME,
customer_id INTEGER,
gender VARCHAR(20),
age Integer,
category VARCHAR(50),
quantiy integer, 
price_per_unit FLOAT,
cogs FLOAT,
total_sale FLOAT
);
---

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

---sql
--Select * from Retail_Sales_Project rsp 
where customer_id is NULL 

-----
--Select * from Retail_Sales_Project rsp 
WHERE cogs is NULL 


--SELECT *
FROM Retail_Sales_Project rsp
WHERE 
    rsp.transactions_id IS blank
    OR rsp.sale_date IS NULL
    OR rsp.sale_time IS NULL
    OR rsp.customer_id IS NULL
    OR rsp.gender IS NULL
    OR rsp.age IS NULL
    OR rsp.category IS NULL
    OR rsp.quantity IS NULL  
    OR rsp.price_per_unit IS NULL
    OR rsp.cogs IS NULL
    OR rsp.total_sale IS NULL;


--SELECT *
FROM Retail_Sales_Project rsp
WHERE 
    rsp.transactions_id IS NULL OR rsp.transactions_id = ''
    OR rsp.sale_date IS NULL OR rsp.sale_date = ''
    OR rsp.sale_time IS NULL OR rsp.sale_time = ''
    OR rsp.customer_id IS NULL OR rsp.customer_id = ''
    OR rsp.gender IS NULL OR rsp.gender = ''
    OR rsp.age IS NULL OR rsp.age = ''
    OR rsp.category IS NULL OR rsp.category = ''
    OR rsp.quantity IS NULL OR rsp.quantity = ''
    OR rsp.price_per_unit IS NULL OR rsp.price_per_unit = ''
    OR rsp.cogs IS NULL OR rsp.cogs = ''
    OR rsp.total_sale IS NULL OR rsp.total_sale = ''

  --select * from Retail_Sales_Project rsp 
  where
  rsp.quantity ='';


--DELETE from Retail_Sales_Project
  where rsp.quantity = '';

--SELECT COUNT(*) from Retail_Sales_Project rsp 

--SELECT Count(rsp.age )
FROM Retail_Sales_Project rsp

### 3. --Data Analysis and Business Key Problems & answers


The following SQL queries were developed to answer specific business questions:

--Q.1. Write a SQL query to retrieve all columns for sales made on '2022-11-05:
--Q.2. Write a SQL query to retrieve sales for the month of December 2022: 
--Q.3. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:
--Q.4. Write a SQL query to calculate the total sales (total_sale) for each category.:
--Q.5. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
--Q.6. Write a SQL query to find all transactions where the total_sale is greater than 1000.:
--Q.7. Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
--Q.8. Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
--Q.9. **Write a SQL query to find the top 5 customers based on the highest total sales **:
--Q.10. Write a SQL query to find the number of unique customers who purchased items from each category.:
--Q.11. Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):
 --------------------------------------------------------------------------------------
--Q.1.	Write a SQL query to retrieve all columns for sales made on '2022-11-05:
     SELECT * From Retail_Sales_Project rsp 
      Where rsp.sale_date ='2022-11-05'
      
--Q.2. Write a SQL query to retrieve sales for the month of December 2022: 
      
Select * from Retail_Sales_Project rsp 
where rsp.sale_date BETWEEN '2022-12-01' and '2022-12-31';

--Q.3. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022
 SELECT * 
 from Retail_Sales_Project rsp 
 Where 
 category ='Clothing' 
 AND
 STRFTIME('%Y-%m',rsp.sale_date) = '2022-11'
 AND rsp.quantity >= 4
 
 --Q.4. Write a SQL query to calculate the total sales (total_sale) for each category.
Select rsp.category,
COUNT(quantity) as Total_quantity,
sum(rsp.total_sale) as Total_Sales
from Retail_Sales_Project rsp 
Group BY 1


 --Q.5. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
SELECT rsp.category, 
Round(AVG(rsp.age),1) AS Avg_age
from Retail_Sales_Project rsp 
WHERE rsp.category ='Beauty' 

--Q.6. Write a SQL query to find all transactions where the total_sale is greater than 1000.:

SELECT * from Retail_Sales_Project rsp 
WHERE rsp.total_sale >1000

--Q.7. Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.

SELECT rsp.category, rsp.gender,
Count(rsp.transactions_id) AS Total_transaction
from Retail_Sales_Project rsp 
Group BY rsp.category ,rsp.gender 
Order BY 1

--Q.8. Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:

Select * FROM 
(
WITH sale_date AS (
    SELECT
        STRFTIME('%Y', rsp.sale_date) AS Year,
        STRFTIME('%m', rsp.sale_date) AS Month,
        AVG(rsp.total_sale) AS Avg_Sale
    FROM Retail_Sales_Project rsp
    GROUP BY 1,2
)
SELECT
    Year,
    Month,
    Avg_Sale,
    RANK() OVER (
        PARTITION BY Year
        ORDER BY Avg_Sale DESC
    ) AS Sales_Rank
FROM sale_date
ORDER BY Year, Sales_Rank
) as T1
Where Sales_Rank = 1;



--Q.9. **Write a SQL query to find the top 5 customers based on the highest total sales **:

SELECT customer_id,
Sum(total_sale) AS Sale
from Retail_Sales_Project rsp 
Group BY 1
Order BY 2 DESC
LIMIT 5;


--Q.10. Write a SQL query to find the number of unique customers who purchased items from each category.:

 Select 
 category,
 COUNT(Distinct rsp.customer_id) as Unique_customer
 from Retail_Sales_Project rsp
 Group BY category	
 
--Q.11. Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):
 
 WITH Hourly_Sale
 as (
 Select *,
 CASE
 	WHEN CAST(strftime('%H', sale_time) as INTEGER) <12 THEN 'Morning'
 	When CAST(strftime('%H', sale_time) as INTEGER) Between 12 AND 17 THEN 'Aftenoon'
 	ELSE 'Evening'
 	END AS Shift_time
 from Retail_Sales_Project rsp
 )
 Select 
 Shift_time,
 Count(*) as Total_order
 From Hourly_Sale
 Group BY Shift_time 
 
 -------------------------------------Project_END----------------------------------------------------------

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project gives a clear and practical introduction to SQL for data analysts. It covers everything from setting up a database and cleaning data to exploring data and writing SQL queries to answer real business questions.
The insights from this work help businesses understand sales trends, customer behavior, and product performance, making it easier to support informed decision‑making.



Thank you for your support, and I look forward to connecting with you!
