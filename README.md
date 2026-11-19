# Soal 1: The Report
### Jawaban versi 1 (MySQL):
```sql
SELECT 
    case when g.Grade >= 8 then s.Name else "NULL" end as Name,
    g.Grade,
    s.Marks
FROM Students s
LEFT JOIN Grades g ON s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
order by g.Grade Desc, s.Name;
```
### Jawaban versi 2 (MS SQL):
```sql
SET NOCOUNT ON;

WITH cte_student_grade AS (
    SELECT
        s.Name AS Name,
        g.Grade AS Grade,
        s.Marks AS Marks
    FROM Students s
    LEFT JOIN Grades g
        ON s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
)
SELECT
    CASE
        WHEN csg.Grade >= 8 THEN csg.Name
        ELSE 'NULL'
    END AS Name,
    csg.Grade,
    csg.Marks
FROM cte_student_grade csg
ORDER BY
    csg.Grade DESC,
    CASE WHEN csg.Grade >= 8 THEN csg.Name END ASC,
    CASE WHEN csg.Grade < 8 THEN csg.Marks END ASC;

go
```
###Output:
![Hasil Query Soal 1](Output_Soal_1_HackerRank.png)


# Soal 2: Weather Observation Station 18
### Jawaban (MS SQL):

```sql
SET NOCOUNT ON;

with cte_min_max as (
    select
    min(LAT_N) min_lat,
    max(LAT_N) max_lat,
    min(LONG_W) min_long,
    max(LONG_W) max_long
    from STATION
)
select 
    cast(round(abs(min_lat - max_lat) + abs(min_long - max_long),4) 
    as decimal(10,4))
from cte_min_max;

go
```
###Output:
![Hasil Query Soal 2](Output_Soal_2_HackerRank.png)


# Soal 3: Top Competitors
### Jawaban (MS SQL):

```sql
SET NOCOUNT ON;

--tentukan max_score_criteria per challenge
WITH cte_max_criteria AS (
    SELECT c.challenge_id, d.score AS max_criteria
    FROM Challenges c
    INNER JOIN Difficulty d ON c.difficulty_level = d.difficulty_level
),
--ambil mereka yg took challenge >1(nama harus >1) dan scorenya reach max_criteria
cte_score_submitted AS (   
  SELECT    
    count(hacker_id) jumlah_challenge_diikuti , hacker_id
    FROM Submissions s
    INNER JOIN cte_max_criteria cmc on cmc.challenge_id = s.challenge_id
    where s.score = cmc.max_criteria 
    group by hacker_id
    having count(hacker_id) > 1
),
--dapatkan identitas para peserta
cte_person_name AS (
    SELECT css.hacker_id, h.name
    FROM Hackers h
    INNER JOIN cte_score_submitted css on h.hacker_id = css.hacker_id
)
SELECT 
    cpn.hacker_id, cpn.name
FROM cte_person_name cpn
INNER JOIN cte_score_submitted css on cpn.hacker_id = css.hacker_id
ORDER BY css.jumlah_challenge_diikuti Desc, cpn.hacker_id

go
```
###Output:
![Hasil Query Soal 3](Output_Soal_3_HackerRank.png)