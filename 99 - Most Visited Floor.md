# **Problem No: 99 - Most Visited Floor.**
## **Difficulty Level:** **Medium**  

**Problem Statement**  

You are a facilities manager at a corporate office building, responsible for tracking employee visits, floor preferences, and resource usage within the premises. The office building has multiple floors, each equipped with various resources such as desks, computers, monitors, and other office supplies. You have a database table “entries” that stores information about employee visits to the office building. Each record in the table represents a visit by an employee and includes details such as their name, the floor they visited, and the resources they used during their visit.
 
Write an SQL query to retrieve the total visits, most visited floor, and resources used by each employee, display the output in ascending order of employee name.

---
## **Input Table**
## User Entries Table

| EMP_NAME | ADDRESS | FLOOR | RESO |
|---|---|---|---|
| Ankit | Bangalore | 1 | CPU |
| Ankit | Bangalore | 1 | CPU |
| Ankit | Bangalore | 2 | DESKTOP |
| Bikaas | Bangalore | 2 | DESKTOP |
| Bikaas | Bangalore | 2 | DESKTOP |
| Bikaas | Bangalore | 1 | MONITOR |
------------------------------------------------------

## **Expected Output**  

| EMP_NAME | TOTAL_VISIT | FLOOR | RESOURCES      |
|----------|-------------|-------|----------------|
| Ankit    | 3           | 1     | CPUDESKTOP     |
| Bikaas   | 3           | 2     | DESKTOPMONITOR |
---------------------------------------------------


## **Table Schema & Sample Data**  

```sql
CREATE TABLE EmployeeResources (
    EMP_NAME VARCHAR(255),
    ADDRESS VARCHAR(255),
    FLOOR VARCHAR(255),
    RESOURCES VARCHAR(255)
);

INSERT INTO EmployeeResources (EMP_NAME, ADDRESS, FLOOR, RESOURCES) VALUES
('Ankit', 'Bangalore', 1, 'CPU'),
('Ankit', 'Bangalore', 1, 'CPU'),
('Ankit', 'Bangalore', 2, 'DESKTOP'),
('Bikaas', 'Bangalore', 2, 'DESKTOP'),
('Bikaas', 'Bangalore', 2, 'DESKTOP'),
('Bikaas', 'Bangalore', 1, 'MONITOR');

-------------------------
WITH cte AS (
    SELECT EMP_NAME, COUNT(*) AS TOTAL_VISIT, GROUP_CONCAT(DISTINCT RESO SEPARATOR '') AS resources
    FROM EmployeeResources
    GROUP BY EMP_NAME
), 
cte2 AS (
    SELECT EMP_NAME, FLOOR, COUNT(*) AS total_visited_floor,
           RANK() OVER (PARTITION BY EMP_NAME ORDER BY COUNT(*) DESC) AS rnk
    FROM EmployeeResources
    GROUP BY EMP_NAME, FLOOR
)
SELECT c.EMP_NAME, c.TOTAL_VISIT, cc.FLOOR, c.resources
FROM cte c 
JOIN cte2 cc ON c.EMP_NAME = cc.EMP_NAME
WHERE cc.rnk = 1;
