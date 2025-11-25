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
- **cleaning the data using ```UPDATE``` statement.**
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


