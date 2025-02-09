# **Problem No: 81 - Unique Daily Purchase**
## **Difficulty Level:** **HARD**  

**Problem Statement**  

Suppose you are analyzing the purchase history of customers in an e-commerce platform. Your task is to identify customers who have bought different products on different dates.
 
Write an SQL to find customers who have bought different products on different dates, means product purchased on a given day is not repeated on any other day by the customer. 
Also note that for the customer to qualify he should have made purchases on at least 2 distinct dates. 
Please note that customer can purchase same product more than once on the 
same day and that doesn't disqualify him. Output should contain customer id and number of products bought by the customer in ascending order of userid.



---
## **Purchase History Table**

| USERID | PRODUCTID | PURCHASEDATE |
|--------|----------|--------------|
| 1      | 1        | 2012-01-23   |
| 1      | 1        | 2012-01-23   |
| 1      | 2        | 2012-01-23   |
| 1      | 3        | 2012-01-25   |
| 2      | 1        | 2012-01-23   |
| 2      | 2        | 2012-01-23   |
| 2      | 2        | 2012-01-25   |
| 2      | 4        | 2012-01-25   |
| 3      | 4        | 2012-01-23   |
| 3      | 1        | 2012-01-23   |
| 4      | 1        | 2012-01-23   |
| 4      | 2        | 2012-01-25   |
----------------------------------------

## **Expected Output**  

| userid | cnt_products |
|--------|--------------|
| 1      | 3            |
| 4      | 2            |


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
    SELECT 
        userid, 
        productid, 
        purchasedate
    FROM history
    GROUP BY userid, productid, purchasedate
), 
cte1 AS (
    SELECT 
        userid, 
        COUNT(DISTINCT purchasedate) AS cnt_dist_date,
        COUNT(productid) AS cnt_products,
        COUNT(DISTINCT productid) AS cnt_dist_products
    FROM cte
    GROUP BY userid
) 
SELECT 
    userid, 
    cnt_products
FROM cte1
WHERE cnt_dist_date > 1 
    AND cnt_products = cnt_dist_products
ORDER BY userid;
