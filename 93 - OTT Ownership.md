# **Problem No: 93 - OTT Ownership**
## **Difficulty Level:** **Medium**  

**Problem Statement**  

You have a table named ott_viewership. Write an SQL query to find the top 2 most-watched shows in each genre in the United States. Return the show name, genre, and total duration watched for each of the top 2 most-watched shows in each genre. sort the result by genre and total duration.

---
## **Input Table**

| VIEWER_ID | SHOW_ID | SHOW_NAME            | GENRE     | COUNTRY       | VIEW_DATE  | DURATION_MIN |
|-----------|---------|----------------------|-----------|---------------|------------|--------------|
| 8         | 108     | Brooklyn Nine-Nine   | Comedy    | Canada        | 2023-05-01 | 25           |
| 7         | 107     | Friends              | Comedy    | United States | 2023-05-01 | 30           |
| 13        | 113     | Game of Thrones      | Fantasy   | United States | 2023-05-01 | 50           |
| 1         | 101     | Stranger Things      | Drama     | United States | 2023-05-01 | 60           |
| 2         | 102     | The Crown            | Drama     | United States | 2023-05-01 | 45           |
| 14        | 114     | The Witcher          | Fantasy   | United States | 2023-05-01 | 55           |
| 3         | 103     | Breaking Bad         | Drama     | United States | 2023-05-02 | 50           |
| 16        | 116     | Friends              | Comedy    | United States | 2023-05-02 | 30           |
| 4         | 104     | Game of Thrones      | Fantasy   | United States | 2023-05-02 | 55           |
| 10        | 110     | Parks and Recreation | Comedy    | United States | 2023-05-02 | 40           |
| 15        | 115     | The Mandalorian      | Sci-Fi    | United States | 2023-05-02 | 45           |
| 9         | 109     | The Office           | Comedy    | United States | 2023-05-02 | 35           |
| 12        | 112     | Breaking Bad         | Drama     | United States | 2023-05-03 | 60           |
| 18        | 118     | Parks and Recreation | Comedy    | Canada        | 2023-05-03 | 40           |
| 11        | 111     | Stranger Things      | Drama     | United States | 2023-05-03 | 55           |
| 5         | 105     | The Mandalorian      | Sci-Fi    | United States | 2023-05-03 | 40           |
| 17        | 117     | The Office           | Comedy    | United States | 2023-05-03 | 35           |
| 6         | 106     | The Witcher          | Fantasy   | United States | 2023-05-03 | 60           |
------------------------------------------------------------------------------------------------------

## **Expected Output**  
| SHOW_NAME        | GENRE     | TOTAL_DURATION |
|------------------|-----------|----------------|
| Friends          | Comedy    | 60             |
| The Office       | Comedy    | 70             |
| Breaking Bad     | Drama     | 110            |
| Stranger Things  | Drama     | 115            |
| Game of Thrones  | Fantasy   | 105            |
| The Witcher      | Fantasy   | 115            |
| The Mandalorian  | Sci-Fi    | 85             |
-------------------------------------------------------

## **Table Schema & Sample Data**  

```sql
CREATE TABLE ott_viewership (
    VIEWER_ID INT,
    SHOW_ID INT,
    SHOW_NAME VARCHAR(255),
    GENRE VARCHAR(50),
    COUNTRY VARCHAR(50),
    VIEW_DATE DATE,
    DURATION_MIN INT
);

INSERT INTO ott_viewership (VIEWER_ID, SHOW_ID, SHOW_NAME, GENRE, COUNTRY, VIEW_DATE, DURATION_MIN)
VALUES
(8, 108, 'Brooklyn Nine-Nine', 'Comedy', 'Canada', '2023-05-01', 25),
(7, 107, 'Friends', 'Comedy', 'United States', '2023-05-01', 30),
(13, 113, 'Game of Thrones', 'Fantasy', 'United States', '2023-05-01', 50),
(1, 101, 'Stranger Things', 'Drama', 'United States', '2023-05-01', 60),
(2, 102, 'The Crown', 'Drama', 'United States', '2023-05-01', 45),
(14, 114, 'The Witcher', 'Fantasy', 'United States', '2023-05-01', 55),
(3, 103, 'Breaking Bad', 'Drama', 'United States', '2023-05-02', 50),
(16, 116, 'Friends', 'Comedy', 'United States', '2023-05-02', 30),
(4, 104, 'Game of Thrones', 'Fantasy', 'United States', '2023-05-02', 55),
(10, 110, 'Parks and Recreation', 'Comedy', 'United States', '2023-05-02', 40),
(15, 115, 'The Mandalorian', 'Sci-Fi', 'United States', '2023-05-02', 45),
(9, 109, 'The Office', 'Comedy', 'United States', '2023-05-02', 35),
(12, 112, 'Breaking Bad', 'Drama', 'United States', '2023-05-03', 60),
(18, 118, 'Parks and Recreation', 'Comedy', 'Canada', '2023-05-03', 40),
(11, 111, 'Stranger Things', 'Drama', 'United States', '2023-05-03', 55),
(5, 105, 'The Mandalorian', 'Sci-Fi', 'United States', '2023-05-03', 40),
(17, 117, 'The Office', 'Comedy', 'United States', '2023-05-03', 35),
(6, 106, 'The Witcher', 'Fantasy', 'United States', '2023-05-03', 60);

-------------------------
WITH RankedShows AS (
    SELECT show_name, genre, SUM(duration_min) AS total_duration,
    ROW_NUMBER() OVER (PARTITION BY genre ORDER BY SUM(duration_min) DESC) AS rn
    FROM ott_viewership
    WHERE  country = 'United States'
    GROUP BY show_name, genre
)
SELECT show_name, genre, total_duration
FROM RankedShows
WHERE rn <= 2
ORDER BY genre, total_duration;
