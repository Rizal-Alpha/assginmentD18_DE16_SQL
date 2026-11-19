# Soal 1: The Report
### Jawaban versi 1:
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
```SET NOCOUNT ON;

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
