# **Problem No: 94 - GAP Sales**
## **Difficulty Level:** **Easy**  

**Problem Statement**  

You have a table called gap_sales. Write an SQL query to find the total sales for each category in each store for the Q2(April-June) of 2023. Return the store ID, category name, and total sales for each category in each store. Sort the result by total sales in ascending order.

---
## **Input Table**

| SALE_ID | STORE_ID | SALE_DATE  | CATEGORY   | TOTAL_SALES |
|---------|----------|------------|------------|-------------|
| 1       | 101      | 2023-01-01 | Clothing   | 6000        |
| 14      | 103      | 2023-02-01 | Outerwear  | 3500        |
| 12      | 102      | 2023-04-03 | Outerwear  | 2000        |
| 2       | 101      | 2023-05-01 | Outerwear  | 2000        |
| 7       | 102      | 2023-05-01 | Clothing   | 7000        |
| 13      | 103      | 2023-05-01 | Clothing   | 8000        |
| 3       | 101      | 2023-05-02 | Clothing   | 4000        |
| 4       | 101      | 2023-05-02 | Outerwear  | 1500        |
| 9       | 102      | 2023-05-02 | Clothing   | 5000        |
| 10      | 102      | 2023-05-02 | Outerwear  | 2500        |
| 15      | 103      | 2023-05-02 | Clothing   | 6000        |
| 16      | 103      | 2023-05-02 | Outerwear  | 3000        |
| 5       | 101      | 2023-05-03 | Clothing   | 3500        |
| 6       | 101      | 2023-05-03 | Outerwear  | 2500        |
| 11      | 102      | 2023-05-03 | Clothing   | 4000        |
| 17      | 103      | 2023-05-03 | Clothing   | 4500        |
| 8       | 102      | 2023-06-01 | Outerwear  | 3000        |
| 18      | 103      | 2023-07-03 | Outerwear  | 2500        |
--------------------------------------------------------------

## **Expected Output**  

| STORE_ID | CATEGORY    | TOTAL_SALES |
|----------|-------------|-------------|
| 103      | Outerwear   |  3000       |        
| 101      | Outerwear   |  6000       |       
| 101      | Clothing    |  7500       |        
| 102      | Outerwear   |  7500       | 
| 102      | Clothing    |  16500      |    
| 103      | Clothing    |  18500      |    
----------------------------------------

## **Table Schema & Sample Data**  

```sql
-- Create the table
CREATE TABLE Sales (
    SALE_ID INT,
    STORE_ID INT,
    SALE_DATE DATE,
    CATEGORY VARCHAR(20),
    TOTAL_SALES INT
);

-- Insert the values
INSERT INTO Sales (SALE_ID, STORE_ID, SALE_DATE, CATEGORY, TOTAL_SALES) VALUES
(1, 101, '2023-01-01', 'Clothing', 6000),
(14, 103, '2023-02-01', 'Outerwear', 3500),
(12, 102, '2023-04-03', 'Outerwear', 2000),
(2, 101, '2023-05-01', 'Outerwear', 2000),
(7, 102, '2023-05-01', 'Clothing', 7000),
(13, 103, '2023-05-01', 'Clothing', 8000),
(3, 101, '2023-05-02', 'Clothing', 4000),
(4, 101, '2023-05-02', 'Outerwear', 1500),
(9, 102, '2023-05-02', 'Clothing', 5000),
(10, 102, '2023-05-02', 'Outerwear', 2500),
(15, 103, '2023-05-02', 'Clothing', 6000),
(16, 103, '2023-05-02', 'Outerwear', 3000),
(5, 101, '2023-05-03', 'Clothing', 3500),
(6, 101, '2023-05-03', 'Outerwear', 2500),
(11, 102, '2023-05-03', 'Clothing', 4000),
(17, 103, '2023-05-03', 'Clothing', 4500),
(8, 102, '2023-06-01', 'Outerwear', 3000),
(18, 103, '2023-07-03', 'Outerwear', 2500);

------------------------------------------
SELECT  store_id, category, SUM(total_sales) AS total_sales
FROM  gap_sales
WHERE 
    YEAR(sale_date) = 2023 
    AND MONTH(sale_date) IN (4, 5, 6)
GROUP BY  store_id, category
ORDER BY total_sales;
