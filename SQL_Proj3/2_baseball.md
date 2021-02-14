# Part 2: Baseball Data

*Introductory - Intermediate level SQL*

---

## Setup

`cd` into the directory you'd like to use for this challenge. Then, download the Lahman SQL Lite dataset

```
curl -L -o lahman.sqlite https://github.com/WebucatorTraining/lahman-baseball-mysql/raw/master/lahmansbaseballdb.sqlite
```

*The `-L` follows redirects, and the `-o` uses the filename instead of outputting to the terminal.*

Make sure sqlite3 is installed

```
conda install -c anaconda sqlite
```

In your notebook, check out the schema

```python
import pandas as pd
import sqlite3

conn = sqlite3.connect('lahman.sqlite')

query = "SELECT * FROM sqlite_master;"

df_schema = pd.read_sql_query(query, conn)

df_schema.tbl_name.unique()
```

---

Please complete this exercise using SQL Lite (i.e., the Lahman baseball data, above) and your Jupyter notebook.

What was the total spent on salaries by each team, each year?

SELECT * FROM salaries GROUP BY teamID, yearID;

What is the first and last year played for each player? Hint: Create a new table from 'Fielding.csv'.

SELECT playerID, yearID
FROM (
SELECT * , ROW_NUMBER()
OVER (PARTITION BY playerID ORDER BY yearID ASC) rn
FROM fielding)
WHERE rn =1

UNION

SELECT playerID, yearID
FROM (
SELECT * , ROW_NUMBER()
OVER (PARTITION BY playerID ORDER BY yearID DESC) rn
FROM fielding)
WHERE rn =1
ORDER BY playerID, yearID

Who has played the most all star games?

SELECT playerID, ROW_NUMBER() OVER (PARTITION BY playerID ORDER BY yearID) AS num_appearance FROM allstarfull ORDER BY num_appearance DESC ;

Willie Mays, Hank Aaron (RIP), and Stan Musial all had 24 appearances.

Which school has generated the most distinct players? Hint: Create new table from 'CollegePlaying.csv'.

WITH colleges AS
(SELECT playerID, schoolID, yearID
FROM (
SELECT * , ROW_NUMBER()
OVER (PARTITION BY playerID ORDER BY yearID DESC) rn
FROM collegeplaying)
WHERE rn =1
)
SELECT schoolID,
ROW_NUMBER() OVER (PARTITION BY schoolID ORDER BY yearID) AS num_players
FROM colleges
ORDER BY num_players DESC;

Texas and USC have produced, by which school players last appeared with, the most players to appear in the MLB with 103.

Which players have the longest career? Assume that the debut and finalGame columns comprise the start and end, respectively, of a player's career. Hint: Create a new table from 'Master.csv'. Also note that strings can be converted to dates using the DATE function and can then be subtracted from each other yielding their difference in days.

SELECT
playerID, debut, finalGame,
(DATE(finalGame) - DATE(debut)) AS 'Career'
FROM people
ORDER BY Career DESC ;

What is the distribution of debut months? Hint: Look at the DATE and EXTRACT functions.

SELECT
strftime('%m', debut) AS 'Debut_Month',
COUNT(* )
FROM people
GROUP BY Debut_Month;