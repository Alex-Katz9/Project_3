# Part 3: Soccer Data

*Introductory - Intermediate level SQL*

---

## Setup

Download the [SQLite database](https://www.kaggle.com/hugomathien/soccer/download). *Note: You may be asked to log in, or "continue and download".* Unpack the ZIP file into your working directory (i.e., wherever you'd like to complete this challenge set). There should be a *database.sqlite* file.

As with Part II, you can check the schema:

```python
import pandas as pd
import sqlite3

conn = sqlite3.connect('database.sqlite')

query = "SELECT * FROM sqlite_master"

df_schema = pd.read_sql_query(query, conn)

df_schema.tbl_name.unique()
```

---

Please complete this exercise using sqlite3 (the soccer data, above) and your Jupyter notebook.

Which team scored the most points when playing at home?

SELECT Team.team_long_name AS Name,
Match.home_team_api_id,
SUM(Match.home_team_goal) AS Total_Goals
FROM Match
LEFT JOIN Team
ON Match.home_team_api_id = Team.team_api_id
GROUP BY Match.home_team_api_id
ORDER BY Total_Goals DESC
;

Real Madrid had the most home goals with 505.

Did this team also score the most points when playing away?

SELECT Team.team_long_name AS Name,
Match.away_team_api_id,
SUM(Match.away_team_goal) AS Total_Goals
FROM Match
LEFT JOIN Team
ON Match.away_team_api_id = Team.team_api_id
GROUP BY Match.away_team_api_id
ORDER BY Total_Goals DESC
;

They were not first in away goals, but second to FC Barcelona by 16 goals.

How many matches resulted in a tie?

SELECT COUNT(* )
FROM Match
WHERE home_team_goal = away_team_goal
;

There were 6596 ties in this database.

How many players have Smith for their last name? How many have 'smith' anywhere in their name?

SELECT COUNT()
FROM Player
WHERE player_name LIKE '% Smith'
;
SELECT COUNT()
FROM Player
WHERE player_name LIKE '%smith%'
;

There are 15 players with a last name of Smith and 18 with 'smith' anywhere in their name.

What was the median tie score? Use the value determined in the previous question for the number of tie games. Hint: PostgreSQL does not have a median function. Instead, think about the steps required to calculate a median and use the WITH command to store stepwise results as a table and then operate on these results.

WITH ties AS (
SELECT home_team_goal,
ROW_NUMBER() OVER (ORDER BY home_team_goal) as row_id
FROM Match
WHERE home_team_goal = away_team_goal
ORDER BY home_team_goal
)

SELECT AVG(home_team_goal) AS Median
FROM ties
WHERE row_id BETWEEN 6596/2 AND 6596/2+1
;

The median goals scored in a tie game is 1 for each team.

What percentage of players prefer their left or right foot? Hint: Calculate either the right or left foot, whichever is easier based on how you setup the problem.

WITH player_foot AS (SELECT player_api_id, preferred_foot
FROM Player_Attributes
GROUP BY player_api_id)

SELECT COUNT(* )
FROM player_foot
WHERE preferred_foot = 'left'
;

Repeat the above for right footed players.

Since there are 11060 players total and 8373 of them prefer their right foot, that works out to a little over 75%.

