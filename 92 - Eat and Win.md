# **Problem No: 92 - Eat and Win**
## **Difficulty Level:** **Medium**  

**Problem Statement**  

A pizza eating competition is organized. All the participants are organized into different groups. In a contest , A participant who eat the most pieces of pizza is the winner and recieves their original bet plus 30% of all losing participants bets. In case of a tie all winning participants will get equal share (of 30%) divided among them .Return the winning participants' names for each group and amount of their payout(round to 2 decimal places) . ordered ascending by group_id , participant_name.

---
## **Input Table**

| GROUP_ID | PARTICIPANT_NAME | SLICE_COUNT | BET |
|----------|------------------|-------------|-----|
| 1        | Alice            | 10          | 51  |
| 1        | Bob              | 15          | 42  |
| 1        | Eve              | 15          | 30  |
| 1        | Jerry            | 12          | 12  |
| 1        | Tom              | 8           | 21  |
| 2        | Charlie          | 20          | 60  |
| 2        | David            | 20          | 72  |
| 2        | Mike             | 20          | 54  |
| 2        | Nancy            | 18          | 42  |
| 2        | Oliver           | 10          | 30  |
| 3        | Frank            | 12          | 51  |
| 3        | Grace            | 18          | 48  |
| 3        | Hank             | 15          | 30  |
| 3        | Irene            | 14          | 21  |
| 3        | John             | 16          | 60  |
| 4        | Ivy              | 25          | 102 |
| 4        | Jack             | 22          | 81  |
| 4        | Kathy            | 20          | 90  |
| 4        | Leo              | 18          | 72  |
| 4        | Mia              | 15          | 51  | 
---------------------------------------------------


## **Expected Output**  

| GROUP_ID | PARTICIPANT_NAME | Total_Payout |
|----------|------------------|--------------|
| 1        | Bob              | 54.6         |
| 1        | Eve              | 42.6         |
| 2        | Charlie          | 67.2         |
| 2        | David            | 79.2         |
| 2        | Mike             | 61.2         |
| 3        | Grace            | 96.6         |
| 4        | Ivy              | 190.2        |
----------------------------------------------

## **Table Schema & Sample Data**  

```sql
-- Create the table
CREATE TABLE Competition (
    GROUP_ID INT,
    PARTICIPANT_NAME VARCHAR(20),
    SLICE_COUNT INT,
    BET INT
);

-- Insert the values
INSERT INTO Competition (GROUP_ID, PARTICIPANT_NAME, SLICE_COUNT, BET) VALUES
(1, 'Alice', 10, 51),
(1, 'Bob', 15, 42),
(1, 'Eve', 15, 30),
(1, 'Jerry', 12, 12),
(1, 'Tom', 8, 21),
(2, 'Charlie', 20, 60),
(2, 'David', 20, 72),
(2, 'Mike', 20, 54),
(2, 'Nancy', 18, 42),
(2, 'Oliver', 10, 30),
(3, 'Frank', 12, 51),
(3, 'Grace', 18, 48),
(3, 'Hank', 15, 30),
(3, 'Irene', 14, 21),
(3, 'John', 16, 60),
(4, 'Ivy', 25, 102),
(4, 'Jack', 22, 81),
(4, 'Kathy', 20, 90),
(4, 'Leo', 18, 72),
(4, 'Mia', 15, 51);
-------------------------
with cte as (
select *, rank() over(partition by group_id order by slice_count desc) as rn
from Competition
)
, cte2 as
(
select group_id,sum(case when rn=1 then 1 else 0 end) as no_of_winners
,sum(case when rn>1 then bet else 0 end)*0.3 as losers_bet
from cte
group by group_id
)
select cte.group_id,cte.participant_name
,round(cte.bet+ (cte2.losers_bet)/cte2.no_of_winners,2) as total_payout
from cte
inner join cte2 on cte.group_id=cte2.group_id
where cte.rn=1
order by cte.group_id,cte.participant_name;
