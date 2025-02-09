# **Problem No: 81 - Consistent Growth**
## **Difficulty Level:** **HARD**  

**Problem Statement**  
In a financial analysis project, you are tasked with identifying companies that have consistently increased their revenue by at least **25% every year**.

You have a table named `COMPANY` that contains information about the revenue of different companies over several years.

**Conditions:**  
- A company’s revenue must increase by **at least 25% every year consecutively**.  
- If a company’s revenue increases by **25% or more** for three consecutive years but **not for the fourth year**, it **will not** be considered.  

---
## **Company Revenue Data**  

| COMPANY_ID | YEAR | REVENUE  |
|------------|------|---------|
| 1          | 2018 | 100000  |
| 1          | 2019 | 125000  |
| 1          | 2020 | 156250  |
| 1          | 2021 | 200000  |
| 1          | 2022 | 260000  |
| 2          | 2018 | 80000   |
| 2          | 2019 | 100000  |
| 2          | 2020 | 130000  |
| 2          | 2021 | 156000  |
| 2          | 2022 | 180000  |
| 3          | 2018 | 50000   |
| 3          | 2019 | 60000   |
| 3          | 2020 | 72000   |
| 3          | 2021 | 80000   |
| 3          | 2022 | 85000   |
| 4          | 2018 | 200000  |
| 4          | 2019 | 260000  |
| 4          | 2020 | 350000  |
| 4          | 2021 | 450000  |
| 4          | 2022 | 600000  |
--------------------------------

## **Expected Output**  

| company_id | total_revenue |
|------------|--------------|
| 1          | 841250       |
| 4          | 1860000      |


## **Table Schema & Sample Data**  

```sql
CREATE TABLE COMPANY (
    COMPANY_ID INT,
    YEAR INT,
    REVENUE INT,
    PRIMARY KEY (COMPANY_ID, YEAR)
);

INSERT INTO COMPANY (COMPANY_ID, YEAR, REVENUE) VALUES
(1, 2018, 100000),
(1, 2019, 125000),
(1, 2020, 156250),
(1, 2021, 200000),
(1, 2022, 260000),
(2, 2018, 80000),
(2, 2019, 100000),
(2, 2020, 130000),
(2, 2021, 156000),
(2, 2022, 180000),
(3, 2018, 50000),
(3, 2019, 60000),
(3, 2020, 72000),
(3, 2021, 80000),
(3, 2022, 85000),
(4, 2018, 200000),
(4, 2019, 260000),
(4, 2020, 350000),
(4, 2021, 450000),
(4, 2022, 600000);

-------------------------
WITH cte AS (
    SELECT company_id, year, revenue, 
        CASE  
            WHEN revenue >= 1.25 * LAG(revenue,1,0) OVER(PARTITION BY company_id ORDER BY year) THEN 1 
            ELSE 0 
        END AS flag
    FROM COMPANY
)
SELECT company_id, SUM(revenue) AS total_revenue 
FROM cte 
WHERE company_id NOT IN (SELECT company_id FROM cte WHERE flag = 0)
GROUP BY company_id;
