# **Problem No: 89 - Netflix Device Type (Part-2).**
## **Difficulty Level:** **Medium**  

# **Problem Statement**  

In the Netflix viewing history dataset, you are tasked with identifying viewers who have a consistent viewing pattern across multiple devices. Specifically, viewers who have watched the same title on more than 1 device type.
 
Write an SQL query to find users who have watched more number of titles on multiple devices than the number of titles they watched on single device. Output the user id , no of titles watched on multiple devices and no of titles watched on single device, display the output in ascending order of user_id.

---
## **Input Table**
## User Activity Table

| USER_ID | TITLE             | DEVICE_TYPE |
|---------|-------------------|-------------|
| 1       | Narcos            | Smart TV    |
| 1       | Stranger Things   | Mobile      |
| 1       | Stranger Things   | Smart TV    |
| 1       | The Crown         | Mobile      |
| 1       | The Crown         | Smart TV    |
| 2       | Narcos            | Mobile      |
| 2       | Narcos            | Tablet      |
| 2       | Stranger Things   | Mobile      |
| 2       | The Crown         | Tablet      |
| 3       | Stranger Things   | Mobile      |
| 3       | Stranger Things   | Tablet      |
| 3       | Stranger Things   | Smart TV    |
| 4       | Stranger Things   | Mobile      |
------------------------------------------------------

## **Expected Output**  

| USER_ID | MULTIPLE_DEVICE_CNT | SINGLE_DEVICE_CNT |
|---------|--------------------|------------------- |
| 1       | 2                  | 1                  |
| 3       | 1                  | 0                  |
--------------------------------------------------------


## **Table Schema & Sample Data**  

```sql
CREATE TABLE UserActivity (
    USER_ID INT,
    TITLE VARCHAR(255),
    DEVICE_TYPE VARCHAR(50)
);

-- Insert the data from the image into the UserActivity table
INSERT INTO UserActivity (USER_ID, TITLE, DEVICE_TYPE) VALUES
(1, 'Narcos', 'Smart TV'),
(1, 'Stranger Things', 'Mobile'),
(1, 'Stranger Things', 'Smart TV'),
(1, 'The Crown', 'Mobile'),
(1, 'The Crown', 'Smart TV'),
(2, 'Narcos', 'Mobile'),
(2, 'Narcos', 'Tablet'),
(2, 'Stranger Things', 'Mobile'),
(2, 'The Crown', 'Tablet'),
(3, 'Stranger Things', 'Mobile'),
(3, 'Stranger Things', 'Tablet'),
(3, 'Stranger Things', 'Smart TV'),
(4, 'Stranger Things', 'Mobile');

-------------------------
WITH DeviceCounts AS (
    SELECT 
        USER_ID,
        TITLE,
        COUNT(DISTINCT DEVICE_TYPE) AS DeviceTypeCount  -- Count distinct device types for each title
    FROM UserActivity  -- Assuming your input table is named UserActivity
    GROUP BY USER_ID, TITLE
),
UserDeviceSummary AS (
    SELECT 
        USER_ID,
        SUM(CASE WHEN DeviceTypeCount > 1 THEN 1 ELSE 0 END) AS MultipleDeviceCount,
        SUM(CASE WHEN DeviceTypeCount = 1 THEN 1 ELSE 0 END) AS SingleDeviceCount
    FROM DeviceCounts
    GROUP BY USER_ID
)
SELECT *
FROM UserDeviceSummary
WHERE MultipleDeviceCount > SingleDeviceCount  -- Filter condition as per your requirement
ORDER BY USER_ID;
