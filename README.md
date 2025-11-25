# SALES-ANALYTICS-WITH-SQL
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

- **BUSINESS QUESTIONS**

1. Write an SQL query to extract all transactions where the category is 'Electronics' for sales made in '2022-10'

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

Q7. 




