# **Problem No: 88 - Netflix Device Type (Part 1)**
## **Difficulty Level:** **Medium**  

**Problem Statement**  

In the Netflix dataset containing information about viewers and their viewing history, devise a query to identify viewers who primarily use mobile devices for viewing, but occasionally switch to other devices. Specifically, find viewers who have watched at least 75% of their total viewing time on mobile devices but have also used at least one other device such as tablets or smart TVs for viewing. Provide the user ID and the percentage of viewing time spent on mobile devices. Round the result to the nearest integer.

---
## **Input Table**

| USER_ID | TITLE             | DEVICE TYPE | WATCH MINS |
|---------|-------------------|-------------|------------|
| 1       | Narcos            | Smart TV    | 90         |
| 1       | Stranger Things   | Mobile      | 60         |
| 1       | The Crown         | Mobile      | 45         |
| 2       | Narcos            | Mobile      | 95         |
| 2       | Stranger Things   | Mobile      | 100        |
| 2       | The Crown         | Tablet      | 55         |
| 3       | Narcos            | Mobile      | 70         |
| 3       | Stranger Things   | Mobile      | 40         |
| 3       | The Crown         | Mobile      | 60         |
| 4       | Narcos            | Tablet      | 80         |
| 4       | Stranger Things   | Mobile      | 70         |
| 4       | The Crown         | Smart TV    | 65         |
----------------------------------------

## **Expected Output**  

| USER_ID | MOBILE PERCENTAGE VIEW |
|---------|------------------------|
| 2       | 78                     |

## **Table Schema & Sample Data**  

```sql
CREATE TABLE user_watch_history (
    user_id INT,
    title VARCHAR(100),
    device_type VARCHAR(50),
    watch_mins INT,
    PRIMARY KEY (user_id, title, device_type)
);

INSERT INTO user_watch_history (user_id, title, device_type, watch_mins) VALUES
(1, 'Narcos', 'Smart TV', 90),
(1, 'Stranger Things', 'Mobile', 60),
(1, 'The Crown', 'Mobile', 45),
(2, 'Narcos', 'Mobile', 95),
(2, 'Stranger Things', 'Mobile', 100),
(2, 'The Crown', 'Tablet', 55),
(3, 'Narcos', 'Mobile', 70),
(3, 'Stranger Things', 'Mobile', 40),
(3, 'The Crown', 'Mobile', 60),
(4, 'Narcos', 'Tablet', 80),
(4, 'Stranger Things', 'Mobile', 70),
(4, 'The Crown', 'Smart TV', 65);

-------------------------
WITH cte AS (
    SELECT 
        user_id, 
        COUNT(DISTINCT device_type) AS cnt_device,
        SUM(CASE WHEN device_type = 'Mobile' THEN watch_mins END) AS mobile_watch_mins,
        SUM(watch_mins) AS total_watch_mins
    FROM user_watch_history
    GROUP BY user_id
)
SELECT 
    user_id,
    ROUND(mobile_watch_mins * 100.0 / total_watch_mins, 0) AS mobile_percentage_view
FROM cte
WHERE cnt_device >= 2
AND (mobile_watch_mins * 1.0 / total_watch_mins) > 0.75;


