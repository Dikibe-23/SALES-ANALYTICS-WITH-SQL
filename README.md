# SALES-ANALYTICS-WITH-SQL

## Contents
1. [Key features](#Key-features)
2. [Skills and Concepts](#Skills-Concepts)
3. [Steps](#Steps)(#Data-retrieval-and-exploration)
4. [Insights](#Insights)
## Description:
This project showcases SQL skills in data analysis and reporting. The dataset contains sales transactions with information on product categories, quantity, prices, and dates. Queries were written to extract business insights such as seasonal trend, product performance, and percentage contributions. I have skipped the process of creating a database and creating a table in PostgreSQL. The focus is on extracting key business information froom the dataset provided in the project repository.

## Key features:
- **Data Loading:** Loaded the data into PostgreSQL.
- **Data Cleaning:** Identified and updated missing or null records using ```UPDATE``` statement.
- **Exploratory Data Analysis(EDA):** Here we explored the data to understand and identify key information the data can provide.
- **Business Intelligence Derivation:** Deduced key business metrics from the data following curated business questions.

**Objectives:**
- Calculate product performance over time.
- Calculate total and yearly revenue across multiple product categories.
- Calculate category contribution (%) to total yearly sales.
- Use windows functions to derive company-wide totals and rankings.
- Prepare analytical queries suitable for integration into dasboards and BI tools.

## Skills and Concepts 
- **Data aggregation** with ``` SUM(), COUNT() and AVG()```.
- **Date function** ```EXTRACT(YEAR FROM ...)```.
- **Grouping and sorting** using ```GROUP BY and ORDER BY``` statements.
- **Window functions** for cummulative and partitioned analysis.
- **Formatting outputs** using ```TO_CHAR``` for currency and percentages.
- **Subqueries and CTEs** for better query structure and readability.

## Steps
Imagine we have the data named ```retailsales``` already in our database (relational) and wish to retrieve the data to answer certain key business questions, the steps are outlined below.

### 1. Data retrieval and exploration

```
SELECT * FROM retailsales;
```

### 2. Check for missing records and clean the data

```
SELECT * FROM retailsales 
WHERE
transactions_id is NULL
OR
sale_date is NULL
OR
sale_time is NULL
OR
customer_id is NULL
OR
gender is NULL
OR
age is NULL
OR
category is NULL
OR
quantity is NULL
OR 
price_per_unit is NULL;
```
- **Cleaning the data using ```UPDATE``` statement.**
```
UPDATE retailsales
SET age = 27
WHERE transactions_id = 432;

UPDATE retailsales
SET age = 30
WHERE transactions_id = 1367;

UPDATE retailsales
SET age = 35
WHERE transactions_id = 1391;

UPDATE retailsales
SET age = 19
WHERE transactions_id = 1432;

UPDATE retailsales
SET age = 20
WHERE transactions_id = 150;

UPDATE retailsales
SET age = 24
WHERE transactions_id = 845;

UPDATE retailsales
SET age = 28
WHERE transactions_id = 1150;

UPDATE retailsales
SET age = 32
WHERE transactions_id = 1845;

UPDATE retailsales
SET age = 35
WHERE transactions_id = 797;

UPDATE retailsales
SET age = 39
WHERE transactions_id = 921;

UPDATE retailsales
SET quantity = 3
WHERE transactions_id = 679;

UPDATE retailsales
SET quantity = 2
WHERE transactions_id = 746;

UPDATE retailsales
SET quantity = 4
WHERE transactions_id = 1225;
```

```
UPDATE retailsales
SET price_per_unit = 150
WHERE transactions_id = 1225;

UPDATE retailsales
SET price_per_unit = 200
WHERE transactions_id = 746;

UPDATE retailsales
SET price_per_unit = 300
WHERE transactions_id = 679;
```

- **Data Exploration:** 

1. We want to know the total number of customers the business has so far order by customer _id in Ascending order 

```
SELECT COUNT(DISTINCT customer_id) AS total_customer FROM retailsales;
```
2. Total number of transactions made by the business so far.

```
SELECT COUNT(transactions_id) AS total_sales FROM retailsales;
```
3. We also want to know the number of item category the business has.
```
SELECT COUNT(DISTINCT category) AS item_category FROM retailsales
```

### BUSINESS QUESTIONS**

Q1. Write an SQL query to extract all transactions where the category is 'Electronics' for sales made in '2022-10'

```
SELECT * FROM retailsales
WHERE category = 'Electronics'
AND 
TO_CHAR(sale_date, 'YYYY-MM') = '2022-10'
AND
quantity > 1;
```

Q2. Write an SQL query to retrieve all columns for sales made on a '2022-06-16'.

```
SELECT * FROM retailsales
WHERE sale_date = '2022-06-16'
```

Q3. Write an SQL query to find the average sale, total orders, and total sale for each category.

```
SELECT category, COUNT(transactions_id) AS total_order, ROUND(AVG(total_sale)) AS avg_sale, SUM(total_sale) AS sum_of_sales 
FROM retailsales
GROUP BY 
category;
```

Q4. Calculate the percentage of trasaction by category.
```
SELECT category, COUNT(transactions_id) AS total_order, ROUND(COUNT(transactions_id)) * 100 / (SELECT COUNT(transactions_id) FROM retailsales) AS order_pct 
FROM retailsales
GROUP BY
category;
```
Q5. Write an SQL query to extract transaction where total sale exceeded 500.

```
SELECT * FROM retailsales
WHERE total_sale > 500;
```

Q6. How much revenue does the company make according to category per year.

```
SELECT DISTINCT EXTRACT(YEAR FROM sale_date) AS year,
category,
TO_CHAR(SUM(quantity * price_per_unit), 'L999,999,999,999') AS total_revenue,
FROM retailsales
GROUP BY category, EXTRACT(YEAR FROM sale_date)
ORDER BY year;
```

Q7. How much revenue does the company make according to category per year.

```
SELECT DISTINCT EXTRACT(YEAR FROM sale_date) AS year,
category,
TO_CHAR(SUM(quantity * price_per_unit), 'L999,999,999,999') AS total_revenue,
FROM retailsales
GROUP BY category, EXTRACT(YEAR FROM sale_date)
ORDER BY year;
```

Q8. How much does the company make from sales for the entire year across all categories.

```
SELECT DISTINCT EXTRACT(YEAR FROM sale_date) AS year,
category,
TO_CHAR(SUM(quantity * price_per_unit), 'L999,999,999,999') AS total_revenue,
TO_CHAR(SUM
(SUM(quantity * price_per_unit)) OVER 
(PARTITION BY EXTRACT(YEAR FROM sale_date)), 'L999, 999, 999, 999') 
AS entire_year_rev
FROM retailsales
GROUP BY category, EXTRACT(YEAR FROM sale_date)
ORDER BY year;
```

Q9. How much percentage did each category to the company's entire year revenue.
```
SELECT DISTINCT EXTRACT(YEAR FROM sale_date) AS year,
category,
TO_CHAR(SUM(total_sale), 'FM999,999,999,999 "€"') AS total_rev,
TO_CHAR(SUM(SUM(total_sale)) OVER (PARTITION BY EXTRACT (YEAR FROM sale_date)), 
'FM999,999,999,999 "€"') AS cumm_rev,
TO_CHAR(100.0 * SUM(total_sale) / SUM(SUM(total_sale)) OVER (PARTITION BY EXTRACT(YEAR FROM sale_date)), 'FM999.00 "%"') AS pct_rev_contribution
FROM retailsales
GROUP BY category, EXTRACT(YEAR FROM sale_date)
ORDER BY year;
```

Q10. Write an SQL query to find the total number of transactions made by each gender for each category.
```
SELECT category, gender,
COUNT(transactions_id) AS total_transactions
FROM retailsales
GROUP BY gender, category
ORDER BY category;
```

Q11. Write an SQL to calculate the average sale for each month. Find out best selling month in each year.
```
SELECT * FROM
(
	SELECT  
	EXTRACT(YEAR FROM sale_date) AS year,
	EXTRACT(MONTH FROM sale_date) AS month,
	AVG(total_sale) AS avg_sale,
	RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC)
	AS ranking
	FROM retailsales
	GROUP BY 1, 2
	ORDER BY 4, 1) AS rank_table

WHERE ranking = 1;
```

Q12. Write an SQL query to determine the top 5 customers based on transaction amount. find out what items category these customers bought.
```
SELECT customer_id,
SUM(total_sale) AS total_sales
FROM retailsales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

Q13. Find the number of unique customers who purchased items from each category.
```
SELECT COUNT (DISTINCT customer_id) AS distinct_customer, 
category
FROM retailsales
GROUP BY category;
```

Q14. Write an query to create each shift and number of orders (Example Morning shift <13, Afternoon shift between 13 - 20, etc.) We use CTE in this example.
```
SELECT * FROM retailsales;

WITH shift_orders
AS
(
SELECT *,
	CASE
		WHEN EXTRACT(HOUR FROM sale_time) < 13 THEN 'Morning'
		WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 13 AND 20 THEN 'Afternoon'
		ELSE 'Night'
	END AS shifts
FROM retailsales
)
SELECT shifts,
COUNT(*) AS number_of_order
FROM shift_orders
GROUP BY shifts;
```

## Insights

1. July 2022 was on average the best selling month. Likewise in 2023, Feb saw the highest average revenue.
2. Clothing receieved the highest number of patronage.
3. Electronics had the highest contribution to total revenue 2022 and 2023 with 35.38% compared to other categories.
4. In 2022, the company realized 454,725€ in revenue across all categories. while in 2023, it realized 458,895€.
5. Afternoon shift workers processed the highest number of orders.





