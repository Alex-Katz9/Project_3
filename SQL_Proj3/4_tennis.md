# Part 4: Tennis Data

*Intermediate - Advanced level SQL*

---

## Setup

We'll be using tennis data from [here](https://archive.ics.uci.edu/ml/datasets/Tennis+Major+Tournament+Match+Statistics).

Navigate to your preferred working directory, and download the data.

```bash
curl -L -o tennis.zip http://archive.ics.uci.edu/ml/machine-learning-databases/00300/Tennis-Major-Tournaments-Match-Statistics.zip
unzip tennis.zip -d tennis
```

Completed using Postgres and SQL Alchemy in Jupyter Notebook

This challenge uses only SQL queries. Please submit answers in a markdown file.

Using the same tennis data, find the number of matches played by each player in each tournament. (Remember that a player can be present as both player1 or player2).

WITH appearances AS (SELECT "Player1" as name, COUNT(* ) AS count FROM aus_men GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count FROM aus_men GROUP BY "Player2")
SELECT name, SUM(count) AS games
FROM appearances
GROUP BY name
ORDER BY games DESC;

Who has played the most matches total in all of US Open, AUST Open, French Open? Answer this both for men and women.

WITH appearances AS ( SELECT "Player1" as name, COUNT(* ) AS count FROM aus_men GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count FROM aus_men GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count FROM aus_women GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count FROM aus_women GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count FROM us_men GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count FROM us_men GROUP BY "Player2"
UNION ALL
SELECT "Player 1" as name, COUNT(* ) AS count FROM us_women GROUP BY "Player 1"
UNION ALL
SELECT "Player 2" as name, COUNT(* ) AS count FROM us_women GROUP BY "Player 2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count FROM wimb_men GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count FROM wimb_men GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count FROM wimb_women GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count FROM wimb_women GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count FROM french_men GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count FROM french_men GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count FROM french_women GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count FROM french_women GROUP BY "Player2")
SELECT name, SUM(count) AS games
FROM appearances
GROUP BY name
ORDER BY games DESC;

From the set of all men and women, we see that the most matches played goes to Rafael Nadal with 28.

Who has the highest first serve percentage? (Just the maximum value in a single match.)

WITH serving AS (
SELECT "Player1" as name, "FSP.1" AS first_serve FROM aus_men
UNION ALL
SELECT "Player2" as name, "FSP.2" AS first_serve FROM aus_men
UNION ALL
SELECT "Player1" as name, "FSP.1" AS first_serve FROM aus_women
UNION ALL
SELECT "Player2" as name, "FSP.2" AS first_serve FROM aus_women
UNION ALL
SELECT "Player1" as name, "FSP.1" AS first_serve FROM us_men
UNION ALL
SELECT "Player2" as name, "FSP.2" AS first_serve FROM us_men
UNION ALL
SELECT "Player 1" as name, "FSP.1" AS first_serve FROM us_women
UNION ALL
SELECT "Player 2" as name, "FSP.2" AS first_serve FROM us_women
UNION ALL
SELECT "Player1" as name, "FSP.1" AS first_serve FROM wimb_men
UNION ALL
SELECT "Player2" as name, "FSP.2" AS first_serve FROM wimb_men
UNION ALL
SELECT "Player1" as name, "FSP.1" AS first_serve FROM wimb_women
UNION ALL
SELECT "Player2" as name, "FSP.2" AS first_serve FROM wimb_women
UNION ALL
SELECT "Player1" as name, "FSP.1" AS first_serve FROM french_men
UNION ALL
SELECT "Player2" as name, "FSP.2" AS first_serve FROM french_men
UNION ALL
SELECT "Player1" as name, "FSP.1" AS first_serve FROM french_women
UNION ALL
SELECT "Player2" as name, "FSP.2" AS first_serve FROM french_women)
SELECT name, first_serve
FROM serving
ORDER BY first_serve DESC;

The highest first serve percentage in a match was 93% by Sara Errani.

What are the unforced error percentages of the top three players with the most wins? (Unforced error percentage is % of points lost due to unforced errors. In a match, you have fields for number of points won by each player, and number of unforced errors for each field.)

WITH errors AS (
SELECT "Player1" as name, COUNT(* ) AS count, SUM("TPW.2") as points_lost, SUM("UFE.1") as errors FROM aus_men WHERE "Result"=1 GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count, SUM("TPW.1") as points_lost, SUM("UFE.2") as errors FROM aus_men WHERE "Result"=0 GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count, SUM("TPW.2") as points_lost, SUM("UFE.1") as errors FROM aus_women WHERE "Result"=1 GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count, SUM("TPW.1") as points_lost, SUM("UFE.2") as errors FROM aus_women WHERE "Result"=0 GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count, SUM("TPW.2") as points_lost, SUM("UFE.1") as errors FROM us_men WHERE "Result"=1 GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count, SUM("TPW.1") as points_lost, SUM("UFE.2") as errors FROM us_men WHERE "Result"=0 GROUP BY "Player2"
UNION ALL
SELECT "Player 1" as name, COUNT(* ) AS count, SUM("TPW.2") as points_lost, SUM("UFE.1") as errors FROM us_women WHERE "Result"=1 GROUP BY "Player 1"
UNION ALL
SELECT "Player 2" as name, COUNT(* ) AS count, SUM("TPW.1") as points_lost, SUM("UFE.2") as errors FROM us_women WHERE "Result"=0 GROUP BY "Player 2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count, SUM("TPW.2") as points_lost, SUM("UFE.1") as errors FROM wimb_men WHERE "Result"=1 GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count, SUM("TPW.1") as points_lost, SUM("UFE.2") as errors FROM wimb_men WHERE "Result"=0 GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count, SUM("TPW.2") as points_lost, SUM("UFE.1") as errors FROM wimb_women WHERE "Result"=1 GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count, SUM("TPW.1") as points_lost, SUM("UFE.2") as errors FROM wimb_women WHERE "Result"=0 GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count, SUM("TPW.2") as points_lost, SUM("UFE.1") as errors FROM french_men WHERE "Result"=1 GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count, SUM("TPW.1") as points_lost, SUM("UFE.2") as errors FROM french_men WHERE "Result"=0 GROUP BY "Player2"
UNION ALL
SELECT "Player1" as name, COUNT(* ) AS count, SUM("TPW.2") as points_lost, SUM("UFE.1") as errors FROM french_women WHERE "Result"=1 GROUP BY "Player1"
UNION ALL
SELECT "Player2" as name, COUNT(* ) AS count, SUM("TPW.1") as points_lost, SUM("UFE.2") as errors FROM french_women WHERE "Result"=0 GROUP BY "Player2")
SELECT name, SUM(count) AS total_wins, SUM(errors)/SUM(points_lost) AS unforced_error_percent
FROM errors
GROUP BY name
ORDER BY total_wins DESC;

The top three in wins (Nadal, Ferrer, and Wawrinka) had unforced error percentages of 23.5, 23.5, 26.8, respectively.

Hint: SUM(double_faults) sums the contents of an entire column. For each row, to add the field values from two columns, the syntax SELECT name, double_faults + unforced_errors can be used.

Special bonus hint: To be careful about handling possible ties, consider using rank functions.

