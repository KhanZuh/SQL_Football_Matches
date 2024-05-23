# Football Matches - Tasks

Each of the questions/tasks below can be answered using a `SELECT` query. When you find a solution copy it into the code block under the question before pushing your solution to GitHub.

1) Find all the matches from 2017.

```sql

SELECT * 
FROM public.matches 
WHERE season = 2017 ;

```

2) Find all the matches featuring Barcelona.

```sql
SELECT *
FROM public.matches
WHERE LOWER(hometeam) = LOWER('Barcelona') OR LOWER(awayteam) = LOWER('Barcelona');

```

3) What are the names of the Scottish divisions included?

```sql
SELECT name
FROM public.divisions
WHERE LOWER(country) = 'scotland'

```

4) Find the value of the `code` for the `Bundesliga` division. Use that code to find out how many matches Freiburg have played in that division. HINT: You will need to query both tables

```sql
SELECT code
FROM public.divisions
WHERE name = 'Bundesliga';

-- This gives back code D1

-- Then for number of matches that Freiburg have played
SELECT COUNT(*)
FROM public.matches
WHERE division_code = 'D1'
AND (LOWER(hometeam) = 'freiburg' OR LOWER(awayteam) = 'freiburg');



```

5)  Find the teams which include the word "City" in their name. HINT: Not every team has been entered into the database with their full name, eg. `Norwich City` are listed as `Norwich`. If your query is correct it should return four teams.

```sql
SELECT DISTINCT hometeam
FROM public.matches
WHERE hometeam LIKE '%City%'
UNION
SELECT DISTINCT awayteam
FROM public.matches
WHERE awayteam LIKE '%City%';

-- The UNION operator in SQL is used to combine the results of two or more SELECT statements into a single result set. It removes duplicate rows by default, which makes it useful for combining data from different sources or tables.

```

6) How many different teams have played in matches recorded in a French division?

```sql
SELECT COUNT(DISTINCT team) 
FROM (
    SELECT DISTINCT hometeam AS team
    FROM matches
    WHERE division_code = 'F1' OR division_code = 'F2'
    UNION ALL
    SELECT DISTINCT awayteam AS team
    FROM matches
    WHERE division_code = 'F1' OR division_code = 'F2'
) AS teams_in_french_division ;

```


7) Have Huddersfield played Swansea in any of the recorded matches?

```sql
SELECT *
FROM public.matches
WHERE 
    (hometeam = 'Huddersfield' AND awayteam = 'Swansea')
    OR
    (hometeam = 'Swansea' AND awayteam = 'Huddersfield');

--     Rubber 🦆
-- This query uses the OR logical operator to check if either of the conditions is true: 
-- 1. Huddersfield is the home team and Swansea is the away team.
-- 2. Swansea is the home team and Huddersfield is the away team. 
-- combining these conditions with OR

-- To count the number of matches where Huddersfield has played against Swansea, wrap the previous query as a subquery and then use the COUNT function. 

SELECT COUNT(*)
FROM (
    SELECT *
    FROM public.matches
    WHERE 
        (hometeam = 'Huddersfield' AND awayteam = 'Swansea')
        OR
        (hometeam = 'Swansea' AND awayteam = 'Huddersfield')
) AS subquery;


-- Rubber 🦆

-- In this query:
-- * The inner query retrieves all matches where Huddersfield has played against Swansea.
-- * The outer query counts the number of rows returned by the inner query, giving you the total number of matches.
-- The result will be a single row containing the count of matches where Huddersfield has played against Swansea.




```

8) How many draws were there in the `Eredivisie` between 2010 and 2015?

```sql

SELECT COUNT(DISTINCT id)
FROM public.matches
WHERE division_code = 'N1'
  AND season BETWEEN 2010 AND 2015
  AND ftr = 'D';

-- Returns 431 

-- Check this number

-- In order to check this number we can:
-- -- Count total matches in Eredivisie between 2010 and 2015

SELECT COUNT(*)
FROM public.matches
WHERE division_code = 'N1'
  AND season BETWEEN 2010 AND 2015;

-- Returns 1836

-- If we calculate the percentage of draws out of the total number of matches:
-- 431/1836 = ~ 23.47% = reasonable


```

9) Select the matches played in the Premier League in order of total goals scored (`fthg` + `ftag`) from highest to lowest. When two matches have the same total the match with more home goals (`fthg`) should come first. 

```sql
<!-- Copy solution here -->


```

10) Find the name of the division in which the most goals were scored in a single season and the year in which it happened.

```sql
<!-- Copy solution here -->


```

### Useful Resources

- [Filtering results](https://www.w3schools.com/sql/sql_where.asp)
- [Ordering results](https://www.w3schools.com/sql/sql_orderby.asp)
- [Grouping results](https://www.w3schools.com/sql/sql_groupby.asp)
