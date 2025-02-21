# **Problem No: 90 - Calculate Customer Interest**
## **Difficulty Level:** **HARD**  

**Problem Statement**  
You are tasked with analyzing the interest earned by customers based on their account balances and transaction history. Each customer's account accrues interest based on their balance and prevailing interest rates. The interest is calculated for the ending balance on each day. Your goal is to determine the total interest earned by each customer for the month of March-2024. The interest rates (per day) are given in the interest table as per the balance amount range.
 
Please assume that the account balance for each customer was 0 at the start of March 2024. Write an SQL to calculate interest earned by each customer from March 1st 2024 to March 31st 2024, display the output in ascending order of customer id. 

---
## **Transactions Table Data**

| TRANSACTION_ID | CUSTOMER_ID | TRANSACTION_DATE | AMOUNT |
|---|---|---|---|
| 1 | 1 | 2024-03-01 | 1000 |
| 2 | 2 | 2024-03-02 | 500 |
| 3 | 3 | 2024-03-03 | 1500 |
| 4 | 1 | 2024-03-15 | -300 |
| 5 | 2 | 2024-03-20 | 700 |
| 6 | 3 | 2024-03-25 | -200 |
| 7 | 1 | 2024-03-28 | 400 |
| 8 | 2 | 2024-03-30 | -800 |
| 9 | 3 | 2024-03-31 | 1000 |
| 10 | 3 | 2024-03-31 | 100 |
-----------------------------------------------
## **InterestRates Table Data**

| RATE_ID | MIN_BALANCE | MAX_BALANCE | INTEREST_RATE |
|---|---|---|---|
| 1 | 0 | 499 | 0.01 |
| 2 | 500 | 999 | 0.02 |
| 3 | 1000 | 1499 | 0.03 |
| 4 | 1500 | 99999 | 0.04 |
----------------------------------------------

## **Expected Output**  

| customer_id | interest_earned |
|---|---|
| 1 | 734.000000 |
| 2 | 548.000000 |
| 3 | 1650.000000 |


## **Table Schema & Sample Data**  

```sql
CREATE TABLE Transactions (
    TRANSACTION_ID INT PRIMARY KEY,
    CUSTOMER_ID INT,
    TRANSACTION_DATE DATE,
    AMOUNT DECIMAL(10, 2) 
);

INSERT INTO Transactions (TRANSACTION_ID, CUSTOMER_ID, TRANSACTION_DATE, AMOUNT) VALUES
(1, 1, '2024-03-01', 1000),
(2, 2, '2024-03-02', 500),
(3, 3, '2024-03-03', 1500),
(4, 1, '2024-03-15', -300),
(5, 2, '2024-03-20', 700),
(6, 3, '2024-03-25', -200),
(7, 1, '2024-03-28', 400),
(8, 2, '2024-03-30', -800),
(9, 3, '2024-03-31', 1000),
(10, 3, '2024-03-31', 100);
---------------------------

CREATE TABLE InterestRates (
    RATE_ID INT PRIMARY KEY,
    MIN_BALANCE DECIMAL(10, 2),
    MAX_BALANCE DECIMAL(10, 2),
    INTEREST_RATE DECIMAL(5, 4) 
);

INSERT INTO InterestRates (RATE_ID, MIN_BALANCE, MAX_BALANCE, INTEREST_RATE) VALUES
(1, 0, 499, 0.01),
(2, 500, 999, 0.02),
(3, 1000, 1499, 0.03),
(4, 1500, 99999, 0.04);

--------------------------------------
WITH cte AS (
    SELECT
        customer_id,
        transaction_date,
        SUM(amount) OVER (PARTITION BY customer_id ORDER BY transaction_date) AS net_amount
    FROM Transactions
),
cte2 AS (
    SELECT
        customer_id,
        transaction_date,
        net_amount,
        DATE_ADD(LEAD(transaction_date, 1, '2024-04-01') OVER (PARTITION BY customer_id ORDER BY transaction_date), INTERVAL 0 DAY) AS next_trans_date
    FROM cte
)
SELECT
    cte2.customer_id,
    SUM(
        DATEDIFF(cte2.next_trans_date, cte2.transaction_date) * cte2.net_amount * ir.interest_rate
    ) AS interest_earned
FROM cte2
INNER JOIN InterestRates ir
    ON cte2.net_amount BETWEEN ir.min_balance AND ir.max_balance
GROUP BY cte2.customer_id
ORDER BY cte2.customer_id;
